---
title: 微服务体系结构中的通信
description: 探索微服务之间的不同通信方式，了解同步和异步方法的含义。
ms.date: 01/30/2020
ms.openlocfilehash: f1a240609b898fe8f365c39ba0c95f486377c445
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91169253"
---
# <a name="communication-in-a-microservice-architecture"></a>微服务体系结构中的通信

在单个进程上运行的整体式应用程序中，组件使用语言级别方法或函数调用进行相互调用。 如果使用代码创建对象（例如 `new ClassName()`），则可以紧密耦合，或者如果正在通过引用抽象而不是具体对象实例来使用依赖项，则可以在耦合方法中调用。 两种方式中的对象都在同一进程中运行。 从整体式应用程序更改为基于微服务的应用程序时，最大的挑战在于改变通信机制。 将进程内的方法调用直接转换为对服务的 RPC 调用将在分布式环境中导致效果不佳且效率低下的通信。 正确设计分布式系统的挑战已众所周知，甚至有一个被称为[分布式计算的谬论](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing)的标准，其中列出了开发者在从整体式设计转向分布式设计时经常会做出的假设。

解决方案不止一种，而是多种。 一个解决方案包括尽可能多地隔离业务微服务。 然后，使用内部微服务之间的异步通信，并用对象间的进程内通信中典型的细粒度通信代替粗粒度通信。 执行此操作的方法有分组调用、将聚合多个内部调用结果的数据返回到客户端。

基于微服务的应用程序是在多个进程或服务上运行的分布式系统，通常甚至跨多个服务器或主机。 每个服务实例通常是一个进程。 因此，服务必须使用进程内通信协议（如 HTTP、AMQP）或二进制协议（如 TCP）进行交互，具体取决于每个服务的性质。

