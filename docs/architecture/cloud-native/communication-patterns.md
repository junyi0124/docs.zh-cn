---
title: 云本机通信模式
description: 了解云本机应用程序中的重要服务通信问题
author: robvet
ms.date: 05/13/2020
ms.openlocfilehash: 5ce789924e828865f7bdf717b081b9112203293a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91160913"
---
# <a name="cloud-native-communication-patterns"></a>云本机通信模式

构造云本机系统时，通信成为一项重大设计决策。 前端客户端应用程序如何与后端微服务通信？ 后端微服务如何相互通信？ 在云本机应用程序中实现通信时，应考虑哪些原则、模式和最佳做法？

## <a name="communication-considerations"></a>通信注意事项

在单一应用程序中，通信非常简单。 代码模块在同一可执行空间中一起执行 (在服务器上处理) 。 这种方法可以提高性能，因为所有内容都在共享内存中同时运行，但会导致难以维护、发展和缩放的代码紧密耦合。

云本机系统实现了基于微服务的体系结构，其中包含许多小型的独立微服务。 每个微服务在单独的进程中执行，通常在部署到 *群集*的容器中运行。

群集将一组虚拟机组合在一起，形成高度可用的环境。 它们是使用业务流程工具进行管理的，该工具负责部署和管理容器化的微服务。 图4-1 显示了使用完全托管的[Azure Kubernetes 服务](/azure/aks/intro-kubernetes)部署到 azure 云中的[Kubernetes](https://kubernetes.io)群集。

![Azure 中的 Kubernetes 群集](./media/kubernetes-cluster-in-azure.png)

**图 4-1**。 Azure 中的 Kubernetes 群集

在群集中，微服务通过 Api 和消息技术相互通信。

尽管它们提供了许多好处，但微服务没有免费的午餐。 组件之间的本地进程内方法调用现在被替换为网络调用。 每个微服务都必须通过网络协议进行通信，这会增加系统的复杂性：

- 网络拥塞、延迟和暂时性故障都是一个经常性的问题。

- 复原 (也就是说，重试失败的请求) 至关重要。

- 某些调用必须是 [幂等](https://www.restapitutorial.com/lessons/idempotency.html) 的，以便保持一致的状态。

- 每个微服务都必须对调用进行身份验证和授权。

- 必须对每个消息进行序列化，然后进行反序列化，这可能会消耗大量资源。

- 消息加密/解密十分重要。

适用于 [容器化 .Net 应用程序的微服务的体系结构](https://dotnet.microsoft.com/download/thank-you/microservices-architecture-ebook)可从 Microsoft 免费获取，为微服务应用程序提供了更深入的通信模式。 在本章中，我们提供了这些模式以及 Azure 云中提供的实现选项的高级概述。

在本章中，我们将首先解决前端应用程序和后端微服务之间的通信。 接下来，我们将探讨后端微服务相互通信。 我们将探索 gRPC 的通信技术。 最后，我们将使用服务网格技术来了解新的创新性通信模式。 我们还将了解 Azure 云如何提供不同种类的 *后备服务* 来支持云本机通信。

>[!div class="step-by-step"]
>[上一页](other-deployment-options.md)
>[下一页](front-end-communication.md)
