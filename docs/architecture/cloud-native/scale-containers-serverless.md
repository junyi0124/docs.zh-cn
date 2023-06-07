---
title: 缩放容器和无服务器应用程序
description: 利用 Azure Kubernetes 服务缩放云本机应用程序以满足用户需求。
ms.date: 05/13/2020
ms.openlocfilehash: edf56ba50ba391e06e588d682918cd473a2cf374
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91165974"
---
# <a name="scaling-containers-and-serverless-applications"></a>缩放容器和无服务器应用程序

缩放应用程序的方法有两种：向上或向外缩放。前者是指将容量添加到单个资源，而后者则是指添加更多的资源来增加容量。

## <a name="the-simple-solution-scaling-up"></a>简单的解决方案：向上缩放

升级具有增加的 CPU、内存、磁盘 i/o 速度和网络 i/o 速度的现有主机服务器称为 " *向上缩放*"。 扩展云本机应用程序涉及从云供应商处选择更强大的资源。 例如，你可以在 Kubernetes 群集中使用具有更大 Vm 的新节点池。 然后，将容器化服务迁移到新池。

无服务器应用通过从专用应用服务计划中选择 [高级函数计划](/azure/azure-functions/functions-scale) 或高级实例大小进行扩展。

## <a name="scaling-out-cloud-native-apps"></a>向外扩展云本机应用

云本机应用程序通常会经历较大的需求波动，并需要一段时间的通知。 它们倾向于横向扩展。通过向现有群集添加 (称为节点) 或应用程序实例的其他计算机，横向扩展。 在 Kubernetes 中，可以通过调整应用的配置设置来进行手动缩放 (例如， [缩放节点池](/azure/aks/use-multiple-node-pools#scale-a-node-pool-manually)) 或通过自动缩放。

AKS 群集可通过以下两种方式之一进行自动缩放：

首先， [横向 Pod 自动缩放程序](/azure/aks/tutorial-kubernetes-scale#autoscale-pods) 监视资源需求，并自动缩放 Pod 副本以满足此要求。 当流量增加时，会自动预配其他副本以扩展你的服务。 同样，当需求减少时，它们将被删除以进行扩展。 定义要缩放的指标，例如 CPU 使用率。 你还可以指定要运行的最小和最大副本数。 AKS 监视该度量值并相应地进行缩放。

接下来， [AKS Cluster 自动缩放程序](/azure/aks/cluster-autoscaler) 功能使你能够跨 Kubernetes 群集自动缩放计算节点，以满足需求。 通过此功能，可以在需要的多个计算容量时，自动将新 Vm 添加到基础 Azure 虚拟机规模集。 它还会在不再需要节点时将其删除。

图3-13 显示了这两个缩放服务之间的关系。

![向外扩展应用服务计划。](./media/aks-cluster-autoscaler.png)

**图 3-13**。 向外扩展应用服务计划。

同时，两者都确保容器实例和计算节点获得最佳数量，以支持波动需求。 横向 pod 自动缩放程序优化所需的 pod 数。 群集自动缩放程序优化所需的节点数。

### <a name="scaling-azure-functions"></a>缩放 Azure Functions

Azure Functions 根据需要自动横向扩展。 服务器资源是基于触发事件的数量动态分配和删除的。 只需为运行函数时使用的计算资源付费。 计费基于执行次数、执行时间和使用的内存。

尽管默认消耗计划为大多数应用提供经济实惠的可缩放解决方案，但 "高级" 选项允许开发人员灵活应对自定义 Azure Functions 要求。 升级到高级计划可对实例大小、准备好实例进行控制， (避免冷启动延迟) 和专用 Vm。

>[!div class="step-by-step"]
>[上一页](deploy-containers-azure.md)
>[下一页](other-deployment-options.md)
