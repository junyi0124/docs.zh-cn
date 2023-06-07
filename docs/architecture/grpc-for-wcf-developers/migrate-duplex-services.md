---
title: 将 WCF 双工服务迁移到 WCF 开发人员的 gRPC-gRPC
description: 了解如何将各种形式的 WCF 双工服务迁移到 gRPC 流式处理服务。
ms.date: 12/15/2020
ms.openlocfilehash: 4741e5ad5c12fa358853381d4f2c286add14f8f5
ms.sourcegitcommit: 655f8a16c488567dfa696fc0b293b34d3c81e3df
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/06/2021
ms.locfileid: "97938021"
---
# <a name="migrate-wcf-duplex-services-to-grpc"></a>将 WCF 双工服务迁移到 gRPC

现在，你已了解基本概念，在此部分中，你将了解更复杂的 *流式处理* gRPC 服务。

有多种方法可在 Windows Communication Foundation (WCF) 中使用双工服务。 某些服务由客户端启动，然后从服务器流式传输数据。 其他全双工服务可能涉及更多的双向通信，例如 WCF 文档中的经典计算器示例。 本章将使用两个可能的 WCF 股票行情来实现并将其迁移到 gRPC：一个使用服务器流 RPC，另一个使用双向流式处理 RPC。

## <a name="server-streaming-rpc"></a>服务器流式处理 RPC

在 [示例 SIMPLESTOCKTICKER WCF 解决方案](https://github.com/dotnet-architecture/grpc-for-wcf-developers/tree/master/SimpleStockTickerSample/wcf/SimpleStockTicker)SimpleStockPriceTicker 中，有一个双工服务，该服务的客户端将与股票符号列表建立连接，而服务器使用 *回调接口* 在更新可用时发送更新。 客户端实现该接口以响应来自服务器的调用。

### <a name="the-wcf-solution"></a>WCF 解决方案

WCF 解决方案实现为 .NET Framework 4 中自承载的 Net.tcp 服务器。*x* 控制台应用程序。

#### <a name="servicecontract"></a>ServiceContract

```csharp
[ServiceContract(SessionMode = SessionMode.Required, CallbackContract = typeof(ISimpleStockTickerCallback))]
public interface ISimpleStockTickerService
{
    [OperationContract(IsOneWay = true)]
    void Subscribe(string[] symbols);
}
```

服务具有不具有返回类型的单个方法，因为它使用回调接口 `ISimpleStockTickerCallback` 将数据实时发送到客户端。

#### <a name="the-callback-interface"></a>回调接口

```csharp
[ServiceContract]
public interface ISimpleStockTickerCallback
{
    [OperationContract(IsOneWay = true)]
    void Update(string symbol, decimal price);
}
```

可以在解决方案中查找这些接口的实现，以及提供可提供测试数据的虚假外部依赖项。

### <a name="grpc-streaming"></a>gRPC 流式处理

用于处理实时数据的 gRPC 过程不同于 WCF 进程。 从客户端到服务器的调用可以创建一个持久性流，该流可对异步到达的消息进行监视。 尽管存在差异，但流可以是处理此数据的更直观的方式，并且在新式编程中更为相关，它强调了 LINQ、反应流、功能编程等。

服务定义需要两条消息：一个用于请求，一个用于流。 服务返回 `StockTickerUpdate` 其声明中包含关键字的消息流 `stream` `return` 。 建议你将添加 `Timestamp` 到更新，以显示价格变化的准确时间。

#### <a name="simple_stock_tickerproto"></a>simple_stock_ticker proto

```protobuf
syntax = "proto3";

option csharp_namespace = "TraderSys.SimpleStockTickerServer.Protos";

import "google/protobuf/timestamp.proto";

package SimpleStockTickerServer;

service SimpleStockTicker {
  rpc Subscribe (SubscribeRequest) returns (stream StockTickerUpdate);
}

message SubscribeRequest {
  repeated string symbols = 1;
}

message StockTickerUpdate {
  string symbol = 1;
  int32 price_cents = 2;
  google.protobuf.Timestamp time = 3;
}
```

### <a name="implement-simplestockticker"></a>实现 SimpleStockTicker

通过将类库中的 `StockPriceSubscriber` 三个类复制 `TraderSys.StockMarket` 到目标解决方案中的新 .NET Standard 类库中，重用 WCF 项目中的虚设。 为了更好地遵循最佳做法，请添加一个 `Factory` 类型来创建该类型的实例，并向 `IStockPriceSubscriberFactory` ASP.NET Core 依赖关系注入服务注册。

#### <a name="the-factory-implementation"></a>工厂实现

```csharp
public interface IStockPriceSubscriberFactory
{
    IStockPriceSubscriber GetSubscriber(string[] symbols);
}

public class StockPriceSubscriberFactory : IStockPriceSubscriberFactory
{
    public IStockPriceSubscriber GetSubscriber(string[] symbols)
    {
        return new StockPriceSubscriber(symbols);
    }
}
```

#### <a name="register-the-factory"></a>注册工厂

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton<IStockPriceSubscriberFactory, StockPriceSubscriberFactory>();
}
```

此类现可用于实现 gRPC `StockTickerService` 。

#### <a name="stocktickerservicecs"></a>StockTickerService.cs

```csharp
public class StockTickerService : Protos.SimpleStockTicker.SimpleStockTickerBase
{
    private readonly IStockPriceSubscriberFactory _subscriberFactory;

