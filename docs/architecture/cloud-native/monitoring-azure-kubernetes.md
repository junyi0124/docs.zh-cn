---
title: 在 Azure Kubernetes 服务中进行监视
description: 在 Azure Kubernetes 服务中进行监视
ms.date: 05/13/2020
ms.openlocfilehash: 3900f169b9be4f807e72392da38a1224d6ce28e3
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91163695"
---
# <a name="monitoring-in-azure-kubernetes-services"></a>在 Azure Kubernetes 服务中进行监视

Kubernetes 中的内置日志记录为基元。 但是，有一些极佳的选项可让你从 Kubernetes 中取出日志，并将其放在正确分析的位置。 如果需要监视 AKS 群集，为 Kubernetes 配置弹性堆栈是一种很好的解决方案。

## <a name="azure-monitor-for-containers"></a>用于容器的 Azure Monitor

[容器 Azure Monitor](/azure/azure-monitor/insights/container-insights-overview) 不仅支持从 Kubernetes 使用日志，还支持从其他业务流程引擎（例如 DC/OS、Docker Swarm 和 Red Hat OpenShift）使用日志。

![使用不同容器中的日志 ](./media/containers-diagram.png)
 **图 7-10**。 使用不同容器中的日志

[Prometheus](https://prometheus.io/) 是一个流行的开源指标监视解决方案。 它属于云本机计算基础。 通常，使用 Prometheus 需要使用其自己的存储管理 Prometheus 服务器。 但是， [为容器 Azure Monitor 提供与 Prometheus 指标端点的直接集成](/azure/azure-monitor/insights/container-insights-prometheus-integration)，因此不需要单独的服务器。

日志和指标信息不只是从群集中运行的容器中收集，而是由群集本身进行收集。 它允许将日志信息与这两者进行关联，这使得跟踪错误变得更加容易。

安装日志收集器不同于 [Windows](/azure/azure-monitor/insights/containers#configure-a-log-analytics-windows-agent-for-kubernetes) 和 [Linux](/azure/azure-monitor/insights/containers#configure-a-log-analytics-linux-agent-for-kubernetes) 群集。 但在这两种情况下，日志集合都作为 Kubernetes [DaemonSet](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)实现，这意味着日志收集器将作为容器在每个节点上运行。

无论哪个协调器或操作系统运行 Azure Monitor 守护程序，日志信息都将转发到与用户熟悉的 Azure Monitor 工具。 这可确保在混合不同的日志源（如混合 Kubernetes/Azure Functions 环境）的环境中获得并行体验。

![显示多个正在运行的容器中的日志记录和指标信息的示例仪表板。 ](./media/containers-dashboard.png)
**图 7-11**。 显示多个正在运行的容器中的日志记录和指标信息的示例仪表板。

## <a name="logfinalize"></a>Log. Finalize ( # A1

日志记录是在大规模部署任何应用程序时最忽略但最重要的部分之一。 随着应用程序的大小和复杂程度的增加，调试它们的难度也很大。 可用的高质量日志使调试变得更加简单，并将其从 "几乎不可能" 领域移到 "令人愉快的体验"。

>[!div class="step-by-step"]
>[上一页](logging-with-elastic-stack.md)
>[下一页](azure-monitor.md)
