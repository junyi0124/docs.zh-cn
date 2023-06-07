---
title: 使用 .NET Core 中的 EventCounters 衡量性能
description: 本教程将介绍如何使用 EventCounters 衡量性能。
ms.date: 08/07/2020
ms.topic: tutorial
ms.openlocfilehash: 2ed7f234b685dab91ab275105d26b474e3bd1a87
ms.sourcegitcommit: 3d6d6595a03915f617349781f455f838a44b0f44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/19/2020
ms.locfileid: "97700738"
---
# <a name="tutorial-measure-performance-using-eventcounters-in-net-core"></a>教程：使用 .NET Core 中的 EventCounters 衡量性能

**本文适用于：** ✔️ .NET Core 3.0 SDK 及更高版本

本教程将介绍如何使用 <xref:System.Diagnostics.Tracing.EventCounter> 衡量高频率事件的性能。 可以使用由各种官方 .NET Core 包或第三方提供者发布的[可用的计数器](available-counters.md)，或创建自己的监视指标。

在本教程中，将：

> [!div class="checklist"]
>
> - 实现 <xref:System.Diagnostics.Tracing.EventSource>。
> - 利用 [dotnet-counters](dotnet-counters.md) 监视计数器。

## <a name="prerequisites"></a>先决条件

本教程使用：