    public StockTickerService(IStockPriceSubscriberFactory subscriberFactory)
    {
        _subscriberFactory = subscriberFactory;
    }

    public override async Task Subscribe(SubscribeRequest request, IServerStreamWriter<StockTickerUpdate> responseStream, ServerCallContext context)
    {
        var subscriber = _subscriberFactory.GetSubscriber(request.Symbols.ToArray());

        subscriber.Update += async (sender, args) =>
            await WriteUpdateAsync(responseStream, args.Symbol, args.Price);

        await AwaitCancellation(context.CancellationToken);
    }

    private async Task WriteUpdateAsync(IServerStreamWriter<StockTickerUpdate> stream, string symbol, decimal price)
    {
        try
        {
            await stream.WriteAsync(new StockTickerUpdate
            {
                Symbol = symbol,
                PriceCents = (int)(price * 100),
                Time = Timestamp.FromDateTimeOffset(DateTimeOffset.UtcNow)
            });
        }
        catch (Exception e)
        {
            // Handle any errors caused by broken connection, etc.
            _logger.LogError($"Failed to write message: {e.Message}");
        }
    }

    private static Task AwaitCancellation(CancellationToken token)
    {
        var completion = new TaskCompletionSource<object>();
        token.Register(() => completion.SetResult(null));
        return completion.Task;
    }
}
```

正如您所看到的，尽管文件中的声明 `.proto` 表明方法返回 `StockTickerUpdate` 消息流，但实际上返回了 `Task` 。 创建流的作业由生成的代码和 gRPC 运行时库处理，后者提供了 `IServerStreamWriter<StockTickerUpdate>` 可供使用的响应流。

不同于 WCF 双工服务，其中服务类的实例在连接打开时保持活动状态，gRPC 服务使用返回的任务使服务保持活动状态。 直到连接关闭，该任务才会完成。

服务可以通过使用中的，来确定客户端何时关闭了连接 `CancellationToken` `ServerCallContext` 。 简单静态方法 `AwaitCancellation` 用于创建在取消标记时完成的任务。

在方法中， `Subscribe` 获取 `StockPriceSubscriber` 并添加一个写入响应流的事件处理程序。 然后，等待连接关闭，然后立即释放， `subscriber` 以防其尝试将数据写入到关闭的流。

`WriteUpdateAsync`方法具有一个 `try` / `catch` 块，用于处理将消息写入流时可能发生的任何错误。 在通过网络进行的持久连接中，这一考虑因素很重要，无论是有意还是因某个原因发生故障，都可以在任何毫秒中断。

### <a name="use-stocktickerservice-from-a-client-application"></a>使用客户端应用程序中的 StockTickerService

按照上一部分中的相同步骤，从文件创建可共享的客户端类库 `.proto` 。 在示例中，有一个 .NET 控制台应用程序，它演示了如何使用客户端。

#### <a name="example-programcs"></a>示例 Program.cs

```csharp
class Program
{
    static async Task Main(string[] args)
    {
        using var channel = GrpcChannel.ForAddress("https://localhost:5001");
        var client = new SimpleStockTicker.SimpleStockTickerClient(channel);

        var request = new SubscribeRequest();
        request.Symbols.AddRange(args);

        using var stream = client.Subscribe(request);

        var tokenSource = new CancellationTokenSource();

        var task = DisplayAsync(stream.ResponseStream, tokenSource.Token);

        WaitForExitKey();

        tokenSource.Cancel();
        await task;
    }
}
```

在这种情况下， `Subscribe` 生成的客户端上的方法不是异步的。 由于其方法是异步的，并且首次调用它时，它将不再 `MoveNext` 完成，直到连接处于活动状态，才能立即创建和使用流。

流将传递给异步 `DisplayAsync` 方法。 然后，应用程序会等待用户按下某个键，然后取消 `DisplayAsync` 方法并等待任务完成，然后再退出。

> [!NOTE]
> 此代码使用新的 c # 8 `using` 声明语法，以释放该方法退出时的流和信道 `Main` 。 这是一小小的更改，但也有一小部分会减少缩进和空行。

#### <a name="consume-the-stream"></a>使用流

WCF 使用回调接口允许服务器直接在客户端上调用方法。 gRPC 流的工作方式不同。 客户端循环访问返回的流并处理消息，就像它们是从返回的本地方法返回的一样 `IEnumerable` 。

`IAsyncStreamReader<T>`类型的工作方式非常类似于 `IEnumerator<T>` 。 `MoveNext`如果有更多数据，并且有 `Current` 返回最新值的属性，则有方法返回 true。 唯一的区别是， `MoveNext` 方法只返回一个 `Task<bool>` 而不是一个 `bool` 。 `ReadAllAsync`扩展方法在 `IAsyncEnumerable` 可与新语法一起使用的标准 c # 8 中包装流 `await foreach` 。

```csharp
static async Task DisplayAsync(IAsyncStreamReader<StockTickerUpdate> stream, CancellationToken token)
{
    try
    {
        await foreach (var update in stream.ReadAllAsync(token))
        {
            Console.WriteLine($"{update.Symbol}: {update.Price}");
        }
    }
    catch (RpcException e) when (e.StatusCode == StatusCode.Cancelled)
    {
        return;
    }
    catch (OperationCanceledException)
    {
        Console.WriteLine("Finished.");
    }
}
```

> [!TIP]
> 对于使用反应性编程模式的开发人员，本章末尾的 [客户端库](client-libraries.md#iobservable) 部分显示了如何添加扩展方法和类以包装 `IAsyncStreamReader<T>` 在中 `IObservable<T>` 。

同样，请务必在此处捕获异常，因为可能存在网络故障，并由于 <xref:System.OperationCanceledException> 代码使用来中断循环而导致不可避免引发的异常 <xref:System.Threading.CancellationToken> 。 该 `RpcException` 类型包含有关 gRPC 运行时错误的大量有用信息，包括 `StatusCode` 。 有关详细信息，请参阅 [第4章中的 *错误处理*](error-handling.md)。

## <a name="bidirectional-streaming"></a>双向流式处理

WCF 全双工服务允许在两个方向上进行异步实时消息传送。 在服务器流式处理示例中，客户端启动请求，然后接收一个更新流。 该服务的更好版本可让客户端在列表中添加和删除股票，而不必停止并创建新的订阅。 已在 [FullStockTicker 示例解决方案](https://github.com/dotnet-architecture/grpc-for-wcf-developers/tree/master/FullStockTickerSample/wcf/FullStockTicker)中实现了该功能。

`IFullStockTickerService`接口提供了三种方法：

- `Subscribe` 启动连接。
- `AddSymbol` 添加要观看的股票符号。
- `RemoveSymbol` 从监视列表中删除符号。

```csharp
[ServiceContract(SessionMode = SessionMode.Required, CallbackContract = typeof(IFullStockTickerCallback))]
public interface IFullStockTickerService
{
    [OperationContract(IsOneWay = true)]
    void Subscribe();

