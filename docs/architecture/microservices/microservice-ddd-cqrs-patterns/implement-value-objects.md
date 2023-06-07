---
title: 实现值对象
description: 适用于容器化的 .NET 应用程序的 .NET 微服务体系结构 | 深入了解有关使用新实体框架功能实现值对象的详细信息和选项。
ms.date: 08/21/2020
ms.openlocfilehash: 1cb7ce04b3ab2f6da25f398e016baf60b863fb6b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91169201"
---
# <a name="implement-value-objects"></a>实现值对象

如前面部分有关实体和聚合的讨论，标识对于实体是必不可少的。 但是，系统中有许多对象和数据项不需要标识和标识跟踪，例如值对象。

值对象可以引用其他实体。 例如，在生成描述如何从一个点转到另一点的路由时，该路由应为值对象。 它将是特定路由上的点的快照，但此建议的路由将不具有标识，即使在内部它可能指 City、Road 等实体。

图 7-13 显示 Order 聚合中的 Address 值对象。

![显示订单聚合中的 Address 值对象的关系图。](./media/implement-value-objects/value-object-within-aggregate.png)

图 7-13  。 Order 聚合中的 Address 值对象

如图 7-13 所示，实体通常由多个属性组成。 例如，`Order` 实体可以建模为具有标识的实体，在内部由一组特性组成，例如 OrderId、OrderDate、OrderItems 等。但地址这个作为由国家/地区或区域、街道、城市等组成的复杂值，必须建模或处理为值对象。

## <a name="important-characteristics-of-value-objects"></a>值对象的重要特征

值对象有两个主要特征：

- 它们没有任何标识。

- 它们是不可变的。

第一个特征上面讨论了。 不可变性是一个重要要求。 创建对象后，值对象的值必须是不可变的。 因此，当构造对象时，必须提供所需的值，但不得允许它们在对象生存期内进行更改。

因其不可变性，值对象允许采取某些技巧来提升性能。 当系统有成千上万个值对象实例且其中很多具有相同值时，这特别有用。 不可变本质使它们可以重用；它们可以是可互换对象，因为值都相同，且没有任何标识。 有时，这种优化可以在运行缓慢的软件和性能良好的软件之间制造差异。 当然，所有这些将取决于应用程序环境和部署上下文。

## <a name="value-object-implementation-in-c"></a>C\# 中的值对象实现

就实现而言，你可以拥有值对象基类，它具有基于所有特性（因为值对象不得基于标识）和其他基本特征间的对比的基本实用工具方法，如相等。 下面的示例演示用于 eShopOnContainers 订购微服务中的值对象基类。

```csharp
public abstract class ValueObject
{
    protected static bool EqualOperator(ValueObject left, ValueObject right)
    {
        if (ReferenceEquals(left, null) ^ ReferenceEquals(right, null))
        {
            return false;
        }
        return ReferenceEquals(left, null) || left.Equals(right);
    }

    protected static bool NotEqualOperator(ValueObject left, ValueObject right)
    {
        return !(EqualOperator(left, right));
    }

    protected abstract IEnumerable<object> GetEqualityComponents();

    public override bool Equals(object obj)
    {
        if (obj == null || obj.GetType() != GetType())
        {
            return false;
        }

        var other = (ValueObject)obj;

        return this.GetEqualityComponents().SequenceEqual(other.GetEqualityComponents());
    }

    public override int GetHashCode()
    {
        return GetEqualityComponents()
            .Select(x => x != null ? x.GetHashCode() : 0)
            .Aggregate((x, y) => x ^ y);
    }
    // Other utility methods
}
```

实现实际值对象时可以使用此类，如下面实例中显示的 Address 值对象：