- [.NET Core 3.1 SDK](https://dotnet.microsoft.com/download/dotnet-core) 或更高版本。
- [dotnet-counters](dotnet-counters.md) 监视事件计数器。
- 要诊断的[示例调试目标](/samples/dotnet/samples/diagnostic-scenarios)应用。

## <a name="get-the-source"></a>获取源

示例应用程序将用作监视的基础。 示例浏览器中提供了[示例 ASP.NET Core 存储库](/samples/dotnet/samples/diagnostic-scenarios)。 下载 zip 文件，下载后提取它，并在你喜欢的 IDE 中打开它。 生成并运行应用程序以确保它正常工作，然后停止应用程序。

## <a name="implement-an-eventsource"></a>实现 EventSource

对于每隔几毫秒发生的事件，最好使每个事件的开销较低（小于一毫秒）。 否则，对性能的影响将很大。 记录事件意味着你将向磁盘写入内容。 如果磁盘不够快，你将丢失事件。 你需要一个解决方案，而不是记录事件本身。

在处理大量事件时，了解每个事件的度量值也无济于事。 大多数时候，你只需要一些统计信息。 因此，你可以在进程本身中获取统计信息，然后偶尔编写一个事件来报告统计信息，这是 <xref:System.Diagnostics.Tracing.EventCounter> 将执行的操作。

下面是有关如何实现 <xref:System.Diagnostics.Tracing.EventSource?displayProperty=fullName> 的示例。 创建名为 MinimalEventCounterSource.cs 的新文件，并使用代码片段作为其源：

:::code language="csharp" source="snippets/EventCounters/MinimalEventCounterSource.cs":::

<xref:System.Diagnostics.Tracing.EventSource.WriteEvent%2A?displayProperty=nameWithType> 行是 <xref:System.Diagnostics.Tracing.EventSource> 部分，而不是 <xref:System.Diagnostics.Tracing.EventCounter> 的一部分，编写它是为了表明你可以一起记录消息和事件计数器。

## <a name="add-an-action-filter"></a>添加操作筛选器

示例源代码是 ASP.NET Core 项目。 可以全局添加将记录总请求时间的[操作筛选器](/aspnet/core/mvc/controllers/filters#action-filters)。 创建名为 LogRequestTimeFilterAttribute.cs 的新文件，并使用以下代码：

```csharp
using System.Diagnostics;
using Microsoft.AspNetCore.Http.Extensions;
using Microsoft.AspNetCore.Mvc.Filters;

namespace DiagnosticScenarios
{
    public class LogRequestTimeFilterAttribute : ActionFilterAttribute
    {
        readonly Stopwatch _stopwatch = new Stopwatch();

        public override void OnActionExecuting(ActionExecutingContext context) => _stopwatch.Start();

        public override void OnActionExecuted(ActionExecutedContext context)
        {
            _stopwatch.Stop();

            MinimalEventCounterSource.Log.Request(
                context.HttpContext.Request.GetDisplayUrl(), _stopwatch.ElapsedMilliseconds);
        }
    }
}
```

操作筛选器在请求开始时启动 <xref:System.Diagnostics.Stopwatch>，并在其完成后停止，捕获运行时间。 总毫秒数记录到 `MinimalEventCounterSource` 单一实例。 为了应用此筛选器，需要将其添加到筛选器集合。 在 Startup.cs 文件中，更新包含此筛选器的 `ConfigureServices` 方法。

```csharp
public void ConfigureServices(IServiceCollection services) =>
    services.AddControllers(options => options.Filters.Add<LogRequestTimeFilterAttribute>())
            .AddNewtonsoftJson();
```

## <a name="monitor-event-counter"></a>监视事件计数器

通过 <xref:System.Diagnostics.Tracing.EventSource> 上的实现和自定义操作筛选器，生成和启动应用程序。
你已将指标记录到 <xref:System.Diagnostics.Tracing.EventCounter> 中，但除非你从其中访问统计信息，否则它将不起作用。 要获取统计信息，需要通过创建以所需事件频率触发的计时器来启用 <xref:System.Diagnostics.Tracing.EventCounter>，并启用侦听器来捕获事件。 为此，可以使用 [dotnet-counters](dotnet-counters.md)。

使用 [dotnet-counters ps](dotnet-counters.md#dotnet-counters-ps) 命令来显示可监视的 .NET 进程的列表。

```console
dotnet-counters ps
```

通过使用 `dotnet-counters ps` 命令的输出中的进程标识符，你可以使用以下 `dotnet-counters monitor` 命令开始监视事件计数器：

```console
dotnet-counters monitor --process-id 2196 --counters Sample.EventCounter.Minimal,Microsoft.AspNetCore.Hosting[total-requests,requests-per-second],System.Runtime[cpu-usage]
```

当 `dotnet-counters monitor` 命令正在运行时，请在浏览器上按住 <kbd>F5</kbd>，以开始向 `https://localhost:5001/api/values` 终结点发出连续请求。 几秒后按 <kbd>q</kbd> 以停止

```console
Press p to pause, r to resume, q to quit.
    Status: Running

[Microsoft.AspNetCore.Hosting]
    Request Rate / 1 sec                               9
    Total Requests                                   134
[System.Runtime]
    CPU Usage (%)                                     13
[Sample.EventCounter.Minimal]
    Request Processing Time (ms)                      34.5
```

`dotnet-counters monitor` 命令非常适合主动监视。 不过，可以收集这些诊断指标以便进行后续处理和分析。 为此，请使用 `dotnet-counters collect` 命令。 `collect` 开关命令类似于 `monitor` 命令，但接受几个其他参数。 你可以指定所需的输出文件名和格式。 对于名为 diagnostics.json 的 JSON 文件，请使用以下命令：

```console
dotnet-counters collect --process-id 2196 --format json -o diagnostics.json --counters Sample.EventCounter.Minimal,Microsoft.AspNetCore.Hosting[total-requests,requests-per-second],System.Runtime[cpu-usage]
```

再一次，当命令正在运行时，在浏览器上按住 <kbd>F5</kbd>，以开始向 `https://localhost:5001/api/values` 终结点发出连续请求。 几秒后按 <kbd>q</kbd> 以停止。 写入 diagnostics.json 文件。 写入的 JSON 文件不会缩进；但为了提升可读性，在这里进行了缩进。

```json
{
  "TargetProcess": "DiagnosticScenarios",
  "StartTime": "8/5/2020 3:02:45 PM",
  "Events": [
    {
      "timestamp": "2020-08-05 15:02:47Z",
      "provider": "System.Runtime",
      "name": "CPU Usage (%)",
      "counterType": "Metric",
      "value": 0
    },
    {
      "timestamp": "2020-08-05 15:02:47Z",
      "provider": "Microsoft.AspNetCore.Hosting",
      "name": "Request Rate / 1 sec",
      "counterType": "Rate",
      "value": 0
    },
    {
      "timestamp": "2020-08-05 15:02:47Z",
      "provider": "Microsoft.AspNetCore.Hosting",
      "name": "Total Requests",
      "counterType": "Metric",
      "value": 134
    },
    {
      "timestamp": "2020-08-05 15:02:47Z",
      "provider": "Sample.EventCounter.Minimal",
      "name": "Request Processing Time (ms)",
      "counterType": "Metric",
      "value": 0
    },
    {
      "timestamp": "2020-08-05 15:02:47Z",
      "provider": "System.Runtime",
      "name": "CPU Usage (%)",
      "counterType": "Metric",
      "value": 0
    },
    {
      "timestamp": "2020-08-05 15:02:48Z",
      "provider": "Microsoft.AspNetCore.Hosting",
      "name": "Request Rate / 1 sec",
      "counterType": "Rate",
      "value": 0
    },
    {
      "timestamp": "2020-08-05 15:02:48Z",
      "provider": "Microsoft.AspNetCore.Hosting",
      "name": "Total Requests",
      "counterType": "Metric",
      "value": 134
    },
    {
      "timestamp": "2020-08-05 15:02:48Z",
      "provider": "Sample.EventCounter.Minimal",
      "name": "Request Processing Time (ms)",
      "counterType": "Metric",
      "value": 0
    },
    {
      "timestamp": "2020-08-05 15:02:48Z",
      "provider": "System.Runtime",
      "name": "CPU Usage (%)",
      "counterType": "Metric",
      "value": 0
    },
    {
      "timestamp": "2020-08-05 15:02:50Z",
      "provider": "Microsoft.AspNetCore.Hosting",
      "name": "Request Rate / 1 sec",
      "counterType": "Rate",
      "value": 0
    },
    {
      "timestamp": "2020-08-05 15:02:50Z",
      "provider": "Microsoft.AspNetCore.Hosting",
      "name": "Total Requests",
      "counterType": "Metric",
      "value": 134
    },
    {
      "timestamp": "2020-08-05 15:02:50Z",
      "provider": "Sample.EventCounter.Minimal",
      "name": "Request Processing Time (ms)",
      "counterType": "Metric",
      "value": 0
    }
  ]
}
```

## <a name="next-steps"></a>后续步骤

> [!div class="nextstepaction"]
> [EventCounters](event-counters.md)