    [OperationContract(IsOneWay = true)]
    void AddSymbol(string symbol);

    [OperationContract(IsOneWay = true)]
    void RemoveSymbol(string symbol);
}
```

回调接口保持不变。

在 gRPC 中实现此模式不是很简单，因为现在有两个数据流，其中一条消息传递到服务器，另一个从服务器到客户端。 不能使用多种方法来实现添加和删除操作，但可以通过使用类型或关键字在单个流上传递多种类型的消息 `Any` `oneof` ，这在 [第3章](protobuf-any-oneof.md)中进行了介绍。

如果有一组可接受的特定类型， `oneof` 则是更好的方法。 使用 `ActionMessage` 可保存 `AddSymbolRequest` 或的 `RemoveSymbolRequest` ：

```protobuf
message ActionMessage {
  oneof action {
    AddSymbolRequest add = 1;
    RemoveSymbolRequest remove = 2;
  }
}

message AddSymbolRequest {
  string symbol = 1;
}

message RemoveSymbolRequest {
  string symbol = 1;
}
```

声明一个双向流式处理服务，该服务采用 `ActionMessage` 消息流：

```protobuf
service FullStockTicker {
  rpc Subscribe (stream ActionMessage) returns (stream StockTickerUpdate);
}
```

此服务的实现与上一个示例的实现类似，不同之处在于方法的第一个参数 `Subscribe` 现在是 `IAsyncStreamReader<ActionMessage>` ，可用于处理 `Add` 和 `Remove` 请求：

```csharp
public override async Task Subscribe(IAsyncStreamReader<ActionMessage> requestStream, IServerStreamWriter<StockTickerUpdate> responseStream, ServerCallContext context)
{
    using var subscriber = _subscriberFactory.GetSubscriber();

    subscriber.Update += async (sender, args) =>
        await WriteUpdateAsync(responseStream, args.Symbol, args.Price);

    var actionsTask = HandleActions(requestStream, subscriber, context.CancellationToken);

    _logger.LogInformation("Subscription started.");
    await AwaitCancellation(context.CancellationToken);

    try { await actionsTask; } catch { /* Ignored */ }

    _logger.LogInformation("Subscription finished.");
}

