---
title: 使用 .NET Core 实现微服务域模型
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 获取面向 DDD 的域模型的实现详细信息。
ms.date: 10/08/2018
ms.openlocfilehash: e24f4e643d258450a2b33ed4dc4aded718bebd82
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91152541"
---
# <a name="implement-a-microservice-domain-model-with-net-core"></a>使用 .NET Core 实现微服务域模型

上一节解释了域模型设计的基本设计原则和模式。 现在开始探索使用 .NET Core（纯 C\# 代码）和 EF Core 实现域模型的可能方式。 域模型将仅由代码组成。 它只有 EF Core 模型要求，并不真正依赖于 EF。 你不应该硬依赖或引用 EF Core 或域模型中的任何其他 ORM。

## <a name="domain-model-structure-in-a-custom-net-standard-library"></a>自定义 .NET Standard 库中的域模型结构

用于 eShopOnContainers 参考应用程序的文件夹组织演示了该应用程序的 DDD 模型。 你可能会发现，不同的文件夹组织能更清楚地传达为应用程序所做的设计选择。 如图 7-10 所示，订购域模型包含两个聚合，即订单聚合和买方聚合。 每个聚合都是一组域实体和值对象，但聚合也可以由单个域实体（聚合根或根实体）组成。

:::image type="complex" source="./media/net-core-microservice-domain-model/ordering-microservice-container.png" alt-text="解决方案资源管理器中的 Ordering.Domain 项目的屏幕截图。":::
Ordering.Domain 项目的解决方案资源管理器视图，其中显示包含 BuyerAggregate 和 OrderAggregate 文件夹的 AggregatesModel 文件夹，这两个文件夹各自包含其实体类、值对象文件等。
:::image-end:::

**图 7-10**。 eShopOnContainers 中订购微服务的域模型结构

此外，域模型层还包含存储库协定（接口）作为域模型的基础结构要求。 换而言之，这些接口表示基础结构层必须实现的存储库和方法。 务必将存储库实现放在域模型层之外、基础结构层库之中，这样，域模型层就不会受到基础结构技术（例如 Entity Framework）中 API 或类的“污染”。

