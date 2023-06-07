---
title: 在异步任务完成时对其进行处理
description: 此示例演示如何使用 C# 中的 Task.WhenAny 启动多个任务并在完成时处理其结果，而不是按照它们的启动顺序进行处理。
ms.date: 08/19/2020
ms.assetid: 25331850-35a7-43b3-ab76-3908e4346b9d
ms.openlocfilehash: 860e94a9c3973ce56e7321741a1136f752aa3d18
ms.sourcegitcommit: 636af37170ae75a11c4f7d1ecd770820e7dfe7bd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/07/2020
ms.locfileid: "91805234"
---
# <a name="process-asynchronous-tasks-as-they-complete-c"></a>在异步任务完成时对其进行处理 (C#)

通过使用 <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType>，可同时启动多个任务，并在它们完成时逐个对它们进行处理，而不是按照它们的启动顺序进行处理。

下面的示例使用查询来创建一组任务。 每个任务都下载指定网站的内容。 在对 while 循环的每次迭代中，对 <xref:System.Threading.Tasks.Task.WhenAny%2A> 的等待调用返回任务集合中首先完成下载的任务。 此任务从集合中删除并进行处理。 循环重复进行，直到集合中不包含任何任务。

## <a name="create-example-application"></a>创建示例应用程序

创建新的 .NET Core 控制台应用程序。 可使用 [dotnet new console](../../../../core/tools/dotnet-new.md#console) 命令或 [Visual Studio](/visualstudio/install/install-visual-studio) 进行创建。 在你最喜欢的编辑器中打开 Program.cs 文件。

### <a name="replace-using-statements"></a>替换 using 语句

将现有 using 语句替换为以下声明：

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Net.Http;
using System.Threading.Tasks;
```

## <a name="add-fields"></a>添加字段

在 `Program` 类定义中，添加以下两个字段：

```csharp
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

`HttpClient` 公开发送 HTTP 请求和接收 HTTP 响应的能力。 `s_urlList` 包括应用程序计划处理的所有 URL。

## <a name="update-application-entry-point"></a>更新应用程序入口点

控制台应用程序的主入口点是 `Main` 方法。 将现有方法替换为以下内容：

```csharp
static Task Main() => SumPageSizesAsync();
```

目前将已更新的 `Main` 方法视为[异步 main 方法](../../../whats-new/csharp-7.md#async-main)，这允许将异步入口点引入可执行文件中。 它表示对 `SumPageSizesAsync` 的调用。

## <a name="create-the-asynchronous-sum-page-sizes-method"></a>创建异步总和页面大小方法

在 `Main` 方法下，添加 `SumPageSizesAsync` 方法：

```csharp
static async Task SumPageSizesAsync()
{
    var stopwatch = Stopwatch.StartNew();

    IEnumerable<Task<int>> downloadTasksQuery =
        from url in s_urlList
        select ProcessUrlAsync(url, s_client);

    List<Task<int>> downloadTasks = downloadTasksQuery.ToList();

    int total = 0;
    while (downloadTasks.Any())
    {
        Task<int> finishedTask = await Task.WhenAny(downloadTasks);
        downloadTasks.Remove(finishedTask);
        total += await finishedTask;
    }

    stopwatch.Stop();

    Console.WriteLine($"\nTotal bytes returned:  {total:#,#}");
    Console.WriteLine($"Elapsed time:          {stopwatch.Elapsed}\n");
}
```

该方法从实例化和启动 <xref:System.Diagnostics.Stopwatch> 开始。 然后它会包含一个查询，执行此查询时，将创建任务集合。 每次对以下代码中的 `ProcessUrlAsync` 进行调用都会返回 <xref:System.Threading.Tasks.Task%601>，其中 `TResult` 是一个整数：

```csharp
IEnumerable<Task<int>> downloadTasksQuery =
    from url in s_urlList
    select ProcessUrlAsync(url, s_client);
```

由于 LINQ 的[延迟执行](../../../../standard/linq/deferred-execution-example.md)，因此可调用 <xref:System.Linq.Enumerable.ToList%2A?displayProperty=nameWithType> 来启动每个任务。

```csharp
List<Task<int>> downloadTasks = downloadTasksQuery.ToList();
```

`while` 循环针对集合中的每个任务执行以下步骤：

1. 等待调用 `WhenAny`，以标识集合中首个已完成下载的任务。

    ```csharp
    Task<int> firstFinishedTask = await Task.WhenAny(downloadTasks);
    ```

1. 从集合中移除任务。

    ```csharp
    downloadTasks.Remove(firstFinishedTask);
    ```

1. 等待 `finishedTask`，由对 `ProcessUrlAsync` 的调用返回。 `finishedTask` 变量是 <xref:System.Threading.Tasks.Task%601>，其中 `TResult` 是整数。 任务已完成，但需等待它检索已下载网站的长度，如以下示例所示。 如果任务出错，`await` 将引发存储在 `AggregateException` 中的第一个子异常，这一点与读取 <xref:System.Threading.Tasks.Task%601.Result?displayProperty=nameWithType> 属性将引发 `AggregateException` 不同。

    ```csharp
    total += await finishedTask;
    ```

## <a name="add-process-method"></a>添加进程方法

在 `SumPageSizesAsync` 方法下添加以下 `ProcessUrlAsync` 方法：

```csharp
static async Task<int> ProcessUrlAsync(string url, HttpClient client)
{
    byte[] content = await client.GetByteArrayAsync(url);
    Console.WriteLine($"{url,-60} {content.Length,10:#,#}");

    return content.Length;
}
```

对于任何给定的 URL，该方法都将使用提供的 `client` 实例以 `byte[]` 形式来获取响应。 将 URL 和长度写入控制台后会返回该长度。

多次运行此程序以验证并不总是以相同顺序显示已下载的长度。

> [!CAUTION]
> 如示例所示，可以在循环中使用 `WhenAny` 来解决涉及少量任务的问题。 但是，如果要处理大量任务，可以采用其他更高效的方法。 有关详细信息和示例，请参阅 [Processing tasks as they complete](https://devblogs.microsoft.com/pfxteam/processing-tasks-as-they-complete)（在任务完成时处理它们）。

## <a name="complete-example"></a>完整示例

下列代码是示例的 Program.cs 文件的完整文本。

:::code language="csharp" source="snippets/multiple-tasks/Program.cs":::

## <a name="see-also"></a>另请参阅

- <xref:System.Threading.Tasks.Task.WhenAny%2A>
- [使用 Async 和 Await 的异步编程 (C#)](index.md)
