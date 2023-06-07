---
title: 可复原通信
description: 构建适用于 Azure 的云本机 .NET 应用 |弹性通信
author: robvet
ms.date: 05/13/2020
ms.openlocfilehash: 18b26223634efc5c05f680d0cbb7c8cbc2490a59
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91166035"
---
# <a name="resilient-communications"></a>弹性通信

本书介绍了基于微服务的体系结构方法。 虽然这种体系结构提供了重要的优势，但却带来了许多挑战：

- *进程外网络通信。* 每个微服务都通过网络协议进行通信，该协议引入了网络拥塞、延迟和暂时性故障。

- *服务发现。* 当跨计算机群集使用其自己的 IP 地址和端口运行时，微服务如何发现并相互通信？

- *复原.* 如何管理短暂的故障并使系统保持稳定？

- *负载均衡。* 如何在多个微服务实例之间分布入站流量？

- *安全性。* 安全问题（例如传输级加密和证书管理）如何强制实施？

- *分布式监视。* -你如何关联和捕获跨多个使用微服务的单个请求的可跟踪性和监视？

你可以使用不同的库和框架解决这些问题，但实现可能成本高昂、复杂且耗时。 最终，还会提供与业务逻辑耦合的基础结构问题。

## <a name="service-mesh"></a>服务网格

一种更好的方法是以 *服务网格*为依据的技术。 [服务网格](https://www.nginx.com/blog/what-is-a-service-mesh/)是一种可配置的基础结构层，其中内置了用于处理服务通信的功能以及上述其他难题。 它通过将这些问题移到服务代理中来分离这些问题。 代理部署到一个单独的进程中 (称为 [挎斗](/azure/architecture/patterns/sidecar)) ，以提供与业务代码的隔离。 但是，挎斗链接到服务-它是用它创建的并共享其生命周期。 图6-7 显示了这种情况。

![使用侧面汽车的服务网格](./media/service-mesh-with-side-car.png)

图 6-7  。 使用侧面汽车的服务网格

在上图中，请注意代理如何截获和管理微服务与群集之间的通信。

服务网格以逻辑方式拆分为两个不同的组件： [数据平面](https://blog.envoyproxy.io/service-mesh-data-plane-vs-control-plane-2774e720f7fc) 和 [控制平面](https://blog.envoyproxy.io/service-mesh-data-plane-vs-control-plane-2774e720f7fc)。 图6-8 显示了这些组件及其责任。

![服务网格控制和数据平面](./media/istio-control-and-data-plane.png)

**图6-8。** 服务网格控制和数据平面

一旦配置后，服务网格就会非常有效。 它可以从服务发现终结点中检索相应的实例池。 然后，网格可以将请求发送到特定的实例，记录结果的延迟和响应类型。 网格可以根据许多因素选择最有可能返回快速响应的实例，包括其观察到的最近请求的延迟。

如果实例无响应或失败，网格将在另一个实例上重试该请求。 如果它返回错误，则网格将从负载平衡池中逐出实例，并在修复后重述该实例。 如果请求超时，网格可能会失败，然后重试该请求。 网格将捕获度量值并将其分发到集中式指标系统。

## <a name="istio-and-envoy"></a>Istio 和 Envoy

虽然目前存在一些服务网格选项，但在撰写本文时， [Istio](https://istio.io/docs/concepts/what-is-istio/) 是最受欢迎的。 Istio 是 IBM、Google 和 Lyft 的联合冒险。 这是一个开源产品，可集成到新的或现有的分布式应用程序。 该技术提供了一个一致且完整的解决方案来保护、连接和监视微服务。 其功能包括：

- 使用基于标识的强身份验证和授权在群集中保护服务间通信。
- 自动负载均衡，适用于 HTTP、 [gRPC](https://grpc.io/)、WEBSOCKET 和 TCP 流量。
- 通过丰富的路由规则、重试、故障转移和故障注入对流量行为进行精细控制。
- 支持访问控制、速率限制和配额的可插入策略层和配置 API。
- 针对群集中的所有流量（包括流入和出口）的自动指标、日志和跟踪。

Istio 实现的关键组件是一种名为 [Envoy 代理](https://www.envoyproxy.io/docs/envoy/latest/intro/what_is_envoy)的代理服务。 它与每个服务一起运行，并为以下功能提供平台无关的基础：

- 动态服务发现。
- 负载均衡。
- TLS 终止。
- HTTP 和 gRPC 代理。
- 断路器复原。
- 运行状况检查。
- [用不](https://martinfowler.com/bliki/CanaryRelease.html)到的部署滚动更新。

如前文所述，Envoy 作为挎斗部署到群集中的每个微服务。

## <a name="integration-with-azure-kubernetes-services"></a>与 Azure Kubernetes 服务集成

Azure 云支持 Istio，并在 Azure Kubernetes 服务中提供对该服务的直接支持。 以下链接可帮助你入门：

- [在 AKS 中安装 Istio](/azure/aks/istio-install)
- [使用 AKS 和 Istio](/azure/aks/istio-scenario-routing)

### <a name="references"></a>参考

- [Polly](http://www.thepollyproject.org/)

- [重试模式](/azure/architecture/patterns/retry)

- [断路器模式](/azure/architecture/patterns/circuit-breaker)

- [Azure 白皮书中的复原能力](https://azure.microsoft.com/mediahandler/files/resourcefiles/resilience-in-azure-whitepaper/Resilience%20in%20Azure.pdf)

- [网络延迟](https://www.techopedia.com/definition/8553/network-latency)

- [冗余](/azure/architecture/guide/design-principles/redundancy)

- [异地复制](/azure/sql-database/sql-database-active-geo-replication)

- [Azure 流量管理器](/azure/traffic-manager/traffic-manager-overview)

- [自动缩放指南](/azure/architecture/best-practices/auto-scaling)

- [Istio](https://istio.io/docs/concepts/what-is-istio/)

- [Envoy 代理](https://www.envoyproxy.io/docs/envoy/latest/intro/what_is_envoy)

>[!div class="step-by-step"]
>[上一页](infrastructure-resiliency-azure.md)
>[下一页](monitoring-health.md)
