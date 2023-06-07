---
title: 在微服务（集成事件）之间实现基于事件的通信
description: 适用于容器化 .NET 应用程序的 .NET 微服务基础结构 | 了解集成事件以在微服务之间实现基于事件的通信。
ms.date: 10/02/2018
ms.openlocfilehash: a778acba3e17b084840b77d903533f9180ca01d9
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91152528"
---
# <a name="implementing-event-based-communication-between-microservices-integration-events"></a>在微服务（集成事件）之间实现基于事件的通信

如前所述，使用基于事件的通信时，当值得注意的事件发生时，微服务会发布事件，例如更新业务实体时。 其他微服务订阅这些事件。 微服务收到事件时，可以更新其自己的业务实体，这可能会导致发布更多事件。 这是最终一致性概念的本质。 通常通过使用事件总线实现来执行此发布/订阅系统。 事件总线可以设计为包含 API 的接口，该 API 是订阅和取消订阅事件和发布事件所需的。 它还可以包含一个或多个基于跨进程或消息通信的实现，例如支持异步通信和发布/订阅模型的消息队列或服务总线。

可以使用事件来实现跨多个服务的业务事务，这可提供这些服务间的最终一致性。 最终一致事务由一系列分布式操作组成。 在每个操作中，微服务会更新业务实体，并发布可触发下一个操作的事件。 下面的图 6-18 显示了通过事件总线发布了 PriceUpdated 事件，因此价格更新传播到购物篮和其他微服务。

![与事件总线进行异步事件驱动通信的关系图。](./media/integration-event-based-microservice-communications/event-driven-communication.png)

**图 6-18**。 基于事件总线的事件驱动的通信

本部分介绍如何使用通用事件总线接口（如图 6-18 所示）实现这种与 .NET 的通信。 存在多种可能的实现，每种实现使用不同的技术或基础结构，例如 RabbitMQ、Azure 服务总线或任何其他第三方开源或商用服务总线。

## <a name="using-message-brokers-and-services-buses-for-production-systems"></a>将消息中转站和服务总线用于生产系统

如体系结构部分所述，可从多个消息技术中选择，用于实现抽象事件总线。 但这些技术的级别不同。 例如，消息中转站传输 RabbitMQ 的级别比 Azure 服务总线、NServiceBus、MassTransit 或 Brighter 的级别低。 这些产品中的大部分可基于 RabbitMQ 或 Azure 服务总线运行。 你对产品的选择取决于针对应用程序需要多少功能和现成的可伸缩性。

如果只是为开发环境实现事件总线概念证明，那么正如 eShopOnContainers 示例所述，基于作为容器运行的 RabbitMQ 的简单实现可能已足够。 但对于任务关键型和需要高可伸缩性的生产系统，可能需要评估和使用 Azure 服务总线。

