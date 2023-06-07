---
title: 使用 Entity Framework Core 实现基础结构持久性层
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 探索使用 Entity Framework Core 实现基础结构持久性层的细节。
ms.date: 01/30/2020
ms.openlocfilehash: 878d4d64e92ca92fd2393d60d496f1c5671e7029
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172347"
---
# <a name="implement-the-infrastructure-persistence-layer-with-entity-framework-core"></a>使用 Entity Framework Core 实现基础结构持久性层

当使用 SQL Server、Oracle 或 PostgreSQL 等关系数据库时，推荐的方法是基于 Entity Framework (EF) 实现持久性层。 EF 支持 LINQ，并为模型提供强类型化的对象，且为数据库提供简化的持久性。

Entity Framework 很长一段时期作为 .NET Framework 的一部分。 使用 .NET Core 时，还应使用 Entity Framework Core，它在 Windows 或 Linux 上的运行方式与在 .NET Core 上的相同。 EF Core 完全重写了 Entity Framework，采用更小的占用空间来实现，且性能上具有重大改进。

## <a name="introduction-to-entity-framework-core"></a>Entity Framework Core 简介

Entity Framework (EF) Core 是轻量化、可扩展和跨平台版的常用 Entity Framework 数据访问技术。 它随 .NET Core 于 2016 年年中引入。

EF 简介已提供于 Microsoft 文档中，因此这里我们只提供指向该信息的链接。

### <a name="additional-resources"></a>其他资源

