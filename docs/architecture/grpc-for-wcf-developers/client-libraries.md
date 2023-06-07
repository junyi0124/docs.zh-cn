---
title: 创建 gRPC 客户端库-WCF 开发人员 gRPC
description: 针对 gRPC services 的共享客户端库/包的讨论。
ms.date: 01/06/2021
ms.openlocfilehash: c55b6d1da2377af0b687e32e7776f12b96b0a2ba
ms.sourcegitcommit: 7ef96827b161ef3fcde75f79d839885632e26ef1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/07/2021
ms.locfileid: "97970123"
---
# <a name="create-grpc-client-libraries"></a>创建 gRPC 客户端库

不需要为 gRPC 应用程序分发客户端库。 你可以在组织中创建文件的共享库 `.proto` ，并且其他团队可以使用这些文件在其自己的项目中生成客户端代码。 但是，如果你有一个专用 NuGet 存储库，并且许多其他团队正在使用 .NET，则可以创建并发布客户端 NuGet 包作为你的服务项目的一部分。 这种方法可以是共享和升级服务的好方法。

分发客户端库的一个优点是，您可以使用有用的 "方便性" 方法和属性来增强生成的 gRPC 和 Protobuf 类。 在客户端代码中，与在服务器中一样，所有类都声明为 `partial` ，因此你可以在不编辑生成的代码的情况下扩展它们。 此行为意味着可以轻松地将构造函数、方法和计算属性添加到基本类型。

> [!CAUTION]
> 不应使用自定义代码来提供基本功能。 您不希望将这一基本功能限制为使用共享库的 .NET 团队，而不是将其提供给使用其他语言或平台（如 Python 或 Java）的团队。

请确保尽可能多的团队可以访问你的 gRPC 服务。 实现此功能的最佳方式是共享 `.proto` 文件，使开发人员能够生成自己的客户端。 这种方法在多平台环境中尤其如此，在这种环境中，不同的团队常常使用不同的编程语言和框架，或者 API 可从外部访问。

## <a name="useful-extensions"></a>有用的扩展

.NET 中有两种常用接口用于处理对象的流： <xref:System.Collections.Generic.IEnumerable%601> 和 <xref:System.IObservable%601> 。 从 .NET Core 3.0 和 c # 8.0 开始，有一个 <xref:System.Collections.Generic.IAsyncEnumerable%601> 用于异步处理流的接口，以及一个 `await foreach` 使用接口的语法。 本部分提供可重用的代码，用于将这些接口应用到 gRPC 流。

使用 .NET gRPC 客户端库，可使用的 `ReadAllAsync` 扩展方法 `IAsyncStreamReader<T>` 创建 `IAsyncEnumerable<T>` 接口。 对于使用反应性编程的开发人员而言，创建接口的等效扩展方法 `IObservable<T>` 可能类似于以下部分中的示例。

### <a name="iobservable"></a>IObservable

`IObservable<T>`接口是的 "事后" 反转 `IEnumerable<T>` 。 反应方法不会从流中提取项，而是允许流将项推送到订阅服务器。 此行为与 gRPC 流非常类似，并且可以轻松地绕过接口的 `IObservable<T>` 接口 `IAsyncStreamReader<T>` 。

此代码比 `IAsyncEnumerable<T>` 代码长，因为 c # 不支持使用可观察量。 必须手动创建实现类。 但它是一个泛型类，因此单个实现适用于所有类型。

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using System.Threading.Tasks;

namespace Grpc.Core
{
    public class GrpcStreamObservable<T> : IObservable<T>
    {
        private readonly IAsyncStreamReader<T> _reader;
        private readonly CancellationToken _token;
        private int _used;

        public GrpcStreamObservable(IAsyncStreamReader<T> reader, CancellationToken token = default)
        {
            _reader = reader;
            _token = token;
            _used = 0;
        }

        public IDisposable Subscribe(IObserver<T> observer) =>
            Interlocked.Exchange(ref _used, 1) == 0
                ? new GrpcStreamSubscription(_reader, observer, _token)
                : throw new InvalidOperationException("Subscribe can only be called once.");

    }
}
```

> [!IMPORTANT]
> 此可观察实现允许 `Subscribe` 只调用一次方法，因为尝试从流中读取多个订阅服务器会导致混乱。 有一些运算符（例如 `Replay` ，在可观察量[](https://www.nuget.org/packages/System.Reactive.Linq)中）支持缓冲和可重复共享的，可与此实现一起使用。

`GrpcStreamSubscription`类处理的枚举 `IAsyncStreamReader` ：

```csharp
public class GrpcStreamSubscription : IDisposable
{
    private readonly Task _task;
    private readonly CancellationTokenSource _tokenSource;
    private bool _completed;

    public GrpcStreamSubscription(IAsyncStreamReader<T> reader, IObserver<T> observer, CancellationToken token)
    {
        Debug.Assert(reader != null);
        Debug.Assert(observer != null);
        _tokenSource = new CancellationTokenSource();
        token.Register(_tokenSource.Cancel);
        _task = Run(reader, observer, _tokenSource.Token);
    }

    private async Task Run(IAsyncStreamReader<T> reader, IObserver<T> observer, CancellationToken token)
    {
        while (!token.IsCancellationRequested)
        {
            try
            {
                if (!await reader.MoveNext(token)) break;
            }
            catch (RpcException e) when (e.StatusCode == Grpc.Core.StatusCode.NotFound)
            {
                break;
            }
            catch (OperationCanceledException)
            {
                break;
            }
            catch (Exception e)
            {
                observer.OnError(e);
                _completed = true;
                return;
            }

            observer.OnNext(reader.Current);
        }

        _completed = true;
        observer.OnCompleted();
    }

    public void Dispose()
    {
        if (!_completed && !_tokenSource.IsCancellationRequested)
        {
            _tokenSource.Cancel();
        }

        _tokenSource.Dispose();
        _task.Dispose();
    }

}
```

现在只需要一个简单的扩展方法，用于从流读取器创建可观察的。

```csharp
using System;
using System.Diagnostics;
using System.Threading;
using System.Threading.Tasks;

namespace Grpc.Core
{
    public static class AsyncStreamReaderObservableExtensions
    {
        public static IObservable<T> AsObservable<T>(this IAsyncStreamReader<T> reader) =>
            new GrpcStreamObservable<T>(reader);
    }
}
```

## <a name="summary"></a>总结

<xref:System.Collections.Generic.IAsyncEnumerable%601>和 <xref:System.IObservable%601> 模型都是支持良好的，并以详细的方式记录了如何处理 .net 中的数据数据流。 gRPC 流可以很好地映射到这两个范例，提供与 .NET 的紧密集成，并提供反应性和异步编程风格。

>[!div class="step-by-step"]
>[上一页](streaming-versus-repeated.md)
>[下一页](security.md)
