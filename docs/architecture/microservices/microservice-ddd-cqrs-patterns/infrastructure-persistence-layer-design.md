---
title: 设计基础结构持久性层
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 在基础结构持久性层的设计中探索存储库模式。
ms.date: 10/08/2018
ms.openlocfilehash: 3c18582eb5db61a61b366c06f361d297e698b39a
ms.sourcegitcommit: 4ad2f8920251f3744240c3b42a443ffbe0a46577
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/08/2020
ms.locfileid: "86100842"
---
# <a name="design-the-infrastructure-persistence-layer"></a>设计基础结构持久性层

数据持久性组件提供对微服务边界（即微服务的数据库）内托管的数据的访问。 它们包含组件（例如存储库和[工作单元](https://martinfowler.com/eaaCatalog/unitOfWork.html)类）的实际实现，例如自定义实体框架 (EF) <xref:Microsoft.EntityFrameworkCore.DbContext> 对象。 EF DbContext 实现两种模式：存储库模式和工作单元模式。

## <a name="the-repository-pattern"></a>存储库模式

存储库是封装访问数据源所需逻辑的类或组件。 它们集中提供常见的数据访问功能，从而提供更好的可维护性，并将用于访问数据库的基础结构或技术与域模型层分离。 如果使用像实体框架这样的对象关系映射 (ORM)，则可通过 LINQ 和强类型化来简化必须实现的代码。 这样你就可以专注于数据持久性逻辑而不是数据访问管道。

存储库模式是一种使用数据源的详细记录方式。 在 [Patterns of Enterprise Application Architecture](https://www.amazon.com/Patterns-Enterprise-Application-Architecture-Martin/dp/0321127420/)（企业应用程序体系结构模式）一书中，Martin Fowler 是这样描述存储库的：

> 存储库执行域模型层与数据映射之间的中介者的任务，以类似于内存中一组域对象的方式工作。 客户端对象以声明方式生成查询，并将它们发送到存储库以获取答案。 从概念上讲，存储库封装了存储在数据库中的一组对象以及可对其执行的操作，从而提供一种更接近持久性层的方式。 存储库还明确支持在一个方向上分离工作域与数据分配或映射之间的依赖关系。

### <a name="define-one-repository-per-aggregate"></a>为每个聚合定义一个存储库

对于每个聚合或聚合根，应创建一个存储库类。 在基于域驱动设计 (DDD) 模式的微服务中，唯一应该用于更新数据库的渠道应是存储库。 这是因为它们与聚合根具有一对一的关系，聚合根控制着聚合的不变量和事务一致性。 可以通过其他渠道查询数据库（就像使用 CQRS 方法时一样），因为查询不会更改数据库的状态。 但是，事务区域（即更新）必须始终由存储库和聚合根控制。

从本质上讲，存储库允许以域实体的形式填充内存中来自数据库的数据。 一旦实体在内存中，就可以对它们进行更改，然后通过事务存回数据库中。

如前所述，如果使用 CQS/CQRS 体系结构模式，则由简单的 SQL 语句使用 Dapper 在域模型外并行执行初始查询。 这种方法比存储库更加灵活，因为可以查询和联接所需的任何表，并且这些查询不受聚合中的规则限制。 该数据会发送到表示层或客户端应用。

如果用户进行更改，要更新的数据会从客户端应用或表示层发送到应用层（例如 Web API 服务）。 在命令处理程序中收到命令时，可以使用存储库获取要从数据库更新的数据。 可使用通过命令传递的数据在内存中对其进行更新，然后通过事务在数据库中添加或更新数据（域实体）。

需要再次强调的是，应该仅为每个聚合根定义一个存储库，如图 7-17 所示。 若要实现聚合根的目标，即维持聚合中所有对象之间的事务一致性，就决不能为数据库中的每个表创建一个存储库。

![显示域和其他基础结构的关系的关系图。](./media/infrastructure-persistence-layer-design/repository-aggregate-database-table-relationships.png)

**图 7-17**。 存储库、聚合和数据库表之间的关系

上图显示了域层和基础结构层之间的关系：购买者聚合依赖于 IBuyerRepository 接口，而订单聚合依赖于 IOrderRepository 接口，这些接口由依赖于 UnitOfWork（可访问数据层中的表，也在此处实现）的对应存储库在基础结构层中实现。

### <a name="enforce-one-aggregate-root-per-repository"></a>强制每个存储库使用一个聚合根

以这样一种方式实现存储库设计可能很有价值，即实施仅聚合根具有存储库的规则。 可以创建一个泛型或基本存储库类型，以限制其使用的实体类型，确保这些实体具有 `IAggregateRoot` 标记接口。

这样，在基础结构层实现的每个存储库类都会实现自己的协定或接口，如以下代码所示：

```csharp
namespace Microsoft.eShopOnContainers.Services.Ordering.Infrastructure.Repositories
{
    public class OrderRepository : IOrderRepository
    {
      // ...
    }
}
```

每个特定的存储库接口都实现 IRepository 泛型接口：

```csharp
public interface IOrderRepository : IRepository<Order>
{
    Order Add(Order order);
    // ...
}
```

但是，若要让代码强制执行每个存储库都应与单个聚合关联的约定，一种更好的方式是实现泛型存储库类型。 这样便可以明确看出正在使用存储库定位特定聚合。 这可以通过实现泛型 `IRepository` 基接口轻松完成，如以下代码所示：

```csharp
public interface IRepository<T> where T : IAggregateRoot
{
    //....
}
```

### <a name="the-repository-pattern-makes-it-easier-to-test-your-application-logic"></a>存储库模式使应用程序逻辑测试更轻松

存储库模式允许使用单元测试轻松测试应用程序。 请记住，单元测试只测试代码，不测试基础结构，所以存储库抽象使得它更容易实现这个目标。

如前面部分所述，建议在域模型层中定义并放置存储库接口，这样，应用层（例如，Web API 微服务）就不会直接依赖于已实现实际存储库类的基础结构层。 通过执行此操作并在 Web API 的控制器中使用依赖项注入，可以实现模拟存储库来返回假数据，而不是数据库中的数据。 这种分离方法可用于创建和运行单元测试，这些单元测试着重于测试应用程序的逻辑，而无需连接到数据库。

与数据库的连接可能会失败，更重要的是，针对数据库运行数百个测试的做法并不可取，原因有两个。 首先，由于存在大量测试，因此，运行起来可能很费时。 其次，数据库记录可能会更改并影响测试结果，因此它们可能不一致。 针对数据库进行的测试不是单元测试，而是集成测试。 你应该有许多快速运行的单元测试，较少针对数据库的集成测试。

根据单元测试的关注点分离原则，你的逻辑应作用于内存中的域实体。 它假定存储库类已经提供这些实体。 一旦你的逻辑修改了域实体，它就会假定存储库类将正确存储它们。 这里的重点在于针对域模型及其域逻辑创建单元测试。 聚合根是 DDD 中的主要一致性边界。

在 eShopOnContainers 中实现的存储库依赖于使用其更改跟踪器的存储库模式和工作单元模式的 EF Core DbContext 实现，因此，它们不会复制此功能。

### <a name="the-difference-between-the-repository-pattern-and-the-legacy-data-access-class-dal-class-pattern"></a>存储库模式与传统数据访问类（DAL 类）模式之间的区别

数据访问对象直接对存储执行数据访问和持久性操作。 存储库使用要在工作单元对象的内存中执行的操作对数据进行标记（正如在 EF 中使用 <xref:Microsoft.EntityFrameworkCore.DbContext> 类时一样），但这些更新不会立即执行到数据库。

工作单元亦称单个事务，涉及多个插入、更新或删除操作。 简而言之，这意味着对于特定的用户操作（例如网站注册），所有插入、更新和删除操作都在单个事务中处理。 这比以更繁琐的方式处理多个数据库事务更有效。

这几个持久性操作稍后会在应用层中的代码发出命令时，在单个操作中执行。 关于将内存中更改应用于实际数据库存储的决策通常基于[工作单元模式](https://martinfowler.com/eaaCatalog/unitOfWork.html)。 在 EF 中，工作单元模式作为 <xref:Microsoft.EntityFrameworkCore.DbContext> 实现。

在许多情况下，这种对存储应用操作的模式或方式可以提高应用程序性能并减少出现不一致的可能性。 此外，它还能减少数据库表中的事务阻塞，因为所有预期操作都作为一个事务的一部分进行提交。 与对数据库执行许多独立操作相比，这样做效率更高。 因此，选定的 ORM 能够通过在同一个事务中对几个更新操作进行分组来优化对数据库的执行，而不是使用许多单独的小型事务执行。

### <a name="repositories-shouldnt-be-mandatory"></a>存储库不应该是强制性的

自定义存储库适用于前面提到的原因，这也是 eShopOnContainers 中的订购微服务所采用的方法。 但是，它不是一种必须在 DDD 设计中甚至在一般的 .NET 开发中实现的模式。

例如，Jimmy Bogard 在为本指南提供直接反馈时表示：

> 这可能是我最重要的一次反馈。 我真的不喜欢存储库，主要是因为它们隐藏了底层持久性机制的重要细节。 这也是我选择 MediatR 命令的原因。 我可以利用持久性层的完整功能，将所有域行为推送到我的聚合根中。 我通常不想模拟存储库 — 我仍然需要对真实存在的东西执行集成测试。 使用 CQRS 意味着我们不再需要存储库。

虽然存储库可能会很有用，但就设计聚合模式和丰富域模型而言，它们对 DDD 设计并不重要。 因此，你可以根据需要使用或不使用存储库模式。 不管怎样，你将在使用 EF Core 时使用存储库模式，在这种情况下，存储库涵盖整个微服务或界定上下文。

## <a name="additional-resources"></a>其他资源

### <a name="repository-pattern"></a>存储库模式

- **Edward Hieatt 和 Rob Mee.Repository pattern**（存储库模式）。 \
  <https://martinfowler.com/eaaCatalog/repository.html>

- **The Repository pattern**（存储库模式） \
  <https://docs.microsoft.com/previous-versions/msp-n-p/ff649690(v=pandp.10)>

- **Eric Evans。域驱动设计：Tackling Complexity in the Heart of Software.** （域驱动设计：软件核心复杂性应对之道） （书；包含对存储库模式的讨论）\
  <https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/>

### <a name="unit-of-work-pattern"></a>工作单元模式

- **Martin Fowler。Unit of Work pattern**（工作单元模式）。 \
  <https://martinfowler.com/eaaCatalog/unitOfWork.html>

- **Implementing the Repository and Unit of Work Patterns in an ASP.NET MVC Application**（在 ASP.NET MVC 应用程序中实现存储库模式和工作单元模式） \
  <https://docs.microsoft.com/aspnet/mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application>

>[!div class="step-by-step"]
>[上一页](domain-events-design-implementation.md)
>[下一页](infrastructure-persistence-layer-implementation-entity-framework-core.md)
