---
title: 将容器和无服务器方法与云本机服务组合在一起
description: 将容器和 Kubernetes 与无服务器方法结合起来
ms.date: 05/13/2020
ms.openlocfilehash: b1b85519ce02ddd1d69735d872cf24fadcc81ef7
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91160887"
---
# <a name="combining-containers-and-serverless-approaches"></a>合并容器和无服务器方法

云本机应用程序通常实现利用容器和业务流程的服务。 通常会有机会将一些应用程序的服务作为 Azure Functions 公开。 但是，在部署到 Kubernetes 的云本机应用程序中，充分利用此相同工具集中的 Azure Functions 是非常好的做法。 幸运的是，您可以将 Azure Functions 包装在 Docker 容器内，并使用与 Kubernetes 的其他应用程序相同的过程和工具进行部署。

## <a name="when-does-it-make-sense-to-use-containers-with-serverless"></a>使用无服务器的容器有何意义？

你的 Azure 函数不知道部署它的平台。 在某些情况下，你可能有特定的要求，并且需要自定义运行函数代码的环境。 你将需要一个支持依赖项的自定义映像或默认映像不支持的配置。 在这些情况下，在自定义 Docker 容器中部署函数是有意义的。

## <a name="when-should-you-avoid-using-containers-with-azure-functions"></a>何时应避免使用带有 Azure Functions 的容器？

如果要使用消耗计费，将无法在容器中运行函数。 此外，如果将函数部署到 Kubernetes 群集，将不再受益于 Azure Functions 提供的内置缩放。 你需要使用 Kubernetes 的缩放功能，详见本章前面所述。

## <a name="how-to-combine-serverless-and-docker-containers"></a>如何组合无服务器和 Docker 容器

若要将 Azure 函数包装在 Docker 容器中，请安装 [Azure Functions Core Tools](https://github.com/Azure/azure-functions-core-tools) ，并运行以下命令：

```console
func init ProjectName --worker-runtime dotnet --docker
```

当创建项目时，它将包括 Dockerfile 和配置为的辅助运行时 `dotnet` 。 现在，你可以在本地创建和测试函数。 使用和命令生成并运行  `docker build` 它 `docker run` 。 有关使用 Docker 支持构建 Azure Functions 的详细步骤，请参阅在 [Linux 上使用自定义映像创建函数](/azure/azure-functions/functions-create-function-linux-custom-image) 教程。

## <a name="how-to-combine-serverless-and-kubernetes-with-keda"></a>如何将无服务器和 Kubernetes 与 KEDA 结合

在本章中，你已了解到 Azure Functions 的平台会自动扩大以满足需求。 但是，在将容器化函数部署到 AKS 时，会丢失内置缩放功能。 为解决此 [Kubernetes，基于事件驱动 (KEDA) ](/azure/azure-functions/functions-kubernetes-keda)。 它实现了包含容器化函数的精细自动缩放 `event-driven Kubernetes workloads,` 。

KEDA 为 Docker 容器中的函数运行时提供事件驱动的缩放功能。 KEDA 可以在没有事件发生时从零个实例 (扩展 `n instances` ，而不会根据负载进行) 。 它通过向 Kubernetes 自动缩放程序 (水平 Pod 自动缩放程序) 公开自定义指标来启用自动缩放。 将 Functions 容器与 KEDA 结合使用时，可以在任何 Kubernetes 群集中复制无服务器函数功能。

值得注意的是，KEDA 项目现在由云本机计算基础 (CNCF) 进行管理。

>[!div class="step-by-step"]
>[上一页](leverage-serverless-functions.md)
>[下一页](deploy-containers-azure.md)
