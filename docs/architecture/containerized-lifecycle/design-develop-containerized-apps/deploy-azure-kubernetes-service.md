---
title: 协调安排微服务和多容器应用程序，实现高可伸缩性和高可用性
description: 了解如何使用 Azure Kubernetes 服务部署应用。
ms.date: 08/06/2020
ms.openlocfilehash: ba9887c0a4837c16a60ebeb006416c0fa8c105e0
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032198"
---
# <a name="deploy-to-azure-kubernetes-service-aks"></a>部署到 Azure Kubernetes 服务 (AKS)

你可以使用安装了 Azure 命令行接口 (Azure CLI) 的首选客户端操作系统（Windows、macOS 或 Linux）与 AKS 进行交互。 有关更多详细信息，请参阅 [Azure CLI 文档](/cli/azure/?view=azure-cli-latest)和[安装指南](/cli/azure/install-azure-cli?view=azure-cli-latest)，以了解可用的环境。

## <a name="create-the-aks-environment-in-azure"></a>在 Azure 中创建 AKS 环境

有多种方法可创建 AKS 环境。 可以使用 Azure-CLI 命令或使用 Azure 门户来完成此操作。

此处是有关使用 Azure-CLI 创建群集和通过 Azure 门户查看结果的一些示例。 还需要在开发计算机中安装 Kubectl 和 Docker。

## <a name="create-the-aks-cluster"></a>创建 AKS 群集

使用以下命令创建 AKS 群集（资源组必须存在）：

```console
az aks create --resource-group explore-docker-aks-rg --name explore-docker-aks --node-vm-size Standard_B2s --node-count 1 --generate-ssh-keys --location westeurope
```

> [!NOTE]
> `--node-vm-size` 和 `--node-count` 参数值适用于样本/开发应用程序。

创建完作业后，你可以看到：

- 在初始资源组中创建的 AKS 群集
- 一个新的相关资源组，其中包含与 AKS 群集相关的资源，如下图所示。

带有 AKS 群集的初始资源组：

![AKS 资源组的浏览器视图。](media/deploy-azure-kubernetes-service/aks-cluster-view.png)

**图 4-17**。 Azure 的 AKS 资源组视图。

AKS 群集资源组：

![Azure AKS 资源组的浏览器视图。](media/deploy-azure-kubernetes-service/aks-resource-group-view.png)

**图 4-18**. Azure 的 AKS 视图。

> [!IMPORTANT]
> 通常无需修改 AKS 群集资源组中的资源。 例如，删除 AKS 群集时，资源组随之删除。

还可以查看使用 `Azure CLI` 和 `Kubectl` 创建的节点。

首先，获取凭据：

```console
az aks get-credentials --resource-group explore-docker-aks-rg --name explore-docker-aks
```

![来自上述命令的控制台输出：将“explore-docker-aks”合并为 /home/miguel/.kube/config 中的当前上下文。](media/deploy-azure-kubernetes-service/get-credentials-command-result.png)

**图 4-19**. `aks get-credentials` 命令结果。

然后，从 Kubectl 获取节点：

```console
kubectl get nodes
```

![来自上述命令的控制台输出：包括状态、时限（运行时间）和版本的节点列表](media/deploy-azure-kubernetes-service/kubectl-get-nodes-command-result.png)

图 4-20。 `kubectl get nodes` 命令结果。

> [!div class="step-by-step"]
> [上一页](orchestrate-high-scalability-availability.md)
> [下一页](docker-apps-development-environment.md)
