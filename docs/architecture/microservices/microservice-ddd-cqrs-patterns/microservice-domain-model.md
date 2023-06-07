---
title: 设计微服务域模型
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 了解设计面向 DDD 的域模型时的关键概念。
ms.date: 01/30/2020
ms.openlocfilehash: fe78e719570d5758b71531beab883e5c24a88dca
ms.sourcegitcommit: 5280b2aef60a1ed99002dba44e4b9e7f6c830604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/03/2020
ms.locfileid: "84306900"
---
# <a name="design-a-microservice-domain-model"></a>设计微服务域模型

为每个业务微服务或绑定上下文定义一个丰富域模型。

你的目标是为每个业务微服务或边界上下文 (BC) 创建一个内聚域模型。 但请记住，BC 或业务微服务有时可能由共享一个域模型的多个物理服务组成。 域模型必须捕获它所代表的单个绑定上下文或业务微服务的规则、行为、业务语言和约束。

## <a name="the-domain-entity-pattern"></a>域实体模式

实体表示域对象，主要由其标识、连续性和随时间推移的持久性来定义，而不仅仅由构成它们的属性来定义。 正如 Eric Evans 所说，“主要由其标识定义的对象称为实体”。 实体在域模型中非常重要，因为它们是模型的基础。 因此，应对其进行仔细识别和设计。

实体的标识可以跨多个微服务或绑定上下文。

同一标识（即，同一 `Id`，不过可能不是同一域实体）可以跨多个绑定上下文或微服务建模。 不过，这并不意味着具有相同属性和逻辑的相同实体会在多个绑定上下文中实现。 相反，每个绑定上下文中的实体都会将其属性和行为限制为该绑定上下文域中所需的属性和行为。

例如，买家实体可能具有某个人的大部分属性，这些属性在配置文件或标识微服务的用户实体中定义，其中包括标识。 但是订购微服务中的买家实体可能具有较少的属性，因为只有某些买家数据与订单流程相关。 每个微服务的上下文或每个绑定上下文都会影响其域模型。

除了实现数据属性外，域实体还必须实现行为。

DDD 中的域实体必须实现与实体数据（在内存中访问的对象）相关的域逻辑或行为。 例如，作为订单实体类的一部分，你必须将业务逻辑和操作作为任务（例如添加订单项、数据验证和总计算）的方法实现。 实体的方法负责处理实体的不变量和规则，而不是将这些规则分布在应用层中。

图 7-8 展示了一个域实体，它不仅实现数据属性，还实现具有相关域逻辑的操作或方法。

![显示域实体模式的关系图。](./media/microservice-domain-model/domain-entity-pattern.png)

**图 7-8**。 实现数据加行为的域实体设计示例

域模型实体通过方法实现行为，即，它不是“贫乏”模型。 当然，实体有时可能不会在实体类中实现任何逻辑。 如果某个聚合内的子实体没有任何特殊逻辑，因为大多数逻辑都在聚合根中定义，则该子实体可能出现这种情况。 如果你有一个复杂的微服务，它在服务类而非域实体中实现了逻辑，那么你可能会陷入贫乏域模型中，下一节将对此进行解释。

### <a name="rich-domain-model-versus-anemic-domain-model"></a>丰富域模型与贫乏域模型

