---
title: 管理 Docker 生产环境
description: 了解管理基于容器的生产环境的关键内容。
ms.date: 08/06/2020
ms.openlocfilehash: dbc5f541478410060420f95f32e4ff5291354075
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91160640"
---
# <a name="manage-production-docker-environments"></a>管理 Docker 生产环境

群集管理和业务流程是控制一组主机的流程。 这可能涉及从群集中添加和删除主机，获取有关主机和容器的当前状态以及启动和停止进程的信息。 群集管理和业务流程与计划密切相关，因为计划程序必须访问群集中的每个主机才能计划服务。 出于此原因，同一个工具通常可用于这两种目的。

## <a name="container-service-and-management-tools"></a>容器服务和管理工具

容器服务提供流行的开源容器群集和业务流程解决方案的快速部署。 它利用 Docker 映像来确保应用程序容器是完全可移植的。 使用容器服务，可以通过 Azure 资源管理器模板或 Azure 门户部署 DC/OS（由 Mesosphere 和 Apache Mesos 提供支持）和 Docker Swarm 群集，以确保可以将这些应用程序扩展到数千容器，甚至数万个容器。

使用 Azure 虚拟机规模集部署这些群集，使其可以利用 Azure 网络功能和存储产品。 要访问容器服务，需要一个 Azure 订阅。 使用容器服务，可以利用 Azure 的企业级功能，同时仍然保持应用程序的可移植性，包括业务流程层的可移植性。

表 6-1 列出了与其业务流程协调程序、计划程序和群集相关的常用管理工具。

**表 6-1**。 Docker 管理工具

| 管理工具 | 说明 | 相关业务流程协调程序 |
|------------------|-------------|-----------------------|
| [用于容器的 Azure Monitor](/azure/monitoring/monitoring-container-insights-overview) | 专用的 Azure Kubernetes 管理工具 | Azure Kubernetes 服务 (AKS) |
| [Kubernetes Web UI（仪表板）](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) | Kubernetes 管理工具可以监视和管理本地 Kubernetes 群集 | Azure Kubernetes 服务 (AKS)<br/>本地 Kubernetes |
| [适用于 Service Fabric 的 Azure 门户](/azure/service-fabric/service-fabric-cluster-creation-via-portal)<br/>[Azure Service Fabric Explorer](/azure/service-fabric/service-fabric-visualizing-your-cluster) | 用于在 Azure、本地开发和其他云上管理 Service Fabric 群集的联机和桌面版本 | Azure Service Fabric |
| [容器监视 (Azure Monitor)](/azure/azure-monitor/insights/containers) | 常规容器管理与监视解决方案。 可以通过[用于容器的 Azure Monitor](/azure/monitoring/monitoring-container-insights-overview) 管理 Kubernetes 群集。 | Azure Service Fabric<br/>Azure Kubernetes 服务 (AKS)<br/>Mesosphere DC/OS 和其他。 |

## <a name="azure-service-fabric"></a>Azure Service Fabric

Azure Service Fabric 也可用于群集部署和管理。 [Service Fabric](https://azure.microsoft.com/services/service-fabric/) 是一种 Microsoft 微服务平台，其中包括用于构建高度可缩放的微服务应用程序的容器业务流程和开发人员编程模型。 Service Fabric 支持 Linux 和 Windows 容器中的 Docker，且可在 Windows 和 Linux 服务器中运行。

以下是 Service Fabric 管理工具：

- [适用于 Service Fabric 的 Azure 门户](/azure/service-fabric/service-fabric-cluster-creation-via-portal)可对群集执行相关的操作（创建/更新/删除）或配置其基础结构（VM、负载均衡器、网络等）

- [Azure Service Fabric Explorer](/azure/service-fabric/service-fabric-visualizing-your-cluster) 是一款专用的 Web UI 和桌面多平台工具，从节点/VM 以及从应用程序和服务的角度来看，该工具提供对 Service Fabric 群集的见解和某些操作。

>[!div class="step-by-step"]
>[上一页](run-microservices-based-applications-in-production.md)
>[下一页](monitor-containerized-application-services.md)