如果需要高级别抽象和更丰富的功能（如 [Sagas](https://docs.particular.net/nservicebus/sagas/)），用于简化分布式开发的长时间运行进程，则可评估 NServiceBus、MassTransit 和 Brighter 等其他商用和开源服务总线。 在此情况下，通常需要使用由高级别服务总线直接提供的抽象和 API，而不是你自己的抽象（例如 [eShopOnContainers](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/BuildingBlocks/EventBus/EventBus/Abstractions/IEventBus.cs) 提供的简单事件总线抽象）。 为此，可研究[使用 NServiceBus 的分叉 eShopOnContainer](https://go.particular.net/eShopOnContainers)（由特定软件实现的其他派生示例）。

当然，你可始终基于 RabbitMQ 和 Docker 等较低级别技术生成自己的服务总线功能，但“彻底改造”所需的工作可能对于自定义企业应用程序而言过于昂贵。

重申一下：eShopOnContainers 示例中展示的示例事件总线抽象和实现旨在仅用作概念证明。 一旦已决定要进行异步的事件驱动通信（如当前部分所述），应选择最适合你的生产需要的服务总线产品。

## <a name="integration-events"></a>集成事件

集成事件用于跨多个微服务或外部系统保持域状态同步。 这可通过在微服务外发布集成事件完成。 将事件发布到多个接收方微服务（订阅到集成事件的尽可能多个微服务）时，每个接收方微服务中的相应事件处理程序会处理该事件。

集成事件基本上是数据保持类，如以下示例所示：

```csharp
public class ProductPriceChangedIntegrationEvent : IntegrationEvent
{
    public int ProductId { get; private set; }
    public decimal NewPrice { get; private set; }
    public decimal OldPrice { get; private set; }

    public ProductPriceChangedIntegrationEvent(int productId, decimal newPrice,
        decimal oldPrice)
    {
        ProductId = productId;
        NewPrice = newPrice;
        OldPrice = oldPrice;
    }
}
```

可以在每个微服务的应用程序级别定义集成事件，以便将其从其他微服务分离，方式类似于在服务器和客户端中定义 ViewModel。 不建议跨多个微服务共享同一个集成事件库，这样做会导致这些微服务与一个事件定义数据库连接。 这与你不希望跨多个微服务共享同一个域模型的原因相同：微服务必须是完全独立的。

仅应跨微服务共享一些类型的库。 其中一种库是最终应用程序块，如 eShopOnContainer 中的[事件总线客户端 API](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/BuildingBlocks/EventBus)。 另一种库是组成可作为 NuGet 组件共享的工具的库，如 JSON 序列化程序。

## <a name="the-event-bus"></a>事件总线

事件总线可实现发布/订阅式通信，无需组件之间相互显式识别，如图 6-19 所示。

![显示基本发布/订阅模式的关系图。](./media/integration-event-based-microservice-communications/publish-subscribe-basics.png)

**图 6-19**。 事件总线的发布/订阅基础知识

上图显示了微服务 A 发布到事件总线，这会分发到订阅微服务 B 和 C，发布服务器无需知道订阅服务器。 事件总线与观察者模式和发布-订阅模式相关。

### <a name="observer-pattern"></a>观察者模式

在[观察者模式](https://en.wikipedia.org/wiki/Observer_pattern)中，主对象（称为可观察对象）将相关信息（事件）告知其他感兴趣的对象（称为观察者）。

### <a name="publishsubscribe-pubsub-pattern"></a>发布-订阅（发布/订阅）模式

[发布/订阅模式](/previous-versions/msp-n-p/ff649664(v=pandp.10))的用途与观察者模式相同：某些事件发生时，需要告知其他服务。 但观察者模式与发布/订阅模式之间存在重要区别。 在观察者模式中，直接从可观察对象广播到观察者，因此它们“知道”彼此。 但在发布/订阅模式中，存在称为中转站、消息中转站或事件总线的第三个组件，发布服务器和订阅服务器都知道第三个组件。 因此，使用发布/订阅模式时，发布服务器和订阅服务器通过所述的事件总线或消息中转站精确分离。

### <a name="the-middleman-or-event-bus"></a>中转站或事件总线

如何实现发布服务器和订阅服务器之间的匿名？ 一个简单方法是让中转站处理所有通信。 事件总线是一个这样的中转站。

事件总线通常由两部分组成：

- 抽象或接口。

- 一个或多个实现。

在图 6-19 中，从应用程序角度看，会发现事件总线实际上是一个发布/订阅通道。 实现此异步通信的方式可能会有差异。 它可以具有多个实现，以便你进行交换，具体取决于环境要求（例如，生产和开发环境）。

在图 6-20 中，可看到事件总线的抽象，包含基于 RabbitMQ、Azure 服务总线或其他事件/消息中转站等基础结构消息技术的多个实现。

![显示事件总线抽象层的添加关系图。](./media/integration-event-based-microservice-communications/multiple-implementations-event-bus.png)

**图 6- 20。** 事件总线的多个实现

最好通过接口定义事件总线，以便它可以使用多种技术（如 RabbitMQ Azure 服务总线或其他技术）实现。 但是，如前所述，仅当需要由你的抽象支持的基本事件总线功能时，才适合使用你自己的抽象（事件总线接口）。 如果需要更丰富的服务总线功能，应使用你喜欢的商用服务总线提供的 API 和抽象，而不是你自己的抽象。

### <a name="defining-an-event-bus-interface"></a>定义事件总线接口

首先，让我们了解一下事件总线接口的一些实现代码和可能的实现。 接口应是通用和简单的，如下所示接口。

```csharp
public interface IEventBus
{
    void Publish(IntegrationEvent @event);

    void Subscribe<T, TH>()
        where T : IntegrationEvent
        where TH : IIntegrationEventHandler<T>;

    void SubscribeDynamic<TH>(string eventName)
        where TH : IDynamicIntegrationEventHandler;

    void UnsubscribeDynamic<TH>(string eventName)
        where TH : IDynamicIntegrationEventHandler;

    void Unsubscribe<T, TH>()
        where TH : IIntegrationEventHandler<T>
        where T : IntegrationEvent;
}
```

`Publish` 方法很简单。 事件总线会向订阅该事件的任何微服务或外部应用程序，广播经过它的集成事件。 该方法由发布事件的微服务使用。

`Subscribe` 方法（你可能有多个实现，具体取决于参数）由要接收事件的微服务使用。 此方法具有两个参数。 第一个是要订阅的集成事件 (`IntegrationEvent`)。 第二个参数是名为 `IIntegrationEventHandler<T>` 的集成事件处理程序（或回调方法），用于在接收者微服务获得集成事件消息时执行。

## <a name="additional-resources"></a>其他资源

一些生产就绪型消息传输解决方案：

- **Azure 服务总线** \
  <https://docs.microsoft.com/azure/service-bus-messaging/>
  
- **NServiceBus** \
  <https://particular.net/nservicebus>
  
- **MassTransit** \
  <https://masstransit-project.com/>

> [!div class="step-by-step"]
> [上一页](database-server-container.md)
> [下一页](rabbitmq-event-bus-development-test-environment.md)