```csharp
public class Address : ValueObject
{
    public String Street { get; private set; }
    public String City { get; private set; }
    public String State { get; private set; }
    public String Country { get; private set; }
    public String ZipCode { get; private set; }

    public Address() { }

    public Address(string street, string city, string state, string country, string zipcode)
    {
        Street = street;
        City = city;
        State = state;
        Country = country;
        ZipCode = zipcode;
    }

    protected override IEnumerable<object> GetEqualityComponents()
    {
        // Using a yield return statement to return each element one at a time
        yield return Street;
        yield return City;
        yield return State;
        yield return Country;
        yield return ZipCode;
    }
}
```

可以看到此 Address 的值对象实现没有标识，因此在 Address 类中甚至在 ValueObject 类中都没有 ID 字段。

在 EF Core 2.0 之前，实体框架 (EF) 使用的类中是不能没有 ID 字段的，EF Core 2.0 在实现不具有 ID 的更好值对象方面发挥了很大的作用。 下一节内容将对此进行详细介绍。

也许有人会争辩说，由于值对象是不可变的，所以应该是只读的（即具有“只获取”属性），这是事实没错。 但是，值对象通常会被执行序列化和反序列化操作以遍历消息队列，并且由于是只读的，这阻止了反序列化器分配值，因此只需将其保留为 `private set`，且其只读程度让此机制成为可能。

## <a name="how-to-persist-value-objects-in-the-database-with-ef-core-20-and-later"></a>如何通过 EF Core 2.0 及更高版本在数据库中持久保存值对象

你刚才看到如何在域模型中定义值对象。 但是，由于 它通常使用标识来确定实体，如何切实地使用 Entity Framework Core 来将其持久保存在数据库中呢？

### <a name="background-and-older-approaches-using-ef-core-11"></a>背景和较早的方法（使用 EF Core 1.1）

背景：使用 EF Core 1.0 和 1.1 版时的限制是不能使用传统 .NET Framework 的 EF 6.x 中所定义的[复杂类型](xref:System.ComponentModel.DataAnnotations.Schema.ComplexTypeAttribute)。 因此，如果使用 EF Core 1.0 或 1.1，则需要将值对象存储为具有 ID 字段的 EF 实体。 然后，为使它看起来像没有任何标识的值对象，你可以隐藏其 ID：表明值对象的标识在域模型中不重要。 可以通过将该 ID 用作[阴影属性](/ef/core/modeling/shadow-properties)来隐藏其 ID。 由于在模型中隐藏 ID 的配置是在 EF 基础结构级别中设置的，ID 对于域模型好似透明的。

在 eShopOnContainers (.NET Core 1.1) 的初始版本中，EF Core 基础结构所需的隐藏 ID 在 DbContext 级别上采用基础结构项目中的 Fluent API 通过以下方式来实现。 因此，该 ID 从域模型角度看是隐藏的，但仍存于基础结构中。

```csharp
// Old approach with EF Core 1.1
// Fluent API within the OrderingContext:DbContext in the Infrastructure project
void ConfigureAddress(EntityTypeBuilder<Address> addressConfiguration)
{
    addressConfiguration.ToTable("address", DEFAULT_SCHEMA);

    addressConfiguration.Property<int>("Id")  // Id is a shadow property
        .IsRequired();
    addressConfiguration.HasKey("Id");   // Id is a shadow property
}
```

但是，将值对象持久保存在数据库中执行起来如同将常规实体持久保存在不同表中。

使用 EF Core 2.0 及更高版本时，有更好的新方法来持久保存值对象。

## <a name="persist-value-objects-as-owned-entity-types-in-ef-core-20-and-later"></a>在 EF Core 2.0 及更高版本中以从属实体类型形式来持久保存值对象

即使 EF Core 中的固有实体类型与 DDD 中的规范值对象模式之间存在一些差距，当前它是在 EF Core 2.0 及更高版本中持久保存值对象的最佳方式。 将在本部分末尾看到限制。

固有实体类型功能已添加到 EF Core 2.0 及以上版本。

