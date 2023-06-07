---
title: Application Insights - 无服务器应用
description: Application Insights 是一个无服务器诊断平台，使开发人员能够检测、会审和诊断 Web 应用、移动应用、桌面应用和微服务中的问题。
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 06/26/2018
ms.openlocfilehash: 42791b052ebb068c9b7109291e66b30b47e5821f
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173316"
---
# <a name="telemetry-with-application-insights"></a>使用 Application Insights 遥测

[Application Insights](/azure/application-insights) 是一个无服务器诊断平台，使开发人员能够检测、会审和诊断 Web 应用、移动应用、桌面应用和微服务中的问题。 只需在门户中翻转开关，即可打开 Application Insights 进入函数应用。 Application Insights 提供所有这些功能，而无需配置服务器或设置自己的数据库。 所有 Application Insights 功能都作为可自动与你的应用集成的服务提供。

![Application Insights 徽标](./media/application-insights-logo.png)

将 Application Insights 添加到现有应用程序就像将检测密钥添加到应用程序设置一样容易。 使用 Application Insights 可以：

- 根据指标创建自定义图表和警报，例如函数调用次数、运行函数所需时间以及异常
- 分析失败和服务器异常
- 通过操作深入了解性能，并衡量调用第三方依赖项所花费的时间
- 监视托管函数应用的所有服务器上的 CPU 使用情况、内存和速率
- 查看实时指标流，包括函数应用的请求计数和延迟
- 使用 [Analytics](/azure/application-insights/app-insights-analytics) 搜索、查询函数数据并据此创建自定义图表

![指标资源管理器](./media/metrics-explorer.png)

除了内置遥测，还可以生成自定义遥测。 以下代码片段使用为函数应用设置的检测密钥来创建自定义遥测客户端：

```csharp
public static TelemetryClient telemetry = new TelemetryClient()
{
    InstrumentationKey = Environment.GetEnvironmentVariable("APPINSIGHTS_INSTRUMENTATIONKEY")
};
```

下面的代码测量了在 [Azure 表存储](/azure/cosmos-db/table-storage-overview) 实例中插入新行所用的时间：

```csharp
var operation = TableOperation.Insert(entry);
var startTime = DateTime.UtcNow;
var timer = System.Diagnostics.Stopwatch.StartNew();
await table.ExecuteAsync(operation);
telemetry.TrackDependency("AzureTableStorageInsert", "Insert", startTime, timer.Elapsed, true);
```

生成的性能图表如下所示：

![自定义遥测](./media/custom-telemetry.png)

自定义遥测显示了插入新行的平均时间为 32.6 毫秒。

Application Insights 提供了一种强大而便捷的方式来记录有关无服务器应用程序的详细遥测。 你可以完全控制所提供的跟踪和日志记录级别。 可以跟踪自定义统计信息，如事件、依赖项和页面视图。 最后，利用强大的分析功能，可以编写查询来询问重要问题并生成图表和高级见解。

有关详细信息，请参阅[监视 Azure Functions](/azure/azure-functions/functions-monitoring)。

>[!div class="step-by-step"]
>[上一页](azure-functions.md)
>[下一页](logic-apps.md)
