---
title: Azure Monitor
description: 使用 Azure Monitor 来了解系统的运行情况。
ms.date: 07/05/2020
ms.openlocfilehash: 65e17740dba49c3ac3f6e13462897b5342da6710
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91160965"
---
# <a name="azure-monitor"></a>Azure Monitor

除了 Azure 中所发现的云应用程序监视解决方案外，任何其他云提供程序都不会获得太多的成熟。 Azure Monitor 是一组用于提供系统状态的工具集合的名称。 它可帮助你了解云本机服务的执行方式，并主动识别影响它们的问题。 图7-12 显示了 Azure Monitor 的高级别视图。

![Azure Monitor 的概要视图。 ](./media/azure-monitor.png)
**图 7-12**。 Azure Monitor 的概要视图。

## <a name="gathering-logs-and-metrics"></a>收集日志和指标

任何监视解决方案中的第一步是收集尽可能多的数据。 收集的数据越多，见解越深入。 检测系统在传统上非常困难。 简单网络管理协议 (SNMP) 是用于收集计算机级别信息的黄金标准协议，但它需要大量的知识和配置。 幸运的是，很多这样的硬性工作已经消除，因为最常见的指标是 Azure Monitor 自动收集的。

应用程序级指标和事件无法自动检测，因为它们特定于要部署的应用程序。 为了收集这些指标，有可直接报告此类信息的 [sdk 和 api](/azure/azure-monitor/app/api-custom-events-metrics) ，例如当客户注册或完成订单。 还可以通过 Application Insights 捕获异常并将其报告回 Azure Monitor。 Sdk 支持在云本机应用程序（包括中转、Python、JavaScript 和 .NET 语言）中找到的大多数语言。

收集有关应用程序状态信息的最终目标是确保最终用户具有良好的体验。 判断用户是否遇到了问题，而不是在进行 [外部 web 测试](/azure/azure-monitor/app/monitor-web-app-availability)的情况？ 这些测试非常简单，只需从世界各地的位置 ping 你的网站，或使代理登录到站点并模拟用户操作。

## <a name="reporting-data"></a>报告数据

收集数据后，可以对数据进行操作、汇总并将其绘制到图表中，这样用户就可以立即查看出现问题的时间。 可以将这些图表收集到仪表板或工作簿中，这是一个多页面报表，旨在告诉您有关系统的某个方面。

无需一些人工智能或机器学习即可完成新式应用程序。 为此， [可以将数据传递](https://www.youtube.com/watch?v=Cuza-I1g9tw) 到 Azure 中的各种机器学习工具，以便您可以提取其他隐藏的趋势和信息。

Application Insights 提供了一种 (功能强大的类似 SQL 的) 查询语言（称为 *Kusto* ），它可以查询记录、汇总记录，甚至绘制图表。 例如，以下查询将查找2007年11月的所有记录，按州分组，并将前10个为饼图。

```kusto
StormEvents
| where StartTime >= datetime(2007-11-01) and StartTime < datetime(2007-12-01)
| summarize count() by State
| top 10 by count_
| render piechart
```

图7-13 显示了此 Application Insights 查询的结果。

![Application Insights 查询结果 ](./media/application_insights_example.png)
 **图 7-13**。 Application Insights 查询结果。

[使用 Kusto](https://dataexplorer.azure.com/clusters/help/databases/Samples)查询有一个板块。 读取 [示例查询](/azure/kusto/query/samples) 也可以指导。

## <a name="dashboards"></a>仪表板

有多种不同的仪表板技术可用于从 Azure Monitor 显示信息。 最简单的情况是，只需在 Application Insights 中运行查询，然后将 [数据绘制到图表](/azure/azure-monitor/learn/tutorial-app-dashboards)中。

![在 Azure 主仪表板 ](./media/azure_dashboard.png)
 **图 7-14**中嵌入 Application Insights 图表的示例。 在 Azure 主仪表板中嵌入 Application Insights 图表的示例。

然后，可以通过使用 "仪表板" 功能，将这些图表嵌入到 Azure 门户中。 对于具有更严格要求的用户（例如，能够向下钻取多个数据层），可以 [Power BI](https://powerbi.microsoft.com/)Azure Monitor 数据。 Power BI 是行业领先的企业级商业智能工具，可从多个不同的数据源聚合数据。

![Power BI 仪表板的示例](./media/powerbidashboard.png)

**图 7-15**。 Power BI 仪表板的示例。

## <a name="alerts"></a>警报

有时，数据仪表板不足。 如果没有人唤醒监视仪表板，则在解决问题之前，甚至会检测到问题。 为此，Azure Monitor 还提供了一个顶部凹槽 [警报解决方案](/azure/azure-monitor/platform/alerts-overview)。 可以通过各种条件触发警报，包括：

- 指标值
- 日志搜索查询
- 活动日志事件
- 基础 Azure 平台的运行状况
- 网址可用性测试

触发警报时，警报可执行各种任务。 在简单端，警报可能只是向个人发送电子邮件通知或发送到个人的短信。 更多涉及的警报可能会在工具（如 PagerDuty）中触发工作流，该工具可感知特定应用程序的调用者。 警报可以触发 [Microsoft Flow](https://flow.microsoft.com/) 的操作，以接近无限制工作流的方式。

当发现警报的常见原因时，可以使用警报的常见原因和解决方法的步骤来增强警报。 高度成熟的云本机应用程序部署可以选择启动自愈任务，这些任务可执行一些操作，例如从规模集中删除失败节点或触发自动缩放活动。 最终，可能不再需要唤醒2AM 上的待命人员来解决实时站点问题，因为系统将能够自行调整以进行补偿或至少 limp，直到在某个时间早上进入工作为止。

Azure Monitor 会自动利用机器学习来了解部署的应用程序的正常运行参数。 这使它能够检测到其正常参数外操作的服务。 例如，站点上典型的工作日流量可能是每分钟10000请求。 然后，在给定的一周突然，每分钟遇到的请求数非常罕见的20000请求。 [智能检测](/azure/azure-monitor/app/proactive-diagnostics) 会注意到与标准的偏差，并触发警报。 同时，趋势分析非常智能，可避免在预计流量负载时触发误报。

## <a name="references"></a>参考

- [Azure Monitor](/azure/azure-monitor/overview)

>[!div class="step-by-step"]
>[上一页](monitoring-azure-kubernetes.md)
>[下一页](identity.md)