固有实体类型允许在任何实体内映射具有以下特征的类型：用作属性且不具有在域模型中显式定义的自己的标识，如值对象。 从属实体类型与其他实体类型共享相同的 CLR 类型（也就是说，它是常规类）。 包含定义性导航的实体是所有者实体。 查询所有者时，固有类型将默认包含在内。

查看一下域模型，从属类型看上去似乎没有任何标识。 但是事实上，固有类型的确有标识，但所有者导航属性为此标识的一部分。

拥有类型的实例的标识并非完全属于他们自己。 它由三个部分组成：

- 所有者标识

- 指向它们的导航属性

- 对于固有类型的集合，一个独立的组成部分（在 EF Core 2.2 及更高版本中受支持）。

例如，在 eShopOnContainers 的订购域模型中，作为 Order 实体的一部分，Address 值对象在所有者实体（即 Order 实体）内作为固有实体类型实现。 `Address` 类型在域模型中没有定义标识属性。 它用作 Order 类型的属性来指定特定订单的发货地址。

依照约定，将为固有类型创建一个阴影主键，并通过表拆分将其映射到与所有者相同的表。 这样就可以通过类似于传统 .NET Framework 的 EF6 中复杂类型的用法来使用固有类型。

请务必注意，固有类型在 EF Core 中永远不会由约定发现，因此你必须显式声明它们。

在 eShopOnContainers 中，在 OrderingContext.cs 文件中的 `OnModelCreating()` 方法中，应用了多个基础结构配置。 其中之一与 Order 实体相关。

```csharp
// Part of the OrderingContext.cs class at the Ordering.Infrastructure project
//
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.ApplyConfiguration(new ClientRequestEntityTypeConfiguration());
    modelBuilder.ApplyConfiguration(new PaymentMethodEntityTypeConfiguration());
    modelBuilder.ApplyConfiguration(new OrderEntityTypeConfiguration());
    modelBuilder.ApplyConfiguration(new OrderItemEntityTypeConfiguration());
    //...Additional type configurations
}
```

在下面的代码中，针对 Order 实体定义了持久性基础结构：

```csharp
// Part of the OrderEntityTypeConfiguration.cs class
//
public void Configure(EntityTypeBuilder<Order> orderConfiguration)
{
    orderConfiguration.ToTable("orders", OrderingContext.DEFAULT_SCHEMA);
    orderConfiguration.HasKey(o => o.Id);
    orderConfiguration.Ignore(b => b.DomainEvents);
    orderConfiguration.Property(o => o.Id)
        .ForSqlServerUseSequenceHiLo("orderseq", OrderingContext.DEFAULT_SCHEMA);

    //Address value object persisted as owned entity in EF Core 2.0
    orderConfiguration.OwnsOne(o => o.Address);

    orderConfiguration.Property<DateTime>("OrderDate").IsRequired();

    //...Additional validations, constraints and code...
    //...
}
```

在前面的代码中，`orderConfiguration.OwnsOne(o => o.Address)` 方法指定了 `Address` 属性是 `Order` 类型的固有实体。

默认情况下，EF Core 约定将固有实体类型属性的数据库列命名为 `EntityProperty_OwnedEntityProperty`。 因此，`Address` 的内部属性将以 `Address_Street`、`Address_City` 这样的名称（对于 `State`、`Country` 和 `ZipCode` 可进行类推）出现在 `Orders` 表中。

可以附加 `Property().HasColumnName()` 连贯性方法来重命名这些列。 如果 `Address` 是公共属性，映射将如下所示：

```csharp
orderConfiguration.OwnsOne(p => p.Address)
                            .Property(p=>p.Street).HasColumnName("ShippingStreet");

orderConfiguration.OwnsOne(p => p.Address)
                            .Property(p=>p.City).HasColumnName("ShippingCity");
```