Martin Fowler 在他的博客文章 [AnemicDomainModel](https://martinfowler.com/bliki/AnemicDomainModel.html) 中是这样描述贫乏域模型的：

贫乏域模型的基本症状是，乍一看上去像是真实存在的。 它包含一些对象，许多以域空间中的名词命名，这些对象与真实域模型具有的丰富关系和结构相关联。 但当你观察它的行为时，问题来了，你发现这些对象几乎没有任何行为，完全就是一些 getter 和 setter 而已。

当然，使用贫乏域模型时，将从一组可捕获所有域或业务逻辑的服务对象（传统上称为*业务层*）中使用这些数据模型。 业务层位于数据模型之上，就像使用数据一样使用数据模型。

贫乏域模型就是一种程序化样式设计。 贫乏实体对象不是真实的对象，因为它们缺乏行为（方法）。 它们只保存数据属性，因此它不是一种面向对象的设计。 通过将所有行为放到服务对象（业务层）中，实质上最终会产生[面条式代码](https://en.wikipedia.org/wiki/Spaghetti_code)或[事务脚本](https://martinfowler.com/eaaCatalog/transactionScript.html)，因而失去域模型提供的优势。

不管怎样，如果微服务或绑定上下文非常简单（CRUD 服务），仅包含数据属性的实体对象形式的贫乏域模型可能已经足够，没必要实现更复杂的 DDD 模式。 在这种情况下，它就是一个持久性模型，因为你特意创建了一个仅包含用于 CRUD 的数据的实体。

这就是为什么微服务体系结构特别适用于多体系结构方法（具体取决于每个绑定上下文）。 例如，在 eShopOnContainers 中，订购微服务实现了 DDD 模式，但目录微服务（一种简单的 CRUD 服务）没有。

有人说贫乏域模型是一种反模式。 这真的取决于你要实现什么。 如果你创建的微服务足够简单（例如，CRUD 服务），则采用贫乏域模型，它不是反模式。 但是，如果需要解决包含大量不断变化的业务规则的微服务域的复杂性，那么贫乏域模型可能是该微服务或绑定上下文的反模式。 在这种情况下，将其设计为具有包含数据加行为的实体的丰富模型并实现附加 DDD 模式（聚合、值对象等）可能对这种微服务的长期成功具有极大的好处。

#### <a name="additional-resources"></a>其他资源

- **DevIQ.域实体** \
  <https://deviq.com/entity/>

- **Martin Fowler。域模型** \
  <https://martinfowler.com/eaaCatalog/domainModel.html>

- **Martin Fowler。贫乏域模型** \
  <https://martinfowler.com/bliki/AnemicDomainModel.html>

### <a name="the-value-object-pattern"></a>值对象模式

正如 Eric Evans 所指出的，“许多对象没有概念标识。 这些对象用于描述某个事物的某些特征。”

实体需要标识，但系统中的许多对象不需要，例如值对象模式。 值对象是一种没有概念标识的对象，用于描述域方面。 这些对象实例化后可表示设计元素，你只会暂时关注它们。 你只关心它们是*什么*，而不关心它们是*谁*。 其示例包括数字和字符串，但也可以是更高级别的概念，例如属性组。

一个微服务中的实体在另一个微服务中可能就不是实体，因为在第二种情况下，绑定上下文可能具有不同的含义。 例如，电子商务应用程序中的地址可能根本没有标识，因为它可能仅表示个人或公司的客户资料的一组属性。 在这种情况下，地址应归类为值对象。 但是，在电力公司的应用程序中，客户地址对于业务领域可能很重要。 因此，该地址必须具有标识，以便计费系统直接链接到该地址。 在这种情况下，地址应归类为域实体。

有名字和姓氏的人通常是一个实体，因为这个人具有标识，即使其名字和姓氏与另一组值重合（例如这些名字还指另一个人）也是如此。

值对象在关系数据库和 ORM（如 Entity Framework (EF)）中很难管理，而在面向文档的数据库中，它们更易于实现和使用。

EF Core 2.0 及更高版本包含[从属实体](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-0/#owned-entities-and-table-splitting)功能，这样更易于处理值对象，稍后将会详细介绍。

#### <a name="additional-resources"></a>其他资源

- **Martin Fowler。值对象模式** \
  <https://martinfowler.com/bliki/ValueObject.html>

- 值对象 \
  <https://deviq.com/value-object/>

- 测试驱动开发中的值对象 \
  [https://leanpub.com/tdd-ebook/read\#leanpub-auto-value-objects](https://leanpub.com/tdd-ebook/read#leanpub-auto-value-objects)

- **Eric Evans。域驱动设计：软件核心复杂性应对之道.** （书；包括值对象的讨论）\
  <https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/>

### <a name="the-aggregate-pattern"></a>聚合模式

域模型包含不同数据实体和进程的群集，可以控制功能的重要方面，例如订单履行或库存。 聚合是一种粒度更细的 DDD 单元，用于描述一群或一组可视为内聚单元的实体和行为。

通常基于所需事务来定义聚合。 一个典型的例子就是订单，订单中还包含订单项列表。 订单项通常是一个实体。 但它是订单聚合内的子实体，该聚合还包含订单实体作为其根实体，通常称为聚合根。

识别聚合可能很难。 聚合是一组必须一致的对象，但不能按此挑选一组对象就将它们标记为聚合。 你必须从域概念开始，并考虑在与该概念相关的最常见事务中使用的实体。 那些需要在事务上一致的实体就形成一个聚合。 考虑事务操作可能是识别聚合的最佳方式。

### <a name="the-aggregate-root-or-root-entity-pattern"></a>聚合根或根实体模式

聚合由至少一个实体组成：聚合根，也称为根实体或主实体。 此外，它还可以有多个子实体和值对象，所有实体和对象一起共同实现所需的行为和事务。

聚合根的目的是确保聚合的一致性；它应该是通过聚合根类中的方法或操作更新聚合的唯一入口点。 只能通过聚合根对聚合内的实体进行更改。 它是聚合的一致性守护者，它会考虑到可能需要在聚合中遵守的所有不变量和一致性规则。 如果单独更改某个子实体或值对象，聚合根无法确保聚合处于有效状态。 这就像一张桌脚松动了的桌子。 保持一致性是聚合根的主要目的。

在图 7-9 中，可以看到一些示例聚合，例如买家聚合，其中包含一个实体（聚合根 Buyer）。 订单聚合包含多个实体和一个值对象。

![比较买家聚合和订单聚合的关系图。](./media/microservice-domain-model/buyer-order-aggregate-pattern.png)

**图 7-9**。 包含多个或单个实体的聚合示例

DDD 域模型由聚合组成，聚合可以只具有一个实体或多个实体，并且还可以包含值对象。 请注意，视你的域而定，Buyer 聚合可能会有其他子实体，就像在 eShopOnContainers 参考应用程序的订购微服务中那样。 图 7-9 仅列举了买家具有单个实体的情况，作为仅包含聚合根的聚合示例。

为了让聚合一直相互隔离并保持它们之间的清晰界限，建议禁止在 DDD 域模型中的聚合之间直接导航，并且模型仅具有外键 (FK) 字段，正如在 eShopOnContainers 的[订购微服务域模型](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.Domain/AggregatesModel/OrderAggregate/Order.cs)中所实现的那样。 Order 实体针对买家只有外键字段，没有 EF Core 导航属性，如以下代码所示：

```csharp
public class Order : Entity, IAggregateRoot
{
    private DateTime _orderDate;
    public Address Address { get; private set; }
    private int? _buyerId; // FK pointing to a different aggregate root
    public OrderStatus OrderStatus { get; private set; }
    private readonly List<OrderItem> _orderItems;
    public IReadOnlyCollection<OrderItem> OrderItems => _orderItems;
    // ... Additional code
}
```

识别和使用聚合需要经过大量探索与体会。 有关详细信息，请参阅下面的“其他资源”列表。

#### <a name="additional-resources"></a>其他资源

- **Vaughn Vernon。有效的聚合设计 - 第 1 部分：单个聚合建模**（来自 <https://dddcommunity.org/>） \
  <https://dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_1.pdf>

- **Vaughn Vernon。有效的聚合设计 - 第 2 部分：让聚合共同工作**（来自 <https://dddcommunity.org/>） \
  <https://dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_2.pdf>

- **Vaughn Vernon。有效的聚合设计 - 第 3 部分：通过发现获取见解**（来自 <https://dddcommunity.org/>） \
  <https://dddcommunity.org/wp-content/uploads/files/pdf_articles/Vernon_2011_3.pdf>

- **Sergey Grybniak。DDD 战术设计模式** \
  <https://www.codeproject.com/Articles/1164363/Domain-Driven-Design-Tactical-Design-Patterns-Part>

- **Chris Richardson.使用聚合开发事务微服务** \
  <https://www.infoq.com/articles/microservices-aggregates-events-cqrs-part-1-richardson>

- **DevIQ.聚合模式** \
  <https://deviq.com/aggregate-pattern/>

>[!div class="step-by-step"]
>[上一页](ddd-oriented-microservice.md)
>[下一页](net-core-microservice-domain-model.md)