微服务社区倡导“[智能终结点和哑管道](https://simplicable.com/new/smart-endpoints-and-dumb-pipes)”的理念。这个标语鼓励设计在微服务之间尽可能分离，在单个微服务内尽可能聚合。 如前所述，每个微服务拥有自己的数据和域逻辑。 但构成端到端应用程序的微服务通常通过使用 REST 通信而不是复杂协议（如 WS-\*）以及灵活的事件驱动通信而不是集中式业务流程协调程序来简化编排。

两个常用的协议是具有资源 API 的 HTTP 请求/响应（查询大部分时）和轻量级异步消息传送（跨多个微服务更新通信时）。 以下各节将对此进行更详细地描述。

## <a name="communication-types"></a>通信类型

客户端和服务可以通过许多不同类型的通信进行通信，每个通信面向不同的方案和目标。 最初，这些类型的通信可以分为两个轴。

第一个轴定义协议是同步还是异步：

- 同步协议。 HTTP 是同步协议。 客户端发送请求并等待服务响应。 这与客户端代码执行无关，可能是同步的（线程被阻止）或异步的（线程没有被阻止，并且响应最终会到达回调）。 重要的一点是，协议 (HTTP/HTTPS) 是同步的，仅当客户端代码接收到 HTTP 服务器响应时，才可以继续其任务。

- 异步协议。 AMQP 之类的其他协议（许多操作系统和云环境支持的协议）使用异步消息。 客户端代码或消息发件人通常不会等待响应。 它只是在发送消息到 RabbitMQ 队列或任何其他消息代理时才发送消息。

第二个轴定义通信具有单个还是多个接收方：

- 单个接收方。 每个请求必须只能由一个接收方或服务来处理。 此通信的示例是[命令模式](https://en.wikipedia.org/wiki/Command_pattern)。

- 多个接收方。 每个请求可以由零到多个接收方处理。 这种类型的通信必须是异步的。 例如[事件驱动体系结构](https://microservices.io/patterns/data/event-driven-architecture.html)等模式中使用的[发布/订阅](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern)机制。 当通过事件在多个微服务之间传播数据更新时，这基于事件总线接口或消息代理；它通常通过服务总线或类似 [Azure 服务总线](https://azure.microsoft.com/services/service-bus/)的项目使用[主题和订阅](/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)并来实现。

基于微服务的应用程序通常将使用这些通信样式的组合。 最常见的类型是在调用常规 Web API HTTP 服务时使用 HTTP/HTTPS 等同步协议进行单个接收方通信。 微服务通常也使用消息传送协议在微服务之间进行异步通信。

必须知道这些轴以便清楚了解可能的通信机制，但在构建微服务时，它们就不是那么重要了。 集成微服务时，客户端线程执行的异步特性和所选协议的异步特性都不是重点。 重要的是  能够异步集成微服务，同时保持微服务独立性，如下节所述。

## <a name="asynchronous-microservice-integration-enforces-microservices-autonomy"></a>异步微服务集成强化了微服务的自治性

如前所述，生成基于微服务的应用程序时，重要的是集成微服务的方法。 理想情况下，应尝试减少内部微服务之间的通信。 微服务间的通信越少越好。 但在许多情况下，必须以某种方式集成微服务。 当需要执行此操作时，关键的规则是微服务间的通信应为异步。 这并不代表必须使用特定协议（例如，异步消息传送与同步 HTTP）。 这仅代表微服务之间的通信应该只通过异步传播数据来完成，但不要依赖其他内部微服务作为初始服务的 HTTP 请求/响应操作的一部分。

如果可能，永远不要依赖于多个微服务之间的同步通信（请求/响应），对于查询也是如此。 即使属于端到端应用程序的其他服务已关闭或不正常，每个微服务的目标也是自治，并且可供客户使用。 如果认为需要从一个微服务向其他微服务（如执行数据查询的 HTTP 请求）进行调用，以便能够向客户端应用程序提供响应，那么当某些微服务失败时体系结构不会迅速恢复。

此外，如图 4-15 的第一部分所示，微服务之间存在 HTTP 依赖项（如使用 HTTP 请求链创建长请求/响应周期时），不仅会使微服务不自治，而且只要该链中的其中一项服务表现不佳，性能就会受到影响。

在微服务间添加的同步依赖项（如查询请求）越多，客户端应用的整体响应时间就越长。

![显示微服务中的三种通信类型的关系图。](./media/communication-in-microservice-architecture/sync-vs-async-patterns-across-microservices.png)

**图 4-15**。 微服务间通信的反模式和模式

如上图所示，在同步通信中，在处理客户端请求的同时，在微服务之间创建请求“链”。 这是一种反模式。 在异步通信中，微服务使用异步消息或 http 轮询与其他微服务通信，但是会立即处理客户端请求。

如果微服务需要在另一个微服务中引发其他操作，请尽可能不要在原始微服务请求和回复操作中同步执行该操作。 而是以异步方式执行（使用异步消息传送或集成事件、队列等）。 但尽可能不要在原始同步请求和回复操作中同步调用操作。

最后（构建微服务时大多数问题会在这里发生），如果初始微服务需要最初由其他微服务拥有的数据，则不要依赖于对该数据进行同步请求。 而是通过使用最终一致性（通常使用集成事件，如后面几节所述），将该数据（仅需要的属性）复制或传播到初始服务的数据库中。

如前面的[标识每个微服务的域模型边界](identify-microservice-domain-model-boundaries.md)部分中所述，跨多个微服务复制某些数据不是设计错误 - 相反，此操作可以将数据翻译成该附加域或有边界的上下文的特定语言或术语。 例如，在 [eShopOnContainers 应用程序](https://github.com/dotnet-architecture/eShopOnContainers)中，有一个名为 `identity-api` 的微服务，它负责名为 `User` 的实体中的大部分用户数据。 但在需要存储 `Ordering` 微服务内的用户数据时，将其存储为名为 `Buyer` 的其他实体。 `Buyer` 实体与原始 `User` 实体共享相同的身份，但它可能只具有 `Ordering` 域所需的少量属性，而不是整个用户配置文件。

可以使用任何协议在微服务之间异步通信和传播数据，以实现最终的一致性。 如前所述，可以通过事件总线或消息代理使用集成事件，或者甚至可以通过轮询其他服务使用 HTTP。 这并不重要。 重要的规则是不在微服务之间创建同步依赖项。

以下各节介绍了可以考虑在基于微服务的应用程序中使用的多种通信样式。

## <a name="communication-styles"></a>通信样式

根据想要使用的通信类型，有许多可以用于通信的协议和选项。 如果正在使用基于同步请求/响应的通信机制，则 HTTP 和 REST 等协议方法是最常见的，尤其是将服务发布到 Docker 主机或微服务集群之外的情况下。 如果在内部进行服务间的通信（在 Docker 主机或微服务群集中），则还建议使用二进制格式通信机制（例如使用 TCP 和二进制格式的 WCF）。 或者，可以使用基于消息的异步通信机制，如 AMQP。

还有 JSON 或 XML 等多种消息格式，或者还有更高效的二进制格式。 如果选择的二进制格式不是标准格式，那么使用该格式公开发布服务可能并不适合。 可以使用非标准格式在微服务之间进行内部通信。 在 Docker 主机或微服务群集（例如 Docker 业务流程协调程序）中的微服务之间进行通信时，或与微服务通信的专用客户端应用程序进行通信时，可能需要执行此操作。

### <a name="requestresponse-communication-with-http-and-rest"></a>使用 HTTP 和 REST 进行请求/响应通信

当客户端使用请求/响应通信时，它将请求发送到服务，然后服务处理请求并返回响应。 请求/响应通信特别适用于查询客户端应用的实时 UI（实时用户界面）数据。 因此，在微服务体系结构中，此通信机制可能会用于大多数查询，如图 4-16 所示。

![显示实时查询和更新的请求/响应通信的关系图。](./media/communication-in-microservice-architecture/request-response-comms-live-queries-updates.png)

**图 4-16**。 使用 HTTP 请求/响应通信（同步或异步）

当客户端使用请求/响应通信时，假定响应将在短时间内（通常少于一秒或最多几秒）到达。 为了延迟响应，需要根据[消息传送模式](/azure/architecture/patterns/category/messaging)和[消息技术](https://en.wikipedia.org/wiki/Message-oriented_middleware)实现异步通信，这是我们在下一节中介绍的另一种方法。

请求/响应通信的常用体系结构样式是 [REST](https://en.wikipedia.org/wiki/Representational_state_transfer)。 此方法基于 [HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol) 协议并与该协议紧密耦合，接受 GET、POST 和 PUT 等 HTTP 谓词。 REST 是创建服务时最常用的体系结构通信方法。 开发 ASP.NET Core Web API 服务时，可以实现 REST 服务。

使用 HTTP REST 服务作为接口定义语言时，还有其他值。 例如，如果使用 [Swagger 元数据](https://swagger.io/)介绍服务 API，则可以使用生成客户端存根的工具来直接发现和使用服务。

### <a name="additional-resources"></a>其他资源

- **Martin Fowler。Richardson 成熟度模型** REST 模型的说明。 \
  <https://martinfowler.com/articles/richardsonMaturityModel.html>

- **Swagger** 官方网站。 \
  <https://swagger.io/>

### <a name="push-and-real-time-communication-based-on-http"></a>基于 HTTP 的推送和实时通信

另一种可能（通常用于不同于 REST 的目的）是与更高级别框架（如 [ASP.NET SignalR](https://www.asp.net/signalr)）和协议（如[WebSocket](https://en.wikipedia.org/wiki/WebSocket)）之间的实时和一对多通信。

如图 4-17 所示，实时 HTTP 通信意味着可以让服务器代码在数据可用时将内容推送到连接的客户端，而不是让服务器等待客户端请求新数据。

![显示基于 SignalR 的推送和实时通信的关系图。](./media/communication-in-microservice-architecture/one-to-many-communication.png)

**图 4-17**。 一对一实时异步消息通信

SignalR 是实现用于将内容从后端服务器推送到客户端的实时通信的好方法。 由于通信是实时的，客户端应用几乎立即显示更改。 这通常由 WebSocket 之类的协议使用多个 WebSocket 连接（每个客户端一个）处理。 一个典型示例是将体育比赛的比分变化同时传送到多个客户端 Web 应用。

>[!div class="step-by-step"]
>[上一页](direct-client-to-microservice-communication-versus-the-api-gateway-pattern.md)
>[下一页](asynchronous-message-based-communication.md)
