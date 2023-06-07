---
title: 域事件。 设计和实现
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 深入了解域事件（在聚合之间建立通信的一个关键概念）。
ms.date: 10/08/2018
ms.openlocfilehash: 651d9cb98444c0729b97f523cc3d688f0f8d51d5
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173362"
---
# <a name="domain-events-design-and-implementation"></a>域事件：设计和实现

使用域事件显式实现域中的更改的副作用。 如果使用 DDD 术语表述，即使用域事件跨多个聚合显式实现副作用。 （可选）为了提高可伸缩性并减小对数据库锁定的影响，可在相同域的聚合之间使用最终一致性。

## <a name="what-is-a-domain-event"></a>什么是域事件？

事件是过去发生的事。 域事件是在域中发生的事，你希望同一个域（进程）的其他部分了解它。 通知部分通常以某种方式对事件作出反应。

域事件的一个重要优势是可以显式表示副作用。

例如，如果只使用实体框架，并且必须对某个事件作出反应，则可能会对需要向触发事件的内容关闭的任何内容进行编码。 因此规则会隐式地关联到代码，需要查看代码才有希望认识到在其中实现了规则。

另一方面，使用域事件会使概念明确，因为会涉及到 `DomainEvent` 以及至少一个 `DomainEventHandler`。

例如在 eShopOnContainers 应用程序中，当创建订单时，用户会成为买家，因此 `OrderStartedDomainEvent` 会被引发并在 `ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler` 中进行处理，这样基础概念便十分明显。

简言之，域事件可帮助根据域专家提供的通用语言显式表示域规则。 通过域事件还可更好地在相同域中的类之间分离关注点。

请务必确保与域事件相关的所有操作都成功完成，或是都未成功完成，正如数据库事务一样。

域事件类似于消息样式事件，但有一个重要的区别。 对于真正的消息传送、消息队列、消息中转站或使用 AMQP 的服务总线，消息始终异步发送，并跨进程和计算机传达。 这对于集成多个绑定上下文、微服务或不同应用程序很有用。 但是，对于域事件，需要从当前运行的域操作引发事件，但需要在相同的域中发生副作用。

域事件和副作用（过后触发的操作，由事件处理程序管理）应几乎立即发生，通常在进程内或在相同的域中。 因此，域事件可以是同步或异步。 但是集成事件应始终是异步。

## <a name="domain-events-versus-integration-events"></a>域事件和集成事件

从语义上看，域事件和集成事件是相同的：都是对已发生事件的通知。 但是，它们的实现必须不同。 域事件是推送到域事件调度程序的消息，可基于 IoC 容器或任何其他方法作为内存中转存进程实现。

但是，集成事件的目的是将已提交事务和更新传播到其他子系统，无论它们是其他微服务、绑定上下文，还是外部应用程序。 因此，它们应仅在成功保存实体时发生，否则便会如同整个操作从未发生一样。

如前所述，集成事件必须基于多个微服务（其他绑定上下文）或外部系统/应用程序之间的异步通信。

因此，事件总线接口需要一些基础结构来实现远程服务之间的进程间和分布式通信。 可以基于商用服务总线、队列、作为邮箱的共享数据库或任何其他分布式和基于推送的消息传送系统。

## <a name="domain-events-as-a-preferred-way-to-trigger-side-effects-across-multiple-aggregates-within-the-same-domain"></a>域事件是在相同域中跨多个聚合触发副作用的首选方式

如果执行与一个聚合实例相关的命令需要其他域规则运行一个或多个其他聚合，应将这些副作用设计和实现为由域事件触发。 如图 7-14 所示，这是一个最重要的用例之一，应将域事件用于在相同域模型中跨多个聚合传播状态更改。

![向买家聚合显示域事件控制的关系图。](./media/domain-events-design-implementation/domain-model-ordering-microservice.png)

**图 7-14**。 域事件在相同域的多个聚合之间强制执行一致性