- **Entity Framework Core** \
  [https://docs.microsoft.com/ef/core/](/ef/core/)

- **借助 Visual Studio 使用 ASP.NET Core 和 Entity Framework Core 入门** \
  [https://docs.microsoft.com/aspnet/core/data/ef-mvc/](/aspnet/core/data/ef-mvc/)

- **DbContext 类** \
  [https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext](xref:Microsoft.EntityFrameworkCore.DbContext)

- **比较 EF Core 和 EF6.x** \
  [https://docs.microsoft.com/ef/efcore-and-ef6/index](/ef/efcore-and-ef6/index)

## <a name="infrastructure-in-entity-framework-core-from-a-ddd-perspective"></a>Entity Framework Core 中的基础结构（DDD 角度）

从 DDD 的角度来看，EF 的一个重要功能是能够使用 POCO 域实体，在 EF 术语中也称为 POCO 代码优先实体。 如果使用 POCO 域实体，根据[持久性无感知](https://deviq.com/persistence-ignorance/)和[基础结构无感知](https://ayende.com/blog/3137/infrastructure-ignorance)原则，域模型类对于持久性是无感知的。

根据 DDD 模式，应在实体类自身内封装域行为和规则，以便在访问任何集合时，它可以控制不变量、验证和规则。 因此，从 DDD 角度来看，允许公开访问子实体或值对象的集合不是一个好做法。 相反，建议公开用于控制如何及何时更新字段和属性集合的方法，以及这种情况下应发生什么行为和操作的方法。

自 EF Core 1.1 开始，若要满足这些 DDD 要求，可以在实体中包含纯字段，而非公共属性。 如果不希望可从外部访问实体字段，创建特性或字段（而非属性）即可。 还可使用专用属性资源库。

相同地，现可使用类型化为 `IReadOnlyCollection<T>`（受依赖 EF 实现持久性的实体中集合（如 `List<T>`）的专用字段成员的支持）的公共属性对集合进行只读访问。 以前版本的 Entity Framework 需要集合属性来支持 `ICollection<T>`，这意味着任何使用父实体类的开发人员都无法通过其属性集合来添加或删除项。 这种可能性会违反 DDD 中的建议模式。

公开只读 `IReadOnlyCollection<T>` 对象时可使用专用集合，如以下代码示例所示：

```csharp
public class Order : Entity
{
    // Using private fields, allowed since EF Core 1.1
    private DateTime _orderDate;
    // Other fields ...

    private readonly List<OrderItem> _orderItems;
    public IReadOnlyCollection<OrderItem> OrderItems => _orderItems;

    protected Order() { }

    public Order(int buyerId, int paymentMethodId, Address address)
    {
        // Initializations ...
    }

    public void AddOrderItem(int productId, string productName,
                             decimal unitPrice, decimal discount,
                             string pictureUrl, int units = 1)
    {
        // Validation logic...

        var orderItem = new OrderItem(productId, productName,
                                      unitPrice, discount,
                                      pictureUrl, units);
        _orderItems.Add(orderItem);
    }
}
```

`OrderItems` 属性仅可通过 `IReadOnlyCollection<OrderItem>` 以只读形式进行访问。 此类型是只读的，因此它免受定期外部更新。

EF Core 提供一种方法，可将域模型映射到物理数据库，而不会“污染”域模型。 这是纯 .NET POCO 代码，因为映射操作在持久性层中实现。 在该映射操作中，需要配置“字段到数据库”映射。 在来自 `OrderingContext` 和 `OrderEntityTypeConfiguration` 类的 `OnModelCreating` 方法的以下示例中，调用 `SetPropertyAccessMode` 来告诉 EF Core 通过其字段访问 `OrderItems` 属性。

```csharp
// At OrderingContext.cs from eShopOnContainers
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
   // ...
   modelBuilder.ApplyConfiguration(new OrderEntityTypeConfiguration());
   // Other entities' configuration ...
}

// At OrderEntityTypeConfiguration.cs from eShopOnContainers
class OrderEntityTypeConfiguration : IEntityTypeConfiguration<Order>
{
    public void Configure(EntityTypeBuilder<Order> orderConfiguration)
    {
        orderConfiguration.ToTable("orders", OrderingContext.DEFAULT_SCHEMA);
        // Other configuration

        var navigation =
              orderConfiguration.Metadata.FindNavigation(nameof(Order.OrderItems));

        //EF access the OrderItem collection property through its backing field
        navigation.SetPropertyAccessMode(PropertyAccessMode.Field);

        // Other configuration
    }
}
```

使用字段而不使用属性时，`OrderItem` 实体得以持久保存，这就好像它有一个 `List<OrderItem>` 属性一样。 但是，它公开单个访问器（`AddOrderItem` 方法），用于将新项添加到订单。 因此，行为和数据绑定在一起，将在使用域模型的任何应用程序代码间保持一致。

## <a name="implement-custom-repositories-with-entity-framework-core"></a>使用 Entity Framework Core 实现自定义存储库

在实现级别上，当执行更新时，存储库就是一个具有数据持久性代码的类，由工作单元（EF Core 中的 DBContext）进行协调，如下面的类所示：

```csharp
// using directives...
namespace Microsoft.eShopOnContainers.Services.Ordering.Infrastructure.Repositories
{
    public class BuyerRepository : IBuyerRepository
    {
        private readonly OrderingContext _context;
        public IUnitOfWork UnitOfWork
        {
            get
            {
                return _context;
            }
        }

        public BuyerRepository(OrderingContext context)
        {
            _context = context ?? throw new ArgumentNullException(nameof(context));
        }

        public Buyer Add(Buyer buyer)
        {
            return _context.Buyers.Add(buyer).Entity;
        }

        public async Task<Buyer> FindAsync(string buyerIdentityGuid)
        {
            var buyer = await _context.Buyers
                .Include(b => b.Payments)
                .Where(b => b.FullName == buyerIdentityGuid)
                .SingleOrDefaultAsync();

            return buyer;
        }
    }
}
```

`IBuyerRepository` 接口来自于域模型层（采用协定的形式）。 但是，存储库在持久性和基础结构层上实现。

EF DbContext 经由依赖关系注入而通过构造函数。 它在同一 HTTP 请求范围内的多个存储库之间进行共享，这得益于其在 IoC 容器（也可使用 `services.AddDbContext<>` 进行显式设置）中的默认生存期 (`ServiceLifetime.Scoped`)。

### <a name="methods-to-implement-in-a-repository-updates-or-transactions-versus-queries"></a>存储库中的实现方法（更新或事务与查询）

每个存储库类中，都应放置持久性方法来更新受其相关聚合约束的实体状态。 请记住，聚合与其相关存储库之间存在一对一关系。 同时请考虑到，聚合根实体对象可能已将子实体嵌入其 EF 图内。 例如，购买者可能有作为相关子实体的多个支付方法。

由于 eShopOnContainers 中订购微服务的方式也基于 CQS/CQRS，大多数查询不在自定义存储库中实现。 一般而言，开发人员可以自由地为表示层创建所需查询和联接，而不受聚合、每个聚合的自定义存储库和 DDD 的限制。 本指南建议的大多数自定义存储库具有多个更新或事务性方法，但只有查询方法需要更新数据。 例如，BuyerRepository 存储库实现 FindAsync 方法，因为应用程序需要知道创建订单相关的新买家之前是否存在特定买家。

但是，正如所提及的，使数据发送至表示层或客户端应用的真正查询方法在使用 Dapper 基于灵活查询的 CQRS 查询中得以实现。

### <a name="using-a-custom-repository-versus-using-ef-dbcontext-directly"></a>使用自定义存储库与直接使用 EF DbContext

实体框架 DbContext 类基于工作单元和存储库模式，且可直接通过代码（如 ASP.NET Core MVC 控制器）进行使用。 工作单元模式和存储库模式产生最简单的代码，如 eShopOnContainers 的 CRUD 目录微服务中所示。 如果需要尽可能简单的代码，建议直接使用 DbContext 类，就像许多开发人员操作的那样。

但是，实现更复杂的微服务或应用程序时，实现自定义存储库将提供以下几个优势。 工作单元和存储库模式旨在封装基础结构持久性层，以便从应用程序和域模型层脱耦。 实现这些模式将促进用于模拟数据库访问的模拟数据库的使用。

图 7-18 中可以看到不使用存储库（直接使用 EF DbContext）与使用存储库（更易于模拟这些存储库）之间的差异。

![显示两个存储库中的组件和数据流的关系图。](./media/infrastructure-persistence-layer-implementation-entity-framework-core/custom-repo-versus-db-context.png)

**图 7-18**。 使用自定义存储库与纯 DbContext

图 7-18 显示了使用自定义存储库添加了抽象层，可用于通过模拟存储库来简化测试。 模拟时有多个备选项。 只模拟存储库或模拟整个工作单元。 通常情况下，只模拟存储库就足够了，不需要提取并模拟整个工作单元那般的复杂。

稍后，当我们关注应用程序层时，将看到依赖关系注入在 ASP.NET Core 中的工作方式，以及使用存储库时的实现方式。

简而言之，自定义存储库允许使用不受数据层状态影响的单元测试来更轻松地测试代码。 如果运行的测试还通过 Entity Framework 访问实际的数据库，它们不是单元测试而是更为缓慢的集成测试。

如果直接使用 DbContext，则必须模拟它，或通过使用包含单元测试的可预测数据的内存中 SQL Server 来运行单元测试。 但模拟 DbContext 或控制假数据所需完成的工作比在存储库级别进行模拟所需完成的工作多。 当然，可以始终测试 MVC 控制器。

## <a name="ef-dbcontext-and-iunitofwork-instance-lifetime-in-your-ioc-container"></a>IoC 容器中的 EF DbContext 和 IUnitOfWork 实例生存期

`DbContext` 对象（作为 `IUnitOfWork` 对象公开）应在相同 HTTP 请求范围内的多个存储库之间进行共享。 例如，当正在执行的操作必须处理多个聚合时，或只是因为正在使用多个存储库实例时，就是这种情况。 还需要提到的是，`IUnitOfWork` 接口属于域层，而不是 EF Core 类型。

为此，`DbContext` 对象的实例必须将其服务生存期设置为 ServiceLifetime.Scoped。 这是在 IoC 容器中通过 ASP.NET Core Web API 项目中 `Startup.cs` 文件的 ConfigureServices 方法向 `services.AddDbContext` 注册 `DbContext` 时的默认生存期。 下面的代码阐释这一点。

```csharp
public IServiceProvider ConfigureServices(IServiceCollection services)
{
    // Add framework services.
    services.AddMvc(options =>
    {
        options.Filters.Add(typeof(HttpGlobalExceptionFilter));
    }).AddControllersAsServices();

    services.AddEntityFrameworkSqlServer()
      .AddDbContext<OrderingContext>(options =>
      {
          options.UseSqlServer(Configuration["ConnectionString"],
                               sqlOptions => sqlOptions.MigrationsAssembly(typeof(Startup).GetTypeInfo().
                                                                                    Assembly.GetName().Name));
      },
      ServiceLifetime.Scoped // Note that Scoped is the default choice
                             // in AddDbContext. It is shown here only for
                             // pedagogic purposes.
      );
}
```

不应将 DbContext 实例化模式配置为 ServiceLifetime.Transient 或 ServiceLifetime.Singleton。

## <a name="the-repository-instance-lifetime-in-your-ioc-container"></a>IoC 容器中的存储库实例生存期

同样地，通常应将存储库生存期设置为范围内（Autofac 中的 InstancePerLifetimeScope）。 也可设置为短暂（Autofac 中的 InstancePerDependency），但使用范围内生存期时服务在内存方面会更有效率。

```csharp
// Registering a Repository in Autofac IoC container
builder.RegisterType<OrderRepository>()
    .As<IOrderRepository>()
    .InstancePerLifetimeScope();
```

DbContext 设置为范围内 (InstancePerLifetimeScope) 生存期（DBContext 的默认生存期）时，为存储库使用的单一生存期可能导致严重的并发问题。

### <a name="additional-resources"></a>其他资源

- **在 ASP.NET MVC 应用程序中实现存储库模式和工作单元模式** \
  <https://www.asp.net/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application>

- **Jonathan Allen。Entity Framework、Dapper 和 Chain 中存储库模式的实现策略** \
  <https://www.infoq.com/articles/repository-implementation-strategies>

- **Cesar de la Torre。比较 ASP.NET Core IoC 容器服务生存期和 Autofac IoC 容器实例范围** \
  <https://devblogs.microsoft.com/cesardelatorre/comparing-asp-net-core-ioc-service-life-times-and-autofac-ioc-instance-scopes/>

## <a name="table-mapping"></a>表映射

表映射标识要从数据库查询并保存到数据库的表数据。 前面你已了解如何使用域实体（例如，产品或订单域）来生成相关的数据库架构。 EF 特别围绕着约定的概念进行设计。 约定处理“表的名称是什么？” 或者“什么属性是主键？”这类问题 约定通常基于常规名称。 例如，主键通常是以 `Id` 结尾的属性。

按照约定，每个实体将设置为映射到名称与 `DbSet<TEntity>` 属性（公开派生上下文中的实体）相同的表中。 如果给定实体未提供任何 `DbSet<TEntity>` 值，则使用类名。

### <a name="data-annotations-versus-fluent-api"></a>数据注释与 Fluent API

存在许多其他 EF Core 约定，其中大多数可通过使用数据注释或 Fluent API 进行更改，并且在 OnModelCreating 方法内实现。

数据注释必须在实体模型类自身上使用，从 DDD 观点来看实体模型类更具侵入性。 这是因为你会因与基础结构数据库相关的数据注释而污染模型。 另一方面，Fluent API 可方便地更改数据持久性基础结构层中大部分的约定和映射，因此实体模型将保持整洁且从持久性基础结构分离。

### <a name="fluent-api-and-the-onmodelcreating-method"></a>Fluent API 和 OnModelCreating 方法

如前文所述，为了更改约定和映射，可以使用 DbContext 类中的 OnModelCreating 方法。

eShopOnContainers 中的订购微服务根据需要实现显式映射和配置，如以下代码所示。

```csharp
// At OrderingContext.cs from eShopOnContainers
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
   // ...
   modelBuilder.ApplyConfiguration(new OrderEntityTypeConfiguration());
   // Other entities' configuration ...
}

// At OrderEntityTypeConfiguration.cs from eShopOnContainers
class OrderEntityTypeConfiguration : IEntityTypeConfiguration<Order>
{
    public void Configure(EntityTypeBuilder<Order> orderConfiguration)
    {
        orderConfiguration.ToTable("orders", OrderingContext.DEFAULT_SCHEMA);

        orderConfiguration.HasKey(o => o.Id);

        orderConfiguration.Ignore(b => b.DomainEvents);

        orderConfiguration.Property(o => o.Id)
            .UseHiLo("orderseq", OrderingContext.DEFAULT_SCHEMA);

        //Address value object persisted as owned entity type supported since EF Core 2.0
        orderConfiguration
            .OwnsOne(o => o.Address, a =>
            {
                a.WithOwner();
            });

        orderConfiguration
            .Property<int?>("_buyerId")
            .UsePropertyAccessMode(PropertyAccessMode.Field)
            .HasColumnName("BuyerId")
            .IsRequired(false);

        orderConfiguration
            .Property<DateTime>("_orderDate")
            .UsePropertyAccessMode(PropertyAccessMode.Field)
            .HasColumnName("OrderDate")
            .IsRequired();

        orderConfiguration
            .Property<int>("_orderStatusId")
            .UsePropertyAccessMode(PropertyAccessMode.Field)
            .HasColumnName("OrderStatusId")
            .IsRequired();

        orderConfiguration
            .Property<int?>("_paymentMethodId")
            .UsePropertyAccessMode(PropertyAccessMode.Field)
            .HasColumnName("PaymentMethodId")
            .IsRequired(false);

        orderConfiguration.Property<string>("Description").IsRequired(false);

        var navigation = orderConfiguration.Metadata.FindNavigation(nameof(Order.OrderItems));

        // DDD Patterns comment:
        //Set as field (New since EF 1.1) to access the OrderItem collection property through its field
        navigation.SetPropertyAccessMode(PropertyAccessMode.Field);

        orderConfiguration.HasOne<PaymentMethod>()
            .WithMany()
            .HasForeignKey("_paymentMethodId")
            .IsRequired(false)
            .OnDelete(DeleteBehavior.Restrict);

        orderConfiguration.HasOne<Buyer>()
            .WithMany()
            .IsRequired(false)
            .HasForeignKey("_buyerId");

        orderConfiguration.HasOne(o => o.OrderStatus)
            .WithMany()
            .HasForeignKey("_orderStatusId");
    }
}
```

可以设置同一 `OnModelCreating` 方法内的所有 Fluent API 映射，但最好是将该代码进行分区并得到多个配置类，每个实体一个，如示例中所示。 尤其是对于大型模型，最好有单独的配置类用于配置不同的实体类型。

示例中的代码演示了几个显式声明和映射。 但是，EF Core 约定自动执行这里的许多映射，因此示例中需要的实际代码可能较小。

### <a name="the-hilo-algorithm-in-ef-core"></a>EF Core 中的 Hi/Lo 算法

前面示例中代码的一个有趣方面是，它使用 [Hi/Lo 算法](https://vladmihalcea.com/the-hilo-algorithm/)作为键生成策略。

在提交更改前需要唯一键时，Hi/Lo 算法很有用。 总之，Hi-Lo 算法将唯一标识符分配给表行，同时不依赖于立即将行存储于数据库中。 这使你立即开始使用标识符，就像常规顺序数据库 ID 中的那般。

Hi/Lo 算法描述用于从相关数据库序列获取一批唯一 ID 的机制。 因为该数据库可保证唯一性，所以这些 ID 可以安全地使用，用户之间因此不会有冲突。 出于这些原因，此算法很有趣：

- 它不中断工作单元模式。

- 它批量获取序列 ID，以最大限度地减少往返数据库的行程。

- 它生成一个人工可读标识符，不同于使用 GUID 的技术。

EF Core 支持使用 `UseHiLo` 方法的 [HiLo](https://stackoverflow.com/questions/282099/whats-the-hi-lo-algorithm)，如前面的示例中所示。

### <a name="map-fields-instead-of-properties"></a>映射字段（而非属性）

使用此功能（自 EF Core 1.1 之后可用），可以直接将列映射到字段。 可以不在实体类中使用属性，而只是将列从表格映射到字段。 它的常见用法是那些无需从实体外进行访问的任意内部状态的专用字段。

可以使用单个字段或集合来执行此操作，如 `List<>` 字段。 我们前面讨论对域模型类进行建模时已提过这点，但此处你可以了解如何通过前述代码中强调的 `PropertyAccessMode.Field` 配置来执行该映射。

### <a name="use-shadow-properties-in-ef-core-hidden-at-the-infrastructure-level"></a>在 EF Core 中使用隐藏在基础结构级别的阴影属性

EF Core 中的阴影属性是不存于实体类模型中的属性。 这些属性的值和状态完全在基础结构级别于 [ChangeTracker](/ef/core/api/microsoft.entityframeworkcore.changetracking.changetracker) 类中进行维护。

## <a name="implement-the-query-specification-pattern"></a>实现查询规范模式

如之前设计部分所述，查询规范模式是域驱动设计模式，设计用作可放置含可选排序及分页逻辑的查询定义的位置。

查询规范模式定义对象中的查询。 例如，为了封装搜索某些产品的分页查询，可以创建采用必要输入参数（pageNumber、pageSize、filter 等）的 PagedProduct 规范。 然后，在任何存储库方法（通常为 List() 重载）内，它会接受 IQuerySpecification 并基于该规范运行预期的查询。

常规规范接口示例如下面的 [eShopOnWeb](https://github.com/dotnet-architecture/eShopOnWeb) 的代码。

```csharp
// GENERIC SPECIFICATION INTERFACE
// https://github.com/dotnet-architecture/eShopOnWeb

public interface ISpecification<T>
{
    Expression<Func<T, bool>> Criteria { get; }
    List<Expression<Func<T, object>>> Includes { get; }
    List<string> IncludeStrings { get; }
}
```

然后，常规规范基类的实现如下所示。

```csharp
// GENERIC SPECIFICATION IMPLEMENTATION (BASE CLASS)
// https://github.com/dotnet-architecture/eShopOnWeb

public abstract class BaseSpecification<T> : ISpecification<T>
{
    public BaseSpecification(Expression<Func<T, bool>> criteria)
    {
        Criteria = criteria;
    }
    public Expression<Func<T, bool>> Criteria { get; }

    public List<Expression<Func<T, object>>> Includes { get; } =
                                           new List<Expression<Func<T, object>>>();

    public List<string> IncludeStrings { get; } = new List<string>();

    protected virtual void AddInclude(Expression<Func<T, object>> includeExpression)
    {
        Includes.Add(includeExpression);
    }

    // string-based includes allow for including children of children
    // e.g. Basket.Items.Product
    protected virtual void AddInclude(string includeString)
    {
        IncludeStrings.Add(includeString);
    }
}
```

以下规范在给定购物篮 ID 或购物篮所属买家的 ID 情况下来加载单个购物篮实体。 它将[立即加载](/ef/core/querying/related-data)购物篮的 `Items` 集合。

```csharp
// SAMPLE QUERY SPECIFICATION IMPLEMENTATION

public class BasketWithItemsSpecification : BaseSpecification<Basket>
{
    public BasketWithItemsSpecification(int basketId)
        : base(b => b.Id == basketId)
    {
        AddInclude(b => b.Items);
    }

    public BasketWithItemsSpecification(string buyerId)
        : base(b => b.BuyerId == buyerId)
    {
        AddInclude(b => b.Items);
    }
}
```

最后，可以在下方看到常规 EF 存储库可以如何使用此类规范来筛选并立即加载与给定实体类型 T 相关的数据。

```csharp
// GENERIC EF REPOSITORY WITH SPECIFICATION
// https://github.com/dotnet-architecture/eShopOnWeb

public IEnumerable<T> List(ISpecification<T> spec)
{
    // fetch a Queryable that includes all expression-based includes
    var queryableResultWithIncludes = spec.Includes
        .Aggregate(_dbContext.Set<T>().AsQueryable(),
            (current, include) => current.Include(include));

    // modify the IQueryable to include any string-based include statements
    var secondaryResult = spec.IncludeStrings
        .Aggregate(queryableResultWithIncludes,
            (current, include) => current.Include(include));

    // return the result of the query using the specification's criteria expression
    return secondaryResult
                    .Where(spec.Criteria)
                    .AsEnumerable();
}
```

除了封装筛选逻辑，该规范还可指定要返回的数据的形状，包括要填充的属性。

尽管我们不建议从存储库返回 `IQueryable`，但可以在存储库中使用它们来生成一系列结果。 可以看到此方法在以上 List 方法中使用，List 方法使用中间 `IQueryable` 表达式来生成查询的包含内容列表，然后再使用最后一行上的规范条件来执行查询。

### <a name="additional-resources"></a>其他资源

- **表映射** \
  [https://docs.microsoft.com/ef/core/modeling/relational/tables](/ef/core/modeling/relational/tables)

- **使用 HiLo 通过 Entity Framework Core 生成键** \
  <https://www.talkingdotnet.com/use-hilo-to-generate-keys-with-entity-framework-core/>

- **支持字段** \
  [https://docs.microsoft.com/ef/core/modeling/backing-field](/ef/core/modeling/backing-field)

- **Steve Smith.Entity Framework Core 中的封装集合** \
  <https://ardalis.com/encapsulated-collections-in-entity-framework-core>

- 阴影属性 \
  [https://docs.microsoft.com/ef/core/modeling/shadow-properties](/ef/core/modeling/shadow-properties)

- **规范模式** \
  <https://deviq.com/specification-pattern/>

> [!div class="step-by-step"]
> [上一页](infrastructure-persistence-layer-design.md)
> [下一页](nosql-database-persistence-infrastructure.md)
