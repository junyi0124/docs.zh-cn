---
title: 通过监视和遥测对应用进行现代化
description: 通过 Azure 云和 Windows 容器现代化现有 .NET 应用程序 | 通过监视和遥测现代化应用
ms.date: 04/30/2018
ms.openlocfilehash: a6094435eece661d99904876ac49b3ca85ec45a7
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91171996"
---
# <a name="modernize-your-apps-with-monitoring-and-telemetry"></a>通过监视和遥测对应用进行现代化

在生产环境中运行应用程序时，请务必了解应用程序的执行情况。 它是否在高级别下执行？ 用户是否遇到了错误，或是应用程序是否稳定和可靠？ 你需要丰富的性能监视、功能强大的警报和仪表板，帮助确保应用程序可用且行为符合预期。 还需要能够快速查看是否存在问题，确定受影响的客户数量，并执行根本原因分析来查找并解决问题。

## <a name="monitor-your-application-with-application-insights"></a>使用 Application Insights 监视应用程序

Application Insights 是面向在多个平台上工作的 Web开发人员的可扩展应用程序性能管理 (APM) 服务。 使用它可以监视实时 Web 应用程序。 Application Insights 可以自动检测性能异常。 其中包含强大的分析工具来帮助诊断问题，帮助你了解用户在应用中实际执行了哪些操作。 Application Insights 旨在帮助持续提高性能与可用性。 它适用于各种平台（包括 .NET、Node.js 和 J2EE）中的应用，不管这些应用是托管在本地还是云中。 Application Insights 与 DevOps 进程集成，并且具有与不同开发工具的连接点。

图 4-10 显示的示例说明了 Application Insights 如何监视应用程序以及它如何将这些见解呈现给仪表板。

![Application Insights 监视仪表板的屏幕截图。](./media/modernize-your-apps-with-monitoring-and-telemetry/application-insights-monitoring-dashboard.png)

**图 4-10.** Application Insights 监视仪表板

## <a name="monitor-your-docker-infrastructure-with-log-analytics-and-its-container-monitoring-solution"></a>利用 Log Analytics 及其容器监视解决方案监视 Docker 基础结构

[Azure Log Analytics](/azure/log-analytics/log-analytics-overview) 是 [Microsoft Azure 总体监视解决方案](/azure/monitoring-and-diagnostics/monitoring-overview)的一部分。 它也是 [Operations Management Suite (OMS)](/azure/operations-management-suite/operations-management-suite-overview) 中的服务。 Log Analytics 可监视云和本地环境（OMS 适用于本地），使其保持较高的可用性和性能。 它可以收集云和本地环境中的资源生成的数据以及其他监视工具的数据，针对多个源提供分析。

与 Azure 基础结构日志相关，Log Analytics 作为 Azure 服务，从其他 Azure 服务（通过 [Azure Monitor](/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor)）、Azure VM、Docker 容器以及本地或其他云基础结构引入日志和指标数据。 Log Analytics 在此数据的基础上提供灵活的日志搜索和现成的分析。 它提供丰富的工具以用于跨多个源分析数据，允许针对所有日志运行复杂的查询，并可以根据指定的条件主动发出警报。 甚至可将自定义数据收集到它的中心 Log Analytics 存储库，你可在其中查询和可视化这些数据。 还可以利用 Log Analytics 的内置解决方案，立即获取基础结构安全性与功能的见解。

可以通过在任意浏览器中运行的 OMS 门户或 Azure 门户访问 Log Analytics。在这些门户中，可以访问配置设置和多个工具来分析和处理收集的数据。

Log Analytics 中的[容器监视解决方案](/azure/log-analytics/log-analytics-containers)可帮助用户在单个位置查看和管理 Docker 和 Windows 容器主机。 解决方案显示哪些容器正在运行，它们正在运行哪些容器映像以及正在运行容器的位置。 可以查看详细审核信息，其中包括了与容器一起使用的命令。 用户还可以通过查看和搜索集中式日志来排查容器问题，而无需远程查看 Docker 或 Windows 主机。 可以在主机上找到可能具有干扰性并且占用过多资源的容器。 此外，还可以查看容器的集中式 CPU、内存、存储器、网络使用情况和性能信息。 在运行 Windows 的计算机上，可以集中比较 Windows Server、Hyper-V 和 Docker 容器中的日志。 解决方案支持以下容器 Orchestrator：

- Docker Swarm

- DC/OS

- Kubernetes

- Red Hat OpenShift

图 4-11 显示了各种容器主机、代理和 OMS 之间的关系。

![Log Analytics 容器监视解决方案的屏幕截图。](./media/modernize-your-apps-with-monitoring-and-telemetry/log-analytics-container-monitoring-solution.png)

**图 4-11.** Log Analytics 容器监视解决方案

你可以将 Log Analytics 容器监视解决方案用于：

- 查看单个位置中有关所有容器主机的信息。

- 了解正在运行的容器、正在运行的映像以及它们的运行位置。

- 查看容器上操作的审核线索。

- 通过查看和搜索集中式日志进行故障排除，而无需远程登录到 Docker 主机。

- 在主机上找到可能是“具有干扰性的邻域”并且占用过多资源的容器。

- 查看容器的集中式 CPU、内存、存储器、网络使用情况和性能信息。

### <a name="additional-resources"></a>其他资源

- **Microsoft Azure 中的监视概述**

<https://docs.microsoft.com/azure/azure-monitor/overview>

- **什么是 Application Insights？**

<https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview>

- **什么是 Log Analytics？**

<https://docs.microsoft.com/azure/log-analytics/log-analytics-overview>

- **Azure Monitor 中的容器监视解决方案**

<https://docs.microsoft.com/azure/azure-monitor/insights/containers>

- **Azure Monitor 概述**

<https://docs.microsoft.com/azure/azure-monitor/overview>

- **什么是 Operations Management Suite (OMS)？**

<https://docs.microsoft.com/azure/operations-management-suite/operations-management-suite-overview>

>[!div class="step-by-step"]
>[上一页](build-resilient-services-ready-for-the-cloud-embrace-transient-failures-in-the-cloud.md)
>[下一页](life-cycle-ci-cd-pipelines-devops-tools.md)