图 7-14 显示了域事件如何实现聚合之间的一致性。 当用户发起订单时，订单聚合将发送 `OrderStarted` 域事件。 OrderStarted 域事件基于标识微服务中的原始用户信息（包含 CreateOrder 命令中提供的信息），由买方聚合处理，以在订购微服务时创建买家对象。

或者，可以使聚合根订阅由聚合（子实体）成员引发的事件。 例如，每个 OrderItem 子实体可以在项目价格高于特定金额，或产品项目金额过高时，引发事件。 然后，聚合根可以接收这些事件，并执行全局计算或聚合。

请注意，此基于事件的通信不会直接在聚合中实现，需要实现域事件处理程序。

处理域事件是一个应用程序问题。 域模型层应只关注域逻辑（域专家可理解的内容），而不应关注应用程序基础结构（如处理程序）和使用存储库的副作用持久性操作。 因此，应用程序层级别是在引发域事件时应执行域事件处理程序触发操作的位置。

域事件还可用于触发任意数量的应用程序操作，并且更重要的是，必须可在将来通过分离方式增加此数目。 例如，发起订单时，可能需要发布域事件以将信息传播到其他聚合，或引发通知等应用程序操作。

关键在于域事件发生时要执行的操作的可变数量。 最终，域中的操作和规则以及应用程序会发展。 事件发生时的复杂性或副作用操作的数量将增加，但如果代码被“固定”（即使用 `new` 创建特定对象），每当需要添加新操作时，还需要更改正在工作和经过测试的代码。