private async Task WriteUpdateAsync(IServerStreamWriter<StockTickerUpdate> stream, string symbol, decimal price)
{
    try
    {
        await stream.WriteAsync(new StockTickerUpdate
        {
            Symbol = symbol,
            PriceCents = (int)(price * 100),
            Time = Timestamp.FromDateTimeOffset(DateTimeOffset.UtcNow)
        });
    }
    catch (Exception e)
    {
        // Handle any errors caused by broken connection, etc.
        _logger.LogError($"Failed to write message: {e.Message}");
    }
}

private static Task AwaitCancellation(CancellationToken token)
{
    var completion = new TaskCompletionSource<object>();
    token.Register(() => completion.SetResult(null));
    return completion.Task;
}
```

`ActionMessage`GRPC 已生成的类保证只能 `Add` 设置和属性中的一个 `Remove` 。 找出哪种 `null` 方法是确定使用哪种类型的消息的有效方法，但有一种更好的方法。 代码生成还在类中创建了，如下所示 `enum ActionOneOfCase` `ActionMessage` ：

```csharp
public enum ActionOneofCase {
    None = 0,
    Add = 1,
    Remove = 2,
}
```

`ActionCase`对象上的属性 `ActionMessage` 可与语句一起使用 `switch` ，以确定要设置的字段：

```csharp
private async Task HandleActions(IAsyncStreamReader<ActionMessage> requestStream, IFullStockPriceSubscriber subscriber, CancellationToken token)
{
    await foreach (var action in requestStream.ReadAllAsync(token))
    {
        switch (action.ActionCase)
        {
            case ActionMessage.ActionOneofCase.None:
                _logger.LogWarning("No Action specified.");
                break;
            case ActionMessage.ActionOneofCase.Add:
                subscriber.Add(action.Add.Symbol);
                break;
            case ActionMessage.ActionOneofCase.Remove:
                subscriber.Remove(action.Remove.Symbol);
                break;
            default:
                _logger.LogWarning($"Unknown Action '{action.ActionCase}'.");
                break;
        }
    }
}
```

> [!TIP]
> `switch` `default` 如果遇到未知值，则该语句有一个记录警告的情况 `ActionOneOfCase` 。 这可用于指示客户端正在使用 `.proto` 已添加更多操作的更高版本的文件。 这是使用 `switch` 比 `null` 在已知字段上测试更好的原因之一。

### <a name="use-fullstocktickerservice-from-a-client-application"></a>使用客户端应用程序中的 FullStockTickerService

有一个简单的 .NET WPF 应用程序，演示如何使用更复杂的客户端。 可在 [GitHub](https://github.com/dotnet-architecture/grpc-for-wcf-developers/tree/master/FullStockTickerSample/grpc/FullStockTicker)上找到完整的应用程序。

在类中使用客户端 `MainWindowViewModel` ，它 `FullStockTicker.FullStockTickerClient` 从依赖关系注入中获取类型的实例：

```csharp
public class MainWindowViewModel : IAsyncDisposable, INotifyPropertyChanged
{
    private readonly FullStockTicker.FullStockTickerClient _client;
    private readonly AsyncDuplexStreamingCall<ActionMessage, StockTickerUpdate> _duplexStream;
    private readonly CancellationTokenSource _cancellationTokenSource;
    private readonly Task _responseTask;
    private string _addSymbol;

