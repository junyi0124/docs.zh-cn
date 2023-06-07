---
title: 处理部分失败错误
description: 了解如何妥善处理部分失败错误。 微服务可能无法完全正常运行，但仍可以执行一些有用的工作。
ms.date: 10/16/2018
ms.openlocfilehash: 3345fffe3a38b8336d71ebb9c88e76d3315fd2e2
ms.sourcegitcommit: a8730298170b8d96b4272e0c3dfc9819c606947b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2020
ms.locfileid: "90738757"
---
# <a name="handle-partial-failure"></a>处理部分失败错误

在基于微服务的应用程序这类分布式系统中，经常会出现部分失败错误。 例如，单个微服务/容器可能会失败，也可能无法在短时间响应，或者单个 VM 或服务器会出现故障。 由于客户端和服务是彼此独立的流程，因此服务可能无法及时响应客户端的请求。 服务可能过载并且对请求的响应速度过慢，或者只是由于网络问题在短时间内无法访问。

例如，请查看 eShopOnContainers 示例应用程序的订单详细信息页。 如果订购微服务在用户尝试提交订单时没有响应，则客户端进程（MVC Web 应用程序）的错误实现（例如，如果客户端代码使用同步 RPC 而没有出现超时）将会无限期地阻止线程等待回应。 每个无响应的等待除了会造成不良用户体验之外，还会消耗或阻止线程，然而线程在高度可缩放应用程序中极有价值。 如果受阻止的线程数量众多，应用程序的运行时最终会耗尽所有线程。 在这种情况下，应用程序会出现全局无响应，而不只是部分无响应，如图 8-1 中所示。

![显示部分失败的关系图。](./media/handle-partial-failure/partial-failures-diagram.png)

**图 8-1**。 因依赖性影响服务线程可用性而出现的部分失败错误

在基于微服务的大型应用程序中，任何部分失败错误都可能被放大，尤其是在大多数内部微服务交互都基于同步 HTTP 调用的情况下（这种情况被视为反模式）。 想想一个每天接收上百万传入调用的系统。 如果系统采用基于同步 HTTP 调用长链的不当设计，那么这些传入调用可能会再产生数百万对数十个内部微服务的传出调用（假设比例为 1:4）作为同步依赖项。 图 8-2 中显示了这种情况，尤其是依赖项 \#3，它启动链，调用依赖项 #4，该依赖项又调用 #5。

![显示多个分布式依赖项的关系图。](./media/handle-partial-failure/multiple-distributed-dependencies.png)

**图 8-2**。 采用 HTTP 请求长链的不当设计所产生的影响

即使每个依赖项本身都具有卓越的可用性，在分布式和基于云的系统中也一定会出现间歇性故障。 这是需要考虑的事实。

如果不设计和实现能保证容错的技术，那么即使是很短的故障时间也会被放大。 例如，50 个依赖项，每个具有 99.99% 的可用性，因为这种连锁效应，每个月会产生几个小时的故障时间。 如果微服务依赖项在处理大量请求时出现失败错误，那么该失败错误可能会迅速蔓延到每个服务中的所有可用请求线程，然后导致整个应用程序出现故障。

![显示微服务中放大的部分失败的关系图。](./media/handle-partial-failure/partial-failure-amplified-microservices.png)

**图 8-3**。 由微服务使用同步 HTTP 调用长链放大的部分失败错误

为最大程度地减少此问题，在此指南的[异步微服务集成强制实现微服务自治性](../architect-microservice-container-applications/communication-in-microservice-architecture.md#asynchronous-microservice-integration-enforces-microservices-autonomy)部分中，建议在内部微服务间使用异步通信。

此外，必须要设计可处理部分失败错误的微服务和客户端应用程序 - 即构建复原微服务和客户端应用程序。

>[!div class="step-by-step"]
>[上一页](index.md)
>[下一页](partial-failure-strategies.md)