此更改可能会导致新 bug，而且此方法还会违背 [SOLID](https://en.wikipedia.org/wiki/SOLID) 的[开/闭原则](https://en.wikipedia.org/wiki/Open/closed_principle)。 不仅如此，协调操作的原始类将不断增加，这违背了[单一功能原则 (SRP)](https://en.wikipedia.org/wiki/Single_responsibility_principle)。

但是，如果使用域事件，可以通过使用以下方法分离功能来创建细化和分离的实现：

1. 发送命令（例如，CreateOrder）。
2. 在命令处理程序中接收命令。
   - 执行单个聚合的事务。
   - （可选）引发副作用的域事件（例如 OrderStartedDomainEvent）。
3. 处理域事件（在当前进程中），这些域事件会在多个聚合或应用程序操作中执行可变数量的副作用。 例如：
   - 验证或创建购买者和付款方式。
   - 创建相关集成事件并将其发送到事件总线，以在微服务中传播状态，或触发外部操作，例如将电子邮件发送给购买者。
   - 处理其他副作用。

如图 7-15 所示，从同一域事件，可以处理与域中的其他聚合相关的多个操作，或处理需要在与集成事件和事件总线相关的微服务中执行的其他应用程序操作。

![显示域事件将数据传递到几个事件处理程序的关系图。](./media/domain-events-design-implementation/aggregate-domain-event-handlers.png)

**图 7-15**。 处理每个域的多个操作

同一个域事件在应用层中可以有多个处理程序，一个处理程序可以解决聚合之间的一致性，另一个处理程序可以发布集成事件，以便其他微服务可以对它执行操作。 事件处理程序通常在应用程序层中，因为会将存储库或应用程序 API 等基础结构对象用于微服务行为。 在此意义上，事件处理程序类似于命令处理程序，因此两者都在应用程序层中。 两者的重要区别是命令应只处理一次。 域事件可处理零或 n 次，因为它可被多个接收方或事件处理程序接收，针对每个处理程序具有不同用途  。

借助每个域事件的可变数量的处理程序，可添加所需数量的域规则，而不会影响当前代码。 例如，可以轻松地添加一些（或者甚至是一个）事件处理程序来实现以下业务规则：

> 如果客户在商店购买的总金额（跨任意数量订单）超过 6,000 美元，则为每个新订单提供 10% 的折扣，并通过电子邮件告知客户将来订单的折扣。

## <a name="implement-domain-events"></a>实现域事件

在 C# 中，域事件只是数据保留结构或类（如 DTO），包含与域中发生的事件相关的所有信息，如以下示例所示：

```csharp
public class OrderStartedDomainEvent : INotification
{
    public string UserId { get; }
    public int CardTypeId { get; }
    public string CardNumber { get; }
    public string CardSecurityNumber { get; }
    public string CardHolderName { get; }
    public DateTime CardExpiration { get; }
    public Order Order { get; }

    public OrderStartedDomainEvent(Order order,
                                   int cardTypeId, string cardNumber,
                                   string cardSecurityNumber, string cardHolderName,
                                   DateTime cardExpiration)
    {
        Order = order;
        CardTypeId = cardTypeId;
        CardNumber = cardNumber;
        CardSecurityNumber = cardSecurityNumber;
        CardHolderName = cardHolderName;
        CardExpiration = cardExpiration;
    }
}
```

这实质上是一个包含与 OrderStarted 事件相关的所有数据的类。

关于域的通用语言，由于事件在过去发生的，事件的类名称应使用过去时态谓词表示，如 OrderStartedDomainEvent 或 OrderShippedDomainEvent。 这是域事件在 eShopOnContainers 的订购微服务中的实现方式。

如前文所述，事件的一个重要特性是：由于事件是在过去发生的，所以不应发生更改。 因此，它必须是一个不可变的类。 可以从上一个代码中看到，属性是只读的。 无法更新对象，只能在创建它时设置值。

这里必须重点强调的是，如果域事件要使用需要序列化和反序列化事件对象的队列进行异步处理，则属性必须是“专用集”而不是只读，以便反序列化程序能够在出队列时分配值。 这不是订购微服务中的问题，因为域事件发布/订阅是使用 MediatR 同步实现的。

### <a name="raise-domain-events"></a>引发域事件

下一个问题是如何引发域事件，使其到达相关的事件处理程序。 可使用多个方法。

Udi Dahan 最初建议（例如在 [Domain Events – Take 2](https://udidahan.com/2008/08/25/domain-events-take-2/)（域事件 – Take 2）等一系列文章中）将静态类用于管理和引发事件。 这可能包括名为 DomainEvents 的静态类，该类会在调用时，使用 `DomainEvents.Raise(Event myEvent)` 等语法立即引发域事件。 Jimmy Bogard 写了过一篇博客文章（[强化你的域：域事件](https://lostechies.com/jimmybogard/2010/04/08/strengthening-your-domain-domain-events/)），其中建议了类似的方法。

但是，如果域事件类是静态的，它也会立即调度给处理程序。 这使得测试和调试更加困难，因为会在引发事件后立即执行具有副作用逻辑的事件处理程序。 测试和调试时，你只希望专注于当前聚合类发生的事件；不希望突然重定向到其他事件处理程序，处理与其他聚合或应用程序逻辑相关的副作用。 因此，其他方法应运而生，下一节将进行介绍。

#### <a name="the-deferred-approach-to-raise-and-dispatch-events"></a>用于引发和调度事件的延迟方法

一个更好的方法是将域事件添加到集合，然后在提交事务之前或之后立即调度这些域事件（正如 EF 中的 SaveChanges），而不是立即调度到域事件处理程序    。 （Jimmy Bogard 的文章 [A better domain events pattern](https://lostechies.com/jimmybogard/2014/05/13/a-better-domain-events-pattern/)（一个更好的域事件模式）中介绍了此方法。）

确定要在提交事务之前还是之后立即发送域事件非常重要，因为这决定了是将副作用添加到相同事务，还是不同事务中。 如果是将副作用添加到不同事务，需要处理跨多个聚合的最终一致性。 下一节中将对此主题进行讨论。

eShopOnContainers 使用延迟方法。 首先，将实体中发生的事件添加到每个实体的集合或事件列表。 此列表应属于实体对象（或最好属于基本实体类），如以下实体基类示例所示：

```csharp
public abstract class Entity
{
     //...
     private List<INotification> _domainEvents;
     public List<INotification> DomainEvents => _domainEvents;

     public void AddDomainEvent(INotification eventItem)
     {
         _domainEvents = _domainEvents ?? new List<INotification>();
         _domainEvents.Add(eventItem);
     }

     public void RemoveDomainEvent(INotification eventItem)
     {
         _domainEvents?.Remove(eventItem);
     }
     //... Additional code
}
```

要引发事件时，只需将其在聚合根实体的方法处添加到代码中的事件集合。

以下代码（属于 [eShopOnContainers 的订单聚合根](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.Domain/AggregatesModel/OrderAggregate/Order.cs)）演示如下示例：

```csharp
var orderStartedDomainEvent = new OrderStartedDomainEvent(this, //Order object
                                                          cardTypeId, cardNumber,
                                                          cardSecurityNumber,
                                                          cardHolderName,
                                                          cardExpiration);
this.AddDomainEvent(orderStartedDomainEvent);
```

请注意 AddDomainEvent 方法的唯一功能是将事件添加到列表。 尚未调度任何事件，尚未调用任何事件处理程序。

你需要在稍后将事务提交到数据库时调度事件。 如果使用 Entity Framework Core，意味着在 EF DbContext 的 SaveChanges 方法中，如以下示例所示：

```csharp
// EF Core DbContext
public class OrderingContext : DbContext, IUnitOfWork
{
    // ...
    public async Task<bool> SaveEntitiesAsync(CancellationToken cancellationToken = default(CancellationToken))
    {
        // Dispatch Domain Events collection.
        // Choices:
        // A) Right BEFORE committing data (EF SaveChanges) into the DB. This makes
        // a single transaction including side effects from the domain event
        // handlers that are using the same DbContext with Scope lifetime
        // B) Right AFTER committing data (EF SaveChanges) into the DB. This makes
        // multiple transactions. You will need to handle eventual consistency and
        // compensatory actions in case of failures.
        await _mediator.DispatchDomainEventsAsync(this);

        // After this line runs, all the changes (from the Command Handler and Domain
        // event handlers) performed through the DbContext will be committed
        var result = await base.SaveChangesAsync();
    }
}
```

使用此代码可将实体事件调度到相应的事件处理程序。

总体结果是将域事件引用（简单添加到内存中列表）从调度到事件处理程序中分离。 此外，可同步或异步调度事件，具体取决于你使用的调度程序。

请注意事务边界在此时发挥巨大作用。 如果工作单元和事务可跨多个聚合（如使用 EF Core 和关系数据库时），这可正常运行。 但是，如果事务不能跨聚合（例如使用 Azure CosmosDB 等 NoSQL 数据库时），必须执行额外步骤以实现一致性。 这是持久性无感知不通用的另一个原因，它取决于你使用的存储系统。

### <a name="single-transaction-across-aggregates-versus-eventual-consistency-across-aggregates"></a>跨聚合的单个事务与跨聚合的最终一致性

是跨聚合执行单个事务，还是依赖跨聚合的最终一致性，这一点存在争议。 Eric Evans 和 Vaughn Vernon 等许多作者提倡遵循一个事务 = 一个聚合的规则，因此支持跨聚合的最终一致性。 例如，Eric Evans 在他的著作《Domain-Driven Design》（域驱动的设计）中表示  ：

> 跨聚合的任何规则都不会始终保持最新状态。 通过事件处理、批处理或其他更新机制，其他依赖项可在特定时间内解析。 （第 128 页）

Vaughn Vernon 在 [Effective Aggregate Design.第 II 部分：让聚合共同工作](https://dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_2.pdf)：

> 因此，如果在一个聚合实例上执行命令需要在一个或多个聚合上执行其他业务规则，则使用最终一致性。\[...\]有一个实用方法可在 DDD 模型中支持最终一致性。 聚合方法发布及时交付到一个或多个异步订阅服务器的域事件。

此观点基于接受细化事务，而不是跨多个聚合或实体的事务。 原理是在第二种情况下，具有高可伸缩性要求的大规模应用程序的数据库锁定的数量巨大。 接受高可伸缩性应用程序不需要多个聚合间的即时事务一致性的事实，有助于接受最终一致性的概念。 业务通常不需要原子更改，而且确定特定操作是否需要原子事务始终是域专家的职责。 如果操作始终需要多个聚合间的原子事务，应考虑聚合是否应该更大，或是否未正确设计。

但 Jimmy Bogard 等其他开发者和架构师赞成在多个聚合中跨越单个事务 - 但仅当这些额外聚合与相同原始命令的副作用相关时。 例如，在 [A better domain events pattern](https://lostechies.com/jimmybogard/2014/05/13/a-better-domain-events-pattern/)（更好的域事件模式）中，Bogard 表示：

> 通常情况下，我希望域事件的副作用发生在相同的逻辑事务中，但不一定是在引发域事件的同一作用域 \[...\]在提交事务前，将事件调度到相应的处理程序。

如果在提交原始事务前调度域事件，这是因为希望在相同事务中添加这些事件的副作用  。 例如，如果 EF DbContext SaveChanges 方法失败，事务会回退所有更改，包括由相关域事件处理程序实现的副作用操作。 这是因为默认情况下 DbContext 生存范围定义为“已设置范围”。 因此，DbContext 对象跨多个在相同范围或对象图中实例化的存储库对象共享。 在开发 Web API 或 MVC 应用时，这与 HttpRequest 范围一致。

实际上，这两种方法（单个原子事务和最终一致性）都是正确的。 实际上取决于你的域或业务要求，以及域专家的建议。 它还取决于所需的服务可缩放性（事务越细化，对数据库锁定的影响越小）。 它取决于你愿意在代码上进行的投资，由于最终一致性需要更复杂的代码以检测聚合中可能的不一致，且需要实现补偿操作。 请考虑以下情况：如果将更改提交到原始聚合，并且之后当调度事件时如果发生问题，事件处理程序无法提交副作用，则聚合之间将产生不一致。

若要执行补偿操作，可将域事件存储在其他数据库表格，使其属于原始事务。 然后，可通过批处理比较事件列表和聚合的当前状态，从而检测不一致并运行补偿操作。 补偿操作属于复杂主题，需要你进行深度分析，包括与业务用户和域专家进行讨论。

在任何情况下，你可以选择所需的方法。 但初始延迟方法（在提交前引发事件，从而使用单个事务）是使用 EF Core 和关系数据库时最简单的方法。 该方法易于实现且在许多业务情景中有效。 这也是 eShopOnContainers 的订购微服务中使用的方法。

但如何将这些事件调度到相应的事件处理程序？ 在上一个示例中看到的 `_mediator` 对象是什么？ 它与用于在事件与其事件处理程序之间映射的技术和项目有关。

### <a name="the-domain-event-dispatcher-mapping-from-events-to-event-handlers"></a>域事件调度程序：从事件映射到事件处理程序

可调度或发布事件后，需要用于发布事件的某种项目，以便每个相关处理程序可获取它，并基于该事件处理副作用。

一种方法是使用真正的消息传送系统或事件总线，可能基于服务总线，而不是内存中事件。 但是，对于第一种情况，真正的消息传送可能对于处理域事件来说用力过猛，因为你只需要处理相同进程（即相同域和应用程序层）中的事件。

将事件映射到多个事件处理程序的另一种方法是使用 IoC 容器中的类型注册，以便动态推断是否调度事件。 换而言之，需要知道什么事件处理程序需要获得特定事件。 图 7-16 显示此方法的简化方法。

![显示域事件调度程序向相应处理程序发送事件的关系图。](./media/domain-events-design-implementation/domain-event-dispatcher.png)

**图 7-16**。 使用 IoC 的域事件调度程序

可以构建所有联结和项目，自行实现此方法。 但是还可使用 [MediatR](https://github.com/jbogard/MediatR)（它实际上使用 IoC 容器）等可用库。 因此，可以直接使用预定义的接口和转存进程对象的发布/调度方法。

在代码中，首先需要在 IoC 容器中注册事件处理程序类型，如 [eShopOnContainers 订购服务](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Infrastructure/AutofacModules/MediatorModule.cs)处的以下示例所示：

```csharp
public class MediatorModule : Autofac.Module
{
    protected override void Load(ContainerBuilder builder)
    {
        // Other registrations ...
        // Register the DomainEventHandler classes (they implement IAsyncNotificationHandler<>)
        // in assembly holding the Domain Events
        builder.RegisterAssemblyTypes(typeof(ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler)
                                       .GetTypeInfo().Assembly)
                                         .AsClosedTypesOf(typeof(IAsyncNotificationHandler<>));
        // Other registrations ...
    }
}
```

代码首先通过查找保留处理程序（使用 typeof(ValidateOrAddBuyerAggregateWhenXxxx)，但你可能已经选择任何其他事件处理程序来查找此程序集）的程序集，识别包含域事件处理程序的程序集。 由于所有事件处理程序实现 IAsyncNotificationHandler 接口，代码仅搜索这些类型并注册所有事件处理程序。

### <a name="how-to-subscribe-to-domain-events"></a>如何订阅域事件

使用 MediatR 时，每个事件处理程序必须使用 INotificationHandler 接口的通用参数上提供的事件类型，如以下代码所示：

```csharp
public class ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler
  : IAsyncNotificationHandler<OrderStartedDomainEvent>
```

基于事件和事件处理程序之间的关系（可将其视为订阅），MediatR 项目可发现每个事件的所有事件处理程序并触发每个事件处理程序。

### <a name="how-to-handle-domain-events"></a>如何处理域事件

最后，事件处理程序通常实现使用基础结构存储库的应用程序层，以获取所需其他聚合并执行副作用域逻辑。 以下[ eShopOnContainers 中的域事件处理程序代码](https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.API/Application/DomainEventHandlers/OrderStartedEvent/ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler.cs)演示实现示例。

```csharp
public class ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler
                   : INotificationHandler<OrderStartedDomainEvent>
{
    private readonly ILoggerFactory _logger;
    private readonly IBuyerRepository<Buyer> _buyerRepository;
    private readonly IIdentityService _identityService;

    public ValidateOrAddBuyerAggregateWhenOrderStartedDomainEventHandler(
        ILoggerFactory logger,
        IBuyerRepository<Buyer> buyerRepository,
        IIdentityService identityService)
    {
        // ...Parameter validations...
    }

    public async Task Handle(OrderStartedDomainEvent orderStartedEvent)
    {
        var cardTypeId = (orderStartedEvent.CardTypeId != 0) ? orderStartedEvent.CardTypeId : 1;
        var userGuid = _identityService.GetUserIdentity();
        var buyer = await _buyerRepository.FindAsync(userGuid);
        bool buyerOriginallyExisted = (buyer == null) ? false : true;

        if (!buyerOriginallyExisted)
        {
            buyer = new Buyer(userGuid);
        }

        buyer.VerifyOrAddPaymentMethod(cardTypeId,
                                       $"Payment Method on {DateTime.UtcNow}",
                                       orderStartedEvent.CardNumber,
                                       orderStartedEvent.CardSecurityNumber,
                                       orderStartedEvent.CardHolderName,
                                       orderStartedEvent.CardExpiration,
                                       orderStartedEvent.Order.Id);

        var buyerUpdated = buyerOriginallyExisted ? _buyerRepository.Update(buyer)
                                                                      : _buyerRepository.Add(buyer);

        await _buyerRepository.UnitOfWork
                .SaveEntitiesAsync();

        // Logging code using buyerUpdated info, etc.
    }
}
```

前面的域事件处理程序代码被视为应用程序层代码，因为其使用基础结构存储库，如有关基础结构持久化层的下一节所述。 事件处理程序还可以使用其他基础结构组件。

#### <a name="domain-events-can-generate-integration-events-to-be-published-outside-of-the-microservice-boundaries"></a>域事件可以生成要在微服务边界之外发布的集成事件

最后，请注意，有时需要跨多个微服务传播事件。 该传播是集成事件，可通过特定域事件处理程序的事件总线发布它。

## <a name="conclusions-on-domain-events"></a>域事件的结论

如前文所述，使用域事件显式实现域中的更改的副作用。 若要使用 DDD 术语表述，则是使用域事件跨一个或多个聚合显式实现副作用。 此外，为了提高可伸缩性并减小对数据库锁定的影响，可在相同域的聚合之间使用最终一致性。

参考应用使用 [MediatR](https://github.com/jbogard/MediatR) 在单个事务中跨聚合同步传播域事件。 但是，也可使用 [RabbitMQ](https://www.rabbitmq.com/) 或 [Azure 服务总线](/azure/service-bus-messaging/service-bus-messaging-overview)等一些 AMQP 实现来异步传播域事件，从而运用最终一致性，但如上所述，必须考虑到失败时要执行补偿操作的需求。

## <a name="additional-resources"></a>其他资源

- **Greg Young.What is a Domain Event?** （什么是域事件？） \
  <https://cqrs.files.wordpress.com/2010/11/cqrs_documents.pdf#page=25>

- **Jan Stenberg。Domain Events and Eventual Consistency** （域事件和最终一致性） \
  <https://www.infoq.com/news/2015/09/domain-events-consistency>

- **Jimmy Bogard。A better domain events pattern** （更好的域事件模式） \
  <https://lostechies.com/jimmybogard/2014/05/13/a-better-domain-events-pattern/>

- **Vaughn Vernon。有效聚合设计，第 II 部分：让聚合共同工作** \
  [https://dddcommunity.org/wp-content/uploads/files/pdf\_articles/Vernon\_2011\_2.pdf](https://dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_2.pdf)

- **Jimmy Bogard。强化域：域事件** \
  <https://lostechies.com/jimmybogard/2010/04/08/strengthening-your-domain-domain-events/>

- **Tony Truong。Domain Events Pattern Example** （域事件模式示例） \
  <https://www.tonytruong.net/domain-events-pattern-example/>

- **Udi Dahan.How to create fully encapsulated Domain Models** （如何创建完全封装的域模型） \
  <https://udidahan.com/2008/02/29/how-to-create-fully-encapsulated-domain-models/>

- **Udi Dahan.Domain Events – Take 2** （域事件 – 搞定两个问题） \
  <https://udidahan.com/2008/08/25/domain-events-take-2/>

- **Udi Dahan.Domain Events – Salvation** （域事件 – 救助） \
  <https://udidahan.com/2009/06/14/domain-events-salvation/>

- **Jan Kronquist。Don't publish Domain Events, return them!** （不要发布域事件，返回它们！） \
  <https://blog.jayway.com/2013/06/20/dont-publish-domain-events-return-them/>

- **Cesar de la Torre。Domain Events vs.Integration Events in DDD and microservices architectures** （DDD 和微服务体系结构中的域事件和集成事件） \
  <https://devblogs.microsoft.com/cesardelatorre/domain-events-vs-integration-events-in-domain-driven-design-and-microservices-architectures/>

>[!div class="step-by-step"]
>[上一页](client-side-validation.md)
>[下一页](infrastructure-persistence-layer-design.md)
