---
title: 基于消息的异步通信
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 基于消息的异步通信是微服务体系结构中的一个重要概念，因为它是保持微服务彼此独立的同时使其最终同步的最佳方式。
ms.date: 09/20/2018
ms.openlocfilehash: 17b3fb3fe3f94d5387359061e3297ebfa6e5be7a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91169240"
---
# <a name="asynchronous-message-based-communication"></a>基于消息的异步通信

在多个微服务及其相关域模型中传播更改时，异步消息传递和事件驱动通信至关重要。 正如前面讨论微服务和绑定上下文 (BC) 时提到的那样，模型（用户、客户、产品、帐户等）对于不同微服务或 BC 的含义可能不同。 这意味着当发生更改时，需要通过某种方式来协调不同模型之间的更改。 其中一个解决方案就是基于异步消息传递的最终一致性和事件驱动通信。

使用消息传递时，各进程通过异步交换消息进行通信。 客户端通过向服务发送消息来向其发出命令或请求。 如果服务需进行回复，它会向客户端发回一条不同的消息。 由于这是一种基于消息的通信，因此客户端认为不会立即收到回复，甚至有可能根本就不会有回复。

消息由标头（标识或安全信息等元数据）和主体组成。 消息通常通过异步协议（如 AMQP）发送。

在微服务社区中，此类通信的首选基础结构是轻型消息代理，它与 SOA 中使用的大型代理和业务流程协调程序不同。 在轻型消息代理中，基础结构通常“很笨”，只能充当消息代理，并且包含简单的实现，例如 RabbitMQ 或云中的可缩放服务总线（如 Azure 服务总线）。 在此方案中，大多数“聪明”的想法仍然停留在生成和使用消息的终结点（也就是微服务）中。

另一个应尽量遵循的规则是，仅在内部服务之间使用异步消息传递，以及仅从客户端应用到前端服务（API 网关以及第一级微服务）使用同步通信（例如 HTTP）。

有两种异步消息通信：基于消息的单接收者通信和基于消息的多接收者通信。 以下部分提供了有关这两种通信的详细信息。

## <a name="single-receiver-message-based-communication"></a>基于消息的单接收者通信

具有单个接收者的基于消息的异步通信意味着存在点到点通信，即，仅向正在从该通道读取数据的一个使用者传递消息，并且该消息仅处理一次。 不过也有一些特殊情况。 例如，在试图从故障中自动恢复的云系统中，可以多次发送相同的消息。 由于网络或其他故障，客户端必须能够重试发送消息，并且服务器必须实现幂等操作，以便对特定消息仅处理一次。

基于消息的单接收者通信特别适用于将异步命令从一个微服务发送到另一个微服务，图 4-18 便阐释了这种方法。

一旦开始发送基于消息的通信（通过命令或事件），就应该避免将基于消息的通信与同步 HTTP 通信混合在一起。

![接收异步消息的单个微服务](./media/asynchronous-message-based-communication/single-receiver-message-based-communication.png)

**图 4-18**. 接收异步消息的单个微服务

当命令来自客户端应用程序时，它们可以作为 HTTP 同步命令实现。 如果需要更高的可伸缩性或者已经处于基于消息的业务流程中，使用基于消息的命令。

## <a name="multiple-receivers-message-based-communication"></a>基于消息的多接收者通信

