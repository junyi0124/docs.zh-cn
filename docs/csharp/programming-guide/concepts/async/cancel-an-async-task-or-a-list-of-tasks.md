---
title: 取消任务列表 (C#)
description: 了解如何使用取消令牌向任务列表发出取消请求的信号。
ms.date: 08/19/2020
ms.topic: tutorial
ms.assetid: eec32dbb-70ea-4c88-bd27-fa2e34546914
ms.openlocfilehash: 79c9db53674182489c89d657786bf39e8bb44b21
ms.sourcegitcommit: 636af37170ae75a11c4f7d1ecd770820e7dfe7bd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/07/2020
ms.locfileid: "91805247"
---
# <a name="cancel-a-list-of-tasks-c"></a>取消任务列表 (C#)

如果不想等待异步控制台应用程序完成，可以取消该应用程序。 通过遵循本主题中的示例，可将取消添加到下载网站内容的应用程序。 可通过将 <xref:System.Threading.CancellationTokenSource> 实例与每个任务进行关联来取消多个任务。 如果选择 <kbd>Enter</kbd> 键，则将取消所有尚未完成的任务。

本教程涉及：

> [!div class="checklist"]
>
> - 创建 .NET 控制台应用程序
> - 编写支持取消的异步应用程序
> - 演示发出取消信号

## <a name="prerequisites"></a>必备条件

本教程需要的内容如下：

- [.NET 5.0 或更高版本的 SDK](https://dotnet.microsoft.com/download/dotnet/5.0)
- 集成开发环境 (IDE)
  - [建议使用 Visual Studio、Visual Studio Code 或 Visual Studio for Mac](https://visualstudio.microsoft.com)

### <a name="create-example-application"></a>创建示例应用程序

创建新的 .NET Core 控制台应用程序。 可通过使用 [`dotnet new console`](../../../../core/tools/dotnet-new.md#console) 命令或从 [Visual Studio](/visualstudio/install/install-visual-studio) 进行创建。 在你最喜欢的编辑器中打开 Program.cs 文件。

### <a name="replace-using-statements"></a>替换 using 语句

将现有 using 语句替换为以下声明：

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Net.Http;
using System.Threading;
using System.Threading.Tasks;
```

## <a name="add-fields"></a>添加字段

在 `Program` 类定义中，添加以下三个字段：

```csharp
static readonly CancellationTokenSource s_cts = new CancellationTokenSource();

static readonly HttpClient s_client = new HttpClient
{
    MaxResponseContentBufferSize = 1_000_000
};

static readonly IEnumerable<string> s_urlList = new string[]
{
    "https://docs.microsoft.com",
    "https://docs.microsoft.com/aspnet/core",
    "https://docs.microsoft.com/azure",
    "https://docs.microsoft.com/azure/devops",
    "https://docs.microsoft.com/dotnet",
    "https://docs.microsoft.com/dynamics365",
    "https://docs.microsoft.com/education",
    "https://docs.microsoft.com/enterprise-mobility-security",
    "https://docs.microsoft.com/gaming",
    "https://docs.microsoft.com/graph",
    "https://docs.microsoft.com/microsoft-365",
    "https://docs.microsoft.com/office",
    "https://docs.microsoft.com/powershell",
    "https://docs.microsoft.com/sql",
    "https://docs.microsoft.com/surface",
    "https://docs.microsoft.com/system-center",
    "https://docs.microsoft.com/visualstudio",
    "https://docs.microsoft.com/windows",
    "https://docs.microsoft.com/xamarin"
};
```

<xref:System.Threading.CancellationTokenSource> 用于向 <xref:System.Threading.CancellationToken> 发出请求取消的信号。 `HttpClient` 公开发送 HTTP 请求和接收 HTTP 响应的能力。 `s_urlList` 包括应用程序计划处理的所有 URL。

## <a name="update-application-entry-point"></a>更新应用程序入口点

控制台应用程序的主入口点是 `Main` 方法。 将现有方法替换为以下内容：

```csharp
static async Task Main()
{
    Console.WriteLine("Application started.");
    Console.WriteLine("Press the ENTER key to cancel...\n");

    Task cancelTask = Task.Run(() =>
    {
        while (Console.ReadKey().Key != ConsoleKey.Enter)
        {
            Console.WriteLine("Press the ENTER key to cancel...");
        }

        Console.WriteLine("\nENTER key pressed: cancelling downloads.\n");
        s_cts.Cancel();
    });

    Task sumPageSizesTask = SumPageSizesAsync();

    await Task.WhenAny(new[] { cancelTask, sumPageSizesTask });

    Console.WriteLine("Application ending.");
}
```

目前将已更新的 `Main` 方法视为[异步 main 方法](../../../whats-new/csharp-7.md#async-main)，这允许将异步入口点引入可执行文件中。 将几条说明性消息写入控制台，然后声明名为 `cancelTask` 的 <xref:System.Threading.Tasks.Task> 实例，这将读取控制台密钥笔画。 如果按 <kbd>Enter</kbd>，则会调用 <xref:System.Threading.CancellationTokenSource.Cancel?displayProperty=nameWithType>。 这将发出取消信号。 下一步，从 `SumPageSizesAsync` 方法分配 `sumPageSizesTask` 变量。 然后，将这两个任务传递到 <xref:System.Threading.Tasks.Task.WhenAny(System.Threading.Tasks.Task[])?displayProperty=nameWithType>，这会在完成两个任务中的任意一个时继续。

## <a name="create-the-asynchronous-sum-page-sizes-method"></a>创建异步总和页面大小方法

在 `Main` 方法下，添加 `SumPageSizesAsync` 方法：

```csharp
static async Task SumPageSizesAsync()
{
    var stopwatch = Stopwatch.StartNew();

    int total = 0;
    foreach (string url in s_urlList)
    {
        int contentLength = await ProcessUrlAsync(url, s_client, s_cts.Token);
        total += contentLength;
    }

    stopwatch.Stop();

    Console.WriteLine($"\nTotal bytes returned:  {total:#,#}");
    Console.WriteLine($"Elapsed time:          {stopwatch.Elapsed}\n");
}
```

该方法从实例化和启动 <xref:System.Diagnostics.Stopwatch> 开始。 然后会在 `s_urlList` 的每个 URL 中进行循环，并调用 `ProcessUrlAsync`。 对于每次迭代，`s_cts.Token` 都会传递到 `ProcessUrlAsync` 方法中，并且代码将返回 <xref:System.Threading.Tasks.Task%601>，其中 `TResult` 是一个整数：

```csharp
int total = 0;
foreach (string url in s_urlList)
{
    int contentLength = await ProcessUrlAsync(url, s_client, s_cts.Token);
    total += contentLength;
}
```

## <a name="add-process-method"></a>添加进程方法

在 `SumPageSizesAsync` 方法下添加以下 `ProcessUrlAsync` 方法：

```csharp
static async Task<int> ProcessUrlAsync(string url, HttpClient client, CancellationToken token)
{
    HttpResponseMessage response = await client.GetAsync(url, token);
    byte[] content = await response.Content.ReadAsByteArrayAsync();
    Console.WriteLine($"{url,-60} {content.Length,10:#,#}");

    return content.Length;
}
```

对于任何给定的 URL，该方法都将使用提供的 `client` 实例以 `byte[]` 形式来获取响应。 <xref:System.Threading.CancellationToken> 实例会传递到 <xref:System.Net.Http.HttpClient.GetAsync(System.String,System.Threading.CancellationToken)?displayProperty=nameWithType> 和 <xref:System.Net.Http.HttpContent.ReadAsByteArrayAsync?displayProperty=nameWithType> 方法中。 `token` 用于注册请求取消。 将 URL 和长度写入控制台后会返回该长度。

### <a name="example-application-output"></a>示例应用程序输出

```console
Application started.
Press the ENTER key to cancel...

https://docs.microsoft.com                                       37,357
https://docs.microsoft.com/aspnet/core                           85,589
https://docs.microsoft.com/azure                                398,939
https://docs.microsoft.com/azure/devops                          73,663
https://docs.microsoft.com/dotnet                                67,452
https://docs.microsoft.com/dynamics365                           48,582
https://docs.microsoft.com/education                             22,924

ENTER key pressed: cancelling downloads.

Application ending.
```

## <a name="complete-example"></a>完整示例

下列代码是示例的 Program.cs 文件的完整文本。

:::code language="csharp" source="snippets/cancel-tasks/cancel-tasks/Program.cs":::

## <a name="see-also"></a>另请参阅

- <xref:System.Threading.CancellationToken>
- <xref:System.Threading.CancellationTokenSource>
- [使用 Async 和 Await 的异步编程 (C#)](index.md)

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [在一段时间后取消异步任务 (C#)](cancel-async-tasks-after-a-period-of-time.md)