你还可以看到 一个 [SeedWork](https://martinfowler.com/bliki/Seedwork.html) 文件夹，其中包含可用作域实体和值对象基础的自定义基类，因此每个域的对象类中都没有冗余代码。

## <a name="structure-aggregates-in-a-custom-net-standard-library"></a>在自定义 .NET Standard 库中构造聚合

聚合是指组合到一起以匹配事务一致性的域对象群集。 这些对象可能是实体实例（其中一个是聚合根或根实体）加上任何附加值对象。

事务一致性意味着，保证聚合在业务操作结束时保持一致且处于最新状态。 例如，eShopOnContainers 订购微服务域模型中订单聚合的组成如图 7-11 所示。

:::image type="complex" source="./media/net-core-microservice-domain-model/vs-solution-explorer-order-aggregate.png" alt-text="OrderAggregate 文件夹及其类的屏幕截图。":::
OrderAggregate 文件夹的详细视图：Address.cs 是值对象，IOrderRepository 是存储库接口，Order.cs 是聚合根，OrderItem.cs 是子实体，而 OrderStatus.cs 是枚举类。
:::image-end:::

**图 7-11**。 Visual Studio 解决方案中的订单聚合

如果打开聚合文件夹中的任意文件，可以看到它是如何被标记为自定义基类或接口的（像实体或值对象一样），正如在 [SeedWork](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Services/Ordering/Ordering.Domain/SeedWork) 文件夹中所实现的一样。

## <a name="implement-domain-entities-as-poco-classes"></a>将域实体作为 POCO 类实现

通过创建实现域实体的 POCO 类，可在 .NET 中实现域模型。 在下面的示例中，Order 类同时被定义为实体和聚合根。 Order 类派生自 Entity 基类，因此它可以重用与实体相关的通用代码。 请记住，这些基类和接口由你在域模型项目中定义，因此它是你的代码，而不是 ORM（例如 EF）中的基础结构代码。

```csharp
// COMPATIBLE WITH ENTITY FRAMEWORK CORE 2.0
// Entity is a custom base class with the ID
public class Order : Entity, IAggregateRoot
{
    private DateTime _orderDate;
    public Address Address { get; private set; }
    private int? _buyerId;

    public OrderStatus OrderStatus { get; private set; }
    private int _orderStatusId;

    private string _description;
    private int? _paymentMethodId;

    private readonly List<OrderItem> _orderItems;
    public IReadOnlyCollection<OrderItem> OrderItems => _orderItems;

    public Order(string userId, Address address, int cardTypeId, string cardNumber, string cardSecurityNumber,
            string cardHolderName, DateTime cardExpiration, int? buyerId = null, int? paymentMethodId = null)
    {
        _orderItems = new List<OrderItem>();
        _buyerId = buyerId;
        _paymentMethodId = paymentMethodId;
        _orderStatusId = OrderStatus.Submitted.Id;
        _orderDate = DateTime.UtcNow;
        Address = address;

        // ...Additional code ...
    }

    public void AddOrderItem(int productId, string productName,
                            decimal unitPrice, decimal discount,
                            string pictureUrl, int units = 1)
    {
        //...
        // Domain rules/logic for adding the OrderItem to the order
        // ...

        var orderItem = new OrderItem(productId, productName, unitPrice, discount, pictureUrl, units);

        _orderItems.Add(orderItem);

    }
    // ...
    // Additional methods with domain rules/logic related to the Order aggregate
    // ...
}
```

值得注意的是，这是一个作为 POCO 类实现的域实体。 它不直接依赖于 Entity Framework Core 或任何其他基础结构框架。 DDD 中采用的就是这种实现方式，即完全通过 C# 代码来实现域模型。

此外，该类用名为 IAggregateRoot 的接口修饰。 该接口是一个空接口，有时称为*标记接口*，仅用于指示此实体类也是聚合根。

标记接口有时被认为是一种反模式；然而，它也是一种对类进行标记的干净方式，尤其是当该接口可能正在不断演变时。 属性可以是标记的另一种选择，但是，将基类 (Entity) 放在 IAggregate 接口旁边比在该类上方放置一个 Aggregate 属性标记更为显眼。 在任何情况下，这都只是一个偏好问题。

具有聚合根意味着与聚合实体的一致性和业务规则相关的大部分代码应该作为 Order 聚合根类中的方法实现（例如，向聚合添加 OrderItem 对象时的 AddOrderItem）。 不能单独或直接创建或更新 OrderItems 对象；AggregateRoot 类必须保持对其子实体执行的任何更新操作的控制和一致性。

## <a name="encapsulate-data-in-the-domain-entities"></a>封装域实体中的数据

实体模型中的一个常见问题是，它们将集合导航属性公开为可公开访问的列表类型。 这使得任何协作方开发人员都能操作这些集合类型的内容，这可能会绕过与集合相关的重要业务规则，从而可能使对象处于无效状态。 若要解决该问题，可向相关集合公开只读访问权限，并显式提供一些方法来定义客户端操作这些集合的方式。

请注意，在前面的代码中，许多属性是只读或私有属性，只能由类方法进行更新，因此任何更新都应考虑在类方法中指定的业务领域不变量和逻辑。

例如，使用 DDD 模式时，不能从任何命令处理程序方法或应用层类执行以下命令（实际上，你应该无法这样做）：

```csharp
// WRONG ACCORDING TO DDD PATTERNS – CODE AT THE APPLICATION LAYER OR
// COMMAND HANDLERS
// Code in command handler methods or Web API controllers
//... (WRONG) Some code with business logic out of the domain classes ...
OrderItem myNewOrderItem = new OrderItem(orderId, productId, productName,
    pictureUrl, unitPrice, discount, units);

//... (WRONG) Accessing the OrderItems collection directly from the application layer // or command handlers
myOrder.OrderItems.Add(myNewOrderItem);
//...
```

在此示例中，Add 方法就是一种数据添加操作，可以直接访问 OrderItems 集合。 因此，与针对子实体执行的该操作相关的大部分域逻辑、规则或验证将分布于应用层（命令处理程序和 Web API 控制器）中。

如果绕过聚合根，聚合根无法保证其不变量、有效性或一致性。 最终将产生面条式代码或事务脚本代码。

若要遵循 DDD 模式，实体不能在任何实体属性中拥有公共 setter。 实体中的更改应由显式方法驱动，这些方法使用显式通用语言来描述它们正在实体中执行的更改。

此外，实体中的集合（如订单项）应为只读属性（稍后会解释 AsReadOnly 方法）。 只能从聚合根类方法或子实体方法中更新它。

从 Order 聚合根的代码中可以看到，所有 setter 都应该是私有的，或者至少是从外部只读的，因此针对实体数据或其子实体的任何操作都必须通过实体类中的方法来执行。 这将以一种面向对象的可控方式（而不是通过实现事务脚本代码）保持一致性。

下面的代码片段展示了对将 OrderItem 对象添加到 Order 聚合的任务进行编码的正确方式。

```csharp
// RIGHT ACCORDING TO DDD--CODE AT THE APPLICATION LAYER OR COMMAND HANDLERS
// The code in command handlers or WebAPI controllers, related only to application stuff
// There is NO code here related to OrderItem object's business logic
myOrder.AddOrderItem(productId, productName, pictureUrl, unitPrice, discount, units);

// The code related to OrderItem params validations or domain rules should
// be WITHIN the AddOrderItem method.

//...
```

在此片段中，与 OrderItem 对象创建相关的大部分验证或逻辑都由 Order 聚合根在 AddOrderItem 方法中控制 — 特别是与聚合中的其他元素相关的验证和逻辑。 例如，你可能在多次调用 AddOrderItem 后得到相同的产品项。 在该方法中，你可以检查产品项并将相同的产品项合并到具有多个单元的单个 OrderItem 对象中。 此外，如果折扣金额不同但产品 ID 相同，则可能会应用较高的折扣。 此原则适用于 OrderItem 对象的任何其他域逻辑。

此外，新的 OrderItem(params) 操作也由 Order 聚合根中的 AddOrderItem 方法控制和执行。 因此，与该操作相关的大部分逻辑或验证（尤其是影响其他子实体间一致性的逻辑或验证）将位于聚合根的单个位置中。 这是聚合根模式的最终目的。

使用 Entity Framework Core 1.1 或更高版本时，可以更好地表示 DDD 实体，因为它允许[映射到字段](/ef/core/modeling/backing-field)以及属性。 这在保护子实体或值对象集合时很有用。 借助此增强功能，你可以使用简单的私有字段，而不必使用属性，并且可以在公共方法中实现对字段集合的任何更新，并通过 AsReadOnly 方法提供只读访问。

在 DDD 中，你想通过实体（或构造函数）中的方法只更新实体，以便控制任何不变量和数据一致性，因此，属性定义为仅具有 get 取值函数。 这些属性受私有字段支持。 只能从类中访问私有成员。 但是，有一种例外情况：EF Core 也需要设置这些字段（这样它就可以返回具有适当值的对象）。

### <a name="map-properties-with-only-get-accessors-to-the-fields-in-the-database-table"></a>将仅具有 get 取值函数的属性映射到数据库表中的字段

将属性映射到数据库表列不是域的责任，而是基础结构和持久性层的一部分。 之所以提到这一点，是为了让你留意 EF Core 1.1 或更高版本中有关如何为实体建模的新功能。 有关此主题的其他详细信息，请参阅基础结构和持久性部分。

使用 EF Core 1.0 或更高版本时，需要在 DbContext 中将定义为仅具有 getter 的属性映射到数据库表中的实际字段。 此操作通过 PropertyBuilder 类的 HasField 方法完成。

### <a name="map-fields-without-properties"></a>映射不含属性的字段

通过借助 EF Core 1.1 或更高版本中的功能将列映射到字段，也可以不使用属性。 你可以只将表中的列映射到字段。 它的常见用例是那些无需从实体外访问的内部状态的私有字段。

例如，前面的 OrderAggregate 代码示例中有几个私有字段，比如 `_paymentMethodId` 字段，对于 setter 或 getter，它们都没有相关属性。 该字段也可以在订单的业务逻辑内进行计算并从订单的方法中使用，但它也需要保存在数据库中。 因此，EF Core（从 v1.1 开始）提供了一种将没有相关属性的字段映射到数据库列的方法。 本指南的[基础结构层](ddd-oriented-microservice.md#the-infrastructure-layer)部分也对此进行了说明。

### <a name="additional-resources"></a>其他资源

- **Vaughn Vernon。使用 DDD 和 Entity Framework 对聚合建模**。 请注意，*不*是 Entity Framework Core。 \
  <https://kalele.io/blog-posts/modeling-aggregates-with-ddd-and-entity-framework/>

- **Julie Lerman.数据点 - 域驱动设计的编码：面向数据聚焦型开发人员的提示** \
  <https://docs.microsoft.com/archive/msdn-magazine/2013/august/data-points-coding-for-domain-driven-design-tips-for-data-focused-devs>

- **Udi Dahan.如何创建完全封装的域模型** \
  <https://udidahan.com/2008/02/29/how-to-create-fully-encapsulated-domain-models/>

> [!div class="step-by-step"]
> [上一页](microservice-domain-model.md)
> [下一页](seedwork-domain-model-base-classes-interfaces.md)