作为一种更灵活的方法，你可能还需要使用发布/订阅机制，以便其他订阅者微服务或外部应用程序能够收到发送者发送的通信。 因此，它可以帮助你遵循发送服务中的[开/闭原理](https://en.wikipedia.org/wiki/Open/closed_principle)。 这样一来，以后无需修改发送者服务也可添加额外的订阅者。

使用发布/订阅通信时，你可能会使用事件总线接口向任何订阅者发布事件。

## <a name="asynchronous-event-driven-communication"></a>事件驱动的异步通信

使用事件驱动的异步通信时，微服务会在其域中发生变化时发布集成事件，而另一个微服务需要知道该变化，例如产品目录微服务中的价格变化。 其他微服务会订阅这些事件，以便以异步方式接收它们。 发生这种情况时，接收者可能会更新自己的域实体，这可能会导致发布更多的集成事件。 通常通过使用事件总线实现来执行此发布/订阅系统。 事件总线可以设计成包含 API 的抽象或接口，订阅或取消订阅事件以及发布事件时需使用该 API。 事件总线还可以包含一个或多个基于任意内部进程和消息传递代理的实现，例如支持异步通信和发布/订阅模型的消息传递队列或服务总线。

如果系统使用受集成事件驱动的最终一致性，建议让最终用户完全清楚这种方法。 系统不应使用模拟集成事件的方法，例如 SignalR 或客户端轮询系统。 最终用户和业务所有者必须显式接受系统中的最终一致性，并意识到在许多情况下，只要显式使用该方法，业务就不会有任何问题。 这一点很重要，因为用户可能希望立即看到一些结果，但在使用最终一致性时可能无法实现。

如前面的[分布式数据管理挑战和解决方案](distributed-data-management.md)部分所述，可以使用集成事件来实现跨多个微服务的业务任务。 这样就可以在这些服务之间实现最终一致性。 最终一致事务由一组分布式操作组成。 在执行每个操作时，相关微服务会更新域实体，并发布另一个集成事件，以便在相同的端到端业务任务中引发下一个操作。

很重要的一点是，你可能想要与订阅同一事件的多个微服务进行通信。 为此，可以使用基于事件驱动通信的发布/订阅消息传递，如图 4-19 所示。 这种发布/订阅机制并不是微服务体系结构所独有的。 它类似于 DDD 中[界定上下文](https://martinfowler.com/bliki/BoundedContext.html)的通信方式，或者类似于在[命令和查询责任分离 (CQRS)](https://martinfowler.com/bliki/CQRS.html) 体系结构模式中将更新从写入数据库传播到读取数据库的方式。 其目标是在分布式系统中的多个数据源之间实现最终一致性。

![显示异步事件驱动的通信的示意图。](./media/asynchronous-message-based-communication/asynchronous-event-driven-communication.png)

**图 4-19**. 事件驱动的异步消息通信

在事件驱动的异步通信中，一个微服务将事件发布到事件总线，许多微服务可以订阅它，以获取通知并对其进行操作。 你的实现将决定要用于基于消息的事件驱动通信的协议。 [AMQP](https://en.wikipedia.org/wiki/Advanced_Message_Queuing_Protocol) 有助于实现可靠的排队通信。

使用事件总线时，你可能希望将基于类中相关实现的抽象级别（如事件总线接口）与使用 API（来自消息代理，如 [RabbitMQ](https://www.rabbitmq.com/)，或来自服务总线，如 [Azure 服务总线及主题](/azure/service-bus-messaging/service-bus-dotnet-how-to-use-topics-subscriptions)）的代码结合使用。 或者，你可能希望使用 NServiceBus、MassTransit 或 Brighter 等更高级别的服务总线来清楚地表述你的事件总线和发布/订阅系统。

## <a name="a-note-about-messaging-technologies-for-production-systems"></a>关于生产系统消息传递技术的说明

可用于实现抽象事件总线的消息传递技术的级别不尽相同。 例如，RabbitMQ（一种消息传递代理传输）和 Azure 服务总线等产品的级别低于可在 RabbitMQ 和 Azure 服务总线之上运行的 NServiceBus、MassTransit 或 Brighter 等其他产品的级别。 你的选择取决于应用程序级别的丰富功能数量以及应用程序所需的现成可伸缩性。 如果只是为开发环境实现事件总线概念证明，那么正如我们在 eShopOnContainers 示例中所做的那样，基于 Docker 容器上运行的 RabbitMQ 的简单实现可能就足够了。

但对于需要超高可伸缩性的任务关键系统和生产系统，可能需要评估 Azure 服务总线。 对于可简化分布式应用程序开发的高级抽象和功能，建议评估其他商用和开源服务总线，例如 NServiceBus、MassTransit 和 Brighter。 当然，你也可以在 RabbitMQ 和 Docker 等较低级别技术的基础上构建自己的服务总线功能。 但是，对于自定义企业应用程序而言，这种管道工作的成本可能太高。

## <a name="resiliently-publishing-to-the-event-bus"></a>弹性地发布到事件总线

在多个微服务中实现事件驱动体系结构时面临的挑战是，如何在原始微服务中以原子方式更新状态，同时将其相关集成事件弹性地发布到事件总线中（在某种程度上基于事务）。 下面介绍了实现此操作的几种方法，但也可能有其他方法。

- 使用事务（基于 DTC）队列，如 MSMQ。 （不过，这是一种传统方法。）

- 使用[事务日志挖掘](https://www.scoop.it/t/sql-server-transaction-log-mining)。

- 使用完整[事件溯源](/azure/architecture/patterns/event-sourcing)模式。

- 使用[发件箱模式](https://www.kamilgrzybek.com/design/the-outbox-pattern/)：将用作消息队列的事务数据库表作为事件创建器组件的基础，该组件将创建并发布事件。

使用异步通信时需要考虑的其他主题包括消息幂等性和重复消息删除。 这些主题将在本指南后面的[在微服务（集成事件）之间实现基于事件的通信](../multi-container-microservice-net-applications/integration-event-based-microservice-communications.md)部分中进行介绍。

## <a name="additional-resources"></a>其他资源

- **事件驱动的消息传递** \
  <https://patterns.arcitura.com/soa-patterns/design_patterns/event_driven_messaging>

- **发布/订阅通道** \
  <https://www.enterpriseintegrationpatterns.com/patterns/messaging/PublishSubscribeChannel.html>

- **Udi Dahan.明确的 CQRS** \
  <https://udidahan.com/2009/12/09/clarified-cqrs/>

- **命令和查询责任分离 (CQRS)**  \
  <https://docs.microsoft.com/azure/architecture/patterns/cqrs>

- **界定上下文之间的通信** \
  <https://docs.microsoft.com/previous-versions/msp-n-p/jj591572(v=pandp.10)>

- **最终一致性** \
  <https://en.wikipedia.org/wiki/Eventual_consistency>

- **Jimmy Bogard。重构复原能力：评估耦合度** \
  <https://jimmybogard.com/refactoring-towards-resilience-evaluating-coupling/>

> [!div class="step-by-step"]
> [上一页](communication-in-microservice-architecture.md)
> [下一页](maintain-microservice-apis.md)