    public MainWindowViewModel(FullStockTicker.FullStockTickerClient client)
    {
        _cancellationTokenSource = new CancellationTokenSource();
        _client = client;
        _duplexStream = _client.Subscribe();
        _responseTask = HandleResponsesAsync(_cancellationTokenSource.Token);

        AddCommand = new AsyncCommand(Add, CanAdd);
    }
```

方法返回的对象 `client.Subscribe()` 现在是 gRPC 库类型的一个实例 `AsyncDuplexStreamingCall<TRequest, TResponse>` ，它提供 `RequestStream` 用于将请求发送到服务器的和 `ResponseStream` 用于处理响应的。

从一些 WPF 方法使用请求流 `ICommand` 来添加和删除符号。 对于每个操作，请在对象上设置相关字段 `ActionMessage` ：

```csharp
private async Task Add()
{
    if (CanAdd())
    {
        await _duplexStream.RequestStream.WriteAsync(new ActionMessage {Add = new AddSymbolRequest {Symbol = AddSymbol}});
    }
}

public async Task Remove(PriceViewModel priceViewModel)
{
    await _duplexStream.RequestStream.WriteAsync(new ActionMessage {Remove = new RemoveSymbolRequest {Symbol = priceViewModel.Symbol}});
    Prices.Remove(priceViewModel);
}
```

> [!IMPORTANT]
> 如果 `oneof` 对某一消息设置字段值，则会自动清除以前设置的任何字段。

在方法中处理响应流 `async` 。 `Task`当窗口关闭时，将保留它返回的内容：

```csharp
private async Task HandleResponsesAsync(CancellationToken token)
{
    var stream = _duplexStream.ResponseStream;

    try
    {
        await foreach (var update in stream.ReadAllAsync(token))
        {
            var price = Prices.FirstOrDefault(p => p.Symbol.Equals(update.Symbol));
            if (price == null)
            {
                price = new PriceViewModel(this) {Symbol = update.Symbol, Price = update.PriceCents / 100m};
                Prices.Add(price);
            }
            else
            {
                price.Price = update.PriceCents / 100m;
            }
        }
    }
    catch (OperationCancelledException) { }
}
```

### <a name="client-cleanup"></a>客户端清理

如果关闭窗口，并 `MainWindowViewModel` (从) 的事件中释放 `Closed` `MainWindow` ，则建议您正确释放 `AsyncDuplexStreamingCall` 对象。 具体而言， `CompleteAsync` 应调用上的方法 `RequestStream` 以正常关闭服务器上的流。 此示例显示了 `DisposeAsync` 示例视图模型中的方法：

```csharp
public async ValueTask DisposeAsync()
{
    try
    {
        await _duplexStream.RequestStream.CompleteAsync().ConfigureAwait(false);
        await _responseTask.ConfigureAwait(false);
    }
    finally
    {
        _duplexStream.Dispose();
    }
}
```

关闭请求流使服务器能够及时释放其自己的资源。 这可以提高服务的效率和可伸缩性，并可防止出现异常。

>[!div class="step-by-step"]
>[上一页](migrate-request-reply.md)
>[下一页](streaming-versus-repeated.md)
