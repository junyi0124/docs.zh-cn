---
title: 服务网格通信基础结构
description: 了解服务网格技术如何简化云本地微服务通信
author: robvet
ms.date: 05/13/2020
ms.openlocfilehash: 39dc1ded06eb0b92a2a1b40cfe981d9bd49bf381
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91165944"
---
# <a name="service-mesh-communication-infrastructure"></a>服务网格通信基础结构

在本章中，我们探讨了微服务通信的难题。 我们说，开发团队需要对后端服务之间的通信方式保密。 理想情况下，服务间通信越少，性能越好。 不过，由于后端服务经常依赖于另一种方式来完成操作，因此不一定能避免这种情况。

我们探讨了不同的方法来实现同步 HTTP 通信和异步消息传递。 在每种情况下，开发人员都能实现通信代码。 通信代码非常复杂且耗时。 决策不当会导致严重的性能问题。

一种更新式的微服务通信中心方法，围绕一种新的、快速发展的技术，以 *服务网格*为依据。 [服务网格](https://www.nginx.com/blog/what-is-a-service-mesh/)是一种可配置的基础结构层，它具有内置功能，可用于处理服务到服务通信、复原和许多跨切削问题。 它会将这些问题的责任转移到微服务和服务网格层。 从微服务抽象出通信。

服务网格的一个关键组件是代理。 在云本机应用程序中，代理的实例通常与每个微服务在一起。 虽然它们在单独的进程中执行，但两者密切相关并共享相同的生命周期。 此模式称为 [挎斗模式](/azure/architecture/patterns/sidecar)，如图4-24 所示。

![使用侧面汽车的服务网格](./media/service-mesh-with-side-car.png)

图 4-24  。 使用侧面汽车的服务网格

请注意，上图中的消息是由每个微服务一起运行的代理截取的。 每个代理都可以配置特定于微服务的流量规则。 它了解消息，并可以将消息路由到您的服务和外界。

除了管理服务到服务通信以外，服务网格还提供对服务发现和负载平衡的支持。

一旦配置后，服务网格就会非常有效。 网格从服务发现终结点中检索相应的实例池。 它向特定服务实例发送请求，记录结果的延迟和响应类型。 它根据不同因素选择最有可能返回快速响应的实例，包括观察到的最近请求的延迟。

服务网格在应用程序级别管理流量、通信和网络问题。 它了解消息和请求。 服务网格通常与容器 orchestrator 集成。 Kubernetes 支持可以添加服务网格的可扩展体系结构。

第6章深入探讨了服务网格技术，其中包括对其体系结构和可用开源实现的讨论。

## <a name="summary"></a>总结

在本章中，我们讨论了云本机通信模式。 我们首先检查前端客户端如何与后端微服务通信。 在此过程中，我们讨论了 API 网关平台和实时通信。 然后介绍了微服务如何与其他后端服务进行通信。 我们同时探讨了跨服务的同步 HTTP 通信和异步消息传递。 我们介绍了 gRPC，这是云中的一项技术。 最后，我们引入了一种新的、快速发展的技术，该技术可简化微服务通信。

特别强调的是托管 Azure 服务，可帮助实现云本机系统中的通信：

- [Azure 应用程序网关](/azure/application-gateway/overview)
- [Azure API 管理](https://azure.microsoft.com/services/api-management/)
- [Azure SignalR 服务](https://azure.microsoft.com/services/signalr-service/)
- [Azure 存储队列](/azure/storage/queues/storage-queues-introduction)
- [Azure 服务总线](/azure/service-bus-messaging/service-bus-messaging-overview)
- [Azure 事件网格](/azure/event-grid/overview)
- [Azure 事件中心](https://azure.microsoft.com/services/event-hubs/)

接下来，我们将迁移到云本机系统中的分布式数据，以及它所呈现的优点和挑战。

### <a name="references"></a>参考

- [.NET 微服务：容器化 .NET 应用程序的体系结构](https://dotnet.microsoft.com/download/thank-you/microservices-architecture-ebook)

- [为微服务设计服务间通信](/azure/architecture/microservices/design/interservice-communication)

- [Azure SignalR 服务是一项完全托管的服务，可用于添加实时功能](https://azure.microsoft.com/blog/azure-signalr-service-a-fully-managed-service-to-add-real-time-functionality/)

- [Azure API 网关入口控制器](https://azure.github.io/application-gateway-kubernetes-ingress/)

- [关于 Azure Kubernetes 服务中的入口 (AKS) ](https://vincentlauzon.com/2018/10/10/about-ingress-in-azure-kubernetes-service-aks/)

- [gRPC 文档](https://grpc.io/docs/guides/)

- [适用于 WCF 开发人员的 gRPC](../grpc-for-wcf-developers/index.md)

- [将 gRPC Services 与 HTTP Api 进行比较](/aspnet/core/grpc/comparison?view=aspnetcore-3.0)

- [通过 .NET 视频构建 gRPC Services](https://channel9.msdn.com/Shows/The-Cloud-Native-Show/Building-Microservices-with-gRPC-and-NET)

>[!div class="step-by-step"]
>[上一页](grpc.md)
>[下一页](distributed-data.md)