可以将 `OwnsOne` 方法链接到连贯性映射。 在以下假设示例中，`OrderDetails` 拥有 `BillingAddress` 和 `ShippingAddress`，它们均为 `Address` 类型。 然后 `OrderDetails` 归 `Order` 类型所有。

```csharp
orderConfiguration.OwnsOne(p => p.OrderDetails, cb =>
    {
        cb.OwnsOne(c => c.BillingAddress);
        cb.OwnsOne(c => c.ShippingAddress);
    });
//...
//...
public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
}

public class OrderDetails
{
    public Address BillingAddress { get; set; }
    public Address ShippingAddress { get; set; }
}

public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

### <a name="additional-details-on-owned-entity-types"></a>固有实体类型上的其他详细信息

- 使用 OwnsOne Fluent API 将导航属性配置为特定类型时即定义固有类型。

- 我们元数据模型中固有类型的定义为以下各项的组合：所有者类型、导航属性，以及固有类型的 CLR 类型。

- 我们堆栈中固有类型实例的标识（键）即为所有者类型标识和固有类型定义的组合。

#### <a name="owned-entities-capabilities"></a>固有实体功能

- 固有类型可以引用其他实体，固有（嵌套固有类型）或非固有（其他实体的常规引用导航属性）均可。

- 可以通过单独的导航属性在同一所有者实体中以不同固有类型的形式来映射相同的 CLR 类型。

- 表拆分由约定设置，但可以通过使用 ToTable 将固有类型映射到其他表来另行选择。

- 立即加载对于固有类型自动执行，即无需对查询调用 `.Include()`。

- 使用 EF Core 2.1 及更高版本可以通过属性 `[Owned]` 进行配置。

- 可以处理固有类型的集合（使用版本 2.2 及更高版本）。

#### <a name="owned-entities-limitations"></a>固有实体限制

- 不能创建固有类型的 `DbSet<T>`（按照设计）。

- 不能对固有类型调用 `ModelBuilder.Entity<T>()`（目前按照设计）。

- 不支持使用同一表格中所有者映射的可选（即为 null）固有类型（即使用表格拆分）。 这是因为对每个属性都进行了映射，因此总体而言，对于 null 复杂值，没有单独的 sentinel。

- 没有对固有类型的继承映射支持，但应能够以不同固有类型的形式映射同一继承层次结构的两个叶类型。 EF Core 不会就它们不属于同一层次结构的事实进行推断。

#### <a name="main-differences-with-ef6s-complex-types"></a>与 EF6 的复杂类型的主要差异

- 表拆分是可选的，即可以根据需要将它们映射到单独的表并仍属固有类型。

- 它们可以引用其他实体（即它们可以充当关系上其他非固有类型的依赖端）。

## <a name="additional-resources"></a>其他资源

- **Martin Fowler。ValueObject 模式** \
  <https://martinfowler.com/bliki/ValueObject.html>

- **Eric Evans。域驱动设计：软件核心复杂性应对之道.** （书；包括值对象的讨论）\
  <https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/>

- **Vaughn Vernon。实现域驱动设计。** （书；包括值对象的讨论）\
  <https://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577/>

- **从属实体类型** \
  <https://docs.microsoft.com/ef/core/modeling/owned-entities>

- 阴影属性   \
  <https://docs.microsoft.com/ef/core/modeling/shadow-properties>

- **复杂类型和/或值对象**。 EF Core GitHub 存储库中的讨论（“问题”选项卡）\
  <https://github.com/dotnet/efcore/issues/246>

- **ValueObject.cs.** eShopOnContainers 中的基值对象类。 \
  <https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.Domain/SeedWork/ValueObject.cs>

- **地址类。** eShopOnContainers 中的示例值对象类。 \
  <https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.Domain/AggregatesModel/OrderAggregate/Address.cs>

> [!div class="step-by-step"]
> [上一页](seedwork-domain-model-base-classes-interfaces.md)
> [下一页](enumeration-classes-over-enum-types.md)
