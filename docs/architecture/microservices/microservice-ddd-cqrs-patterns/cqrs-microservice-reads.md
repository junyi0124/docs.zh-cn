---
title: 在 CQRS 微服务中实现读取/查询
description: 适用于容器化 .NET 应用程序的 .NET 微服务体系结构 | 了解如何使用 Dapper 在 eShopOnContainers 中的订购微服务上实现 CQRS 查询端。
ms.date: 10/08/2018
ms.openlocfilehash: e6ea7b4b7b37df9ee972319f597ab045bf3bd215
ms.sourcegitcommit: aa6d8a90a4f5d8fe0f6e967980b8c98433f05a44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2020
ms.locfileid: "90678798"
---
# <a name="implement-readsqueries-in-a-cqrs-microservice"></a>在 CQRS 微服务中实现读取/查询

对于读取/查询，来自 eShopOnContainers 引用应用程序的订购微服务独立于 DDD 模型和事务区域实现查询。 这主要是因为查询的需要和事务的需要有很大不同。 写入的执行事务必须符合域逻辑。 另一方面，查询是幂等的，可以与域规则分离。

如图 7-3 所示，这种方法很简单。 根据用户界面应用程序的要求，Web API 控制器使用任意基础结构（如 Dapper 等微观对象关系映射程序 (ORM)）并返回动态 ViewModel 就可实现 API 接口。

![在简化的 CQRS 中显示高级查询端的关系图。](./media/cqrs-microservice-reads/simple-approach-cqrs-queries.png)

**图 7-3**。 CQRS 微服务中用于查询的最简单方法

可以通过使用微型 ORM（如 Dapper）查询数据库（返回动态 ViewModel），实现采用简化 CQRS 方法的最简单查询端方法。 查询定义查询数据库并返回为每个查询动态构建的动态 ViewModel。 因为查询是幂等的，所以无论查询运行多少次，数据都不会更改。 因此，不会受到事务端所用 DDD 模式的限制（如聚合和其他模式），这也是查询与事务区域分离的原因。 查询数据库以获取 UI 需要的数据，并返回动态 ViewModel，除在 SQL 语句中外，动态 ViewModel 不需要在任何地方静态定义（ViewModel 没有类）。

因为这种方法很简单，因此查询端所需的代码（例如使用 [Dapper](https://github.com/StackExchange/Dapper) 等微型 ORM 的代码）可[在同一 Web API 项目](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/Queries/OrderQueries.cs)中实现。 如图 7-4 所示。 查询在 eShopOnContainers 解决方案的“Ordering.API”微服务项目中定义。

![Ordering.API 项目的“查询”文件夹的屏幕截图。](./media/cqrs-microservice-reads/ordering-api-queries-folder.png)

**图 7-4**。 eShopOnContainers 的订购微服务中的查询

## <a name="use-viewmodels-specifically-made-for-client-apps-independent-from-domain-model-constraints"></a>使用专为客户端应用构建的 ViewModel（不受域模型的约束）

由于执行查询是为获取客户端应用程序所需的数据，因此返回类型应根据查询所返回的数据专为客户端构建。 这些模型或数据传输对象 (DTO) 称为 ViewModel。

返回数据 (ViewModel) 可以是来自数据库中多个实体或表的联接数据结果，或者是事务区域域模型中定义的多个聚合中的联接数据结果。 在这种情况下，因为正在创建独立于域模型的查询，所以忽略了聚合边界和约束，因此可随意查询可能需要的任何表和列。 这种方法极大地提高了开发人员创建或更新查询的效率和灵活性。

ViewModel 可以是在类中定义的静态类型（如在排序微服务中实现的那样）。 或者可根据执行的查询动态创建它们，开发人员可灵活处理。

## <a name="use-dapper-as-a-micro-orm-to-perform-queries"></a>使用 Dapper 作为微型 ORM 以执行查询

可使用任何微型 ORM、Entity Framework Core 甚至普通的 ADO.NET 进行查询。 在示例应用程序中，选择 Dapper 用于 eShopOnContainers 中的订单微服务，这是常用微型 ORM 的一个良好示例。 由于它是一个轻量化的框架，因此能够以极佳的性能运行普通 SQL 查询。 使用 Dapper，可写入一个可访问和联接多个表的 SQL 查询。

Dapper 是开源项目（最初由 Sam Saffron 创建），也是在 [Stack Overflow](https://stackoverflow.com/) 中使用的构建基块的一部分。 要使用 Dapper，只需通过 [Dapper NuGet 包](https://www.nuget.org/packages/Dapper)进行安装，如下图所示：

![“NuGet 包”视图中 Dapper 包的屏幕截图。](./media/cqrs-microservice-reads/drapper-package-nuget.png)

还需要添加一个 `using` 指令，使代码具有 Dapper 扩展方法的访问权限。

在代码中使用 Dapper 时，可直接使用 <xref:System.Data.SqlClient> 命名空间中提供的 <xref:System.Data.SqlClient.SqlConnection> 类。 通过 QueryAsync 方法和扩展 <xref:System.Data.SqlClient.SqlConnection> 类的其他扩展方法，可以直观而高效地运行查询。

## <a name="dynamic-versus-static-viewmodels"></a>动态与静态 Viewmodel

将 ViewModel 从服务器端返回到客户端应用时，可将这些 ViewModel 看作 DTO（数据传输对象），这些 DTO 与实体模型的内部实体域不同，因为 ViewModel 以客户端应用所需的方式保存数据。 因此，在许多情况下，可以聚合来自多个域实体的数据，并根据客户端应用需要数据的方式精确地组合这些 ViewModel。

可以显式定义这些 Viewmodel 或 DTO（作为数据持有者类），如后面的代码段中所示的 `OrderSummary` 类。 或者，可以简单地根据查询返回的属性（作为动态类型）返回动态 Viewmodel 或动态 DTO。

### <a name="viewmodel-as-dynamic-type"></a>ViewModel 作为动态类型

如以下代码所示，通过只返回一个内部基于查询所返回属性的动态类型，查询可直接返回 `ViewModel`。 这意味着要返回的属性的子集是基于查询本身的。 因此，如果将新列添加到查询或联接，则该数据将动态添加到返回的 `ViewModel` 中。

```csharp
using Dapper;
using Microsoft.Extensions.Configuration;
using System.Data.SqlClient;
using System.Threading.Tasks;
using System.Dynamic;
using System.Collections.Generic;

public class OrderQueries : IOrderQueries
{
    public async Task<IEnumerable<dynamic>> GetOrdersAsync()
    {
        using (var connection = new SqlConnection(_connectionString))
        {
            connection.Open();
            return await connection.QueryAsync<dynamic>(
                @"SELECT o.[Id] as ordernumber,
                o.[OrderDate] as [date],os.[Name] as [status],
                SUM(oi.units*oi.unitprice) as total
                FROM [ordering].[Orders] o
                LEFT JOIN[ordering].[orderitems] oi ON o.Id = oi.orderid
                LEFT JOIN[ordering].[orderstatus] os on o.OrderStatusId = os.Id
                GROUP BY o.[Id], o.[OrderDate], os.[Name]");
        }
    }
}
```

重点在于，通过使用动态类型，返回的数据集合将动态组装为 ViewModel。

**优点：** 无论何时更新查询的 SQL 语句，此方法都可降低修改静态 ViewModel 类的需求，这使得此设计方法在编码时非常灵活、直观，并在未来更改时能快速改善。

**缺点：** 长远来看，动态类型可能会对清晰度以及服务与客户端应用的兼容性造成不利影响。 此外，如果使用动态类型，中间件软件（如 Swashbuckle）无法为返回类型提供相同级别的文档。

### <a name="viewmodel-as-predefined-dto-classes"></a>ViewModel 作为预定义的 DTO 类

**优点：** 具有静态预定义的 ViewModel 类（如基于显式 DTO 类的“协定”）毫无疑问有利于公共 API 和长期微服务，即使它们只用于同一应用程序。

如果想要为 Swagger 指定响应类型，需要使用显式 DTO 类作为返回类型。 因此，预定义的 DTO 类允许从 Swagger 提供更丰富的信息。 它可在使用 API 时改善 API 文档和兼容性。

**缺点：** 如前所述，更新代码时，更新 DTO 类需要执行更多步骤。

*经验之谈：* 在 eShopOnContainers 的订单微服务内实现的查询中，开始使用动态 ViewModel 进行开发，这是因为它在早期开发阶段具有直观性和灵活性。 但是，一旦开发稳定后，我们将选择重构 API，并将静态或预定义的 DTO 用于 ViewModel，因为这样能使微服务的使用者更加清晰地了解用作“协定”的显式 DTO 类型。

在以下示例中，可以看到查询如何使用显式 ViewModel DTO 类返回数据：OrderSummary 类。

```csharp
using Dapper;
using Microsoft.Extensions.Configuration;
using System.Data.SqlClient;
using System.Threading.Tasks;
using System.Dynamic;
using System.Collections.Generic;

public class OrderQueries : IOrderQueries
{
  public async Task<IEnumerable<OrderSummary>> GetOrdersAsync()
    {
        using (var connection = new SqlConnection(_connectionString))
        {
            connection.Open();
            return await connection.QueryAsync<OrderSummary>(
                  @"SELECT o.[Id] as ordernumber,
                  o.[OrderDate] as [date],os.[Name] as [status],
                  SUM(oi.units*oi.unitprice) as total
                  FROM [ordering].[Orders] o
                  LEFT JOIN[ordering].[orderitems] oi ON  o.Id = oi.orderid
                  LEFT JOIN[ordering].[orderstatus] os on o.OrderStatusId = os.Id
                  GROUP BY o.[Id], o.[OrderDate], os.[Name]
                  ORDER BY o.[Id]");
        }
    }
}
```

#### <a name="describe-response-types-of-web-apis"></a>介绍 Web API 的响应类型

使用 Web API 和微服务的开发人员最关心的是返回的内容 - 特别是响应类型和错误代码（如果不是标准内容）。 响应类型在 XML 注释和数据批注中进行处理。

如果没有关于 Swagger UI 的合适文档，使用者无法了解返回的类型或可返回的 HTTP 代码。 该问题已通过添加 <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute?displayProperty=nameWithType> 解决，因此 Swashbuckle 可以生成 API 返回模型和值的更加详细的信息，如以下代码所示：

```csharp
namespace Microsoft.eShopOnContainers.Services.Ordering.API.Controllers
{
    [Route("api/v1/[controller]")]
    [Authorize]
    public class OrdersController : Controller
    {
        //Additional code...
        [Route("")]
        [HttpGet]
        [ProducesResponseType(typeof(IEnumerable<OrderSummary>),
            (int)HttpStatusCode.OK)]
        public async Task<IActionResult> GetOrders()
        {
            var userid = _identityService.GetUserIdentity();
            var orders = await _orderQueries
                .GetOrdersFromUserAsync(Guid.Parse(userid));
            return Ok(orders);
        }
    }
}
```

但是，`ProducesResponseType` 属性无法使用动态类型，而需要使用显式类型，如 `OrderSummary` ViewModel DTO，如下例所示：

```csharp
public class OrderSummary
{
    public int ordernumber { get; set; }
    public DateTime date { get; set; }
    public string status { get; set; }
    public double total { get; set; }
}
```

这也是长期来看，显式返回类型比动态类型更好的另一原因。 使用 `ProducesResponseType` 属性时，还可以指定关于可能的 HTTP 错误/代码（如 200、400 等）的预期结果。

下图中，可以看到 Swagger UI 如何显示 ResponseType 信息。

![Ordering.API 的 Swagger UI 页面的屏幕截图。](./media/cqrs-microservice-reads/swagger-ordering-http-api.png)

**图 7-5**。 显示来自 Web API 的响应类型和可能的 HTTP 状态代码的 Swagger UI

此图像显示了基于 ViewModel 类型的一些示例值，以及可能返回的 HTTP 状态代码。

## <a name="additional-resources"></a>其他资源

- **Dapper**  
 <https://github.com/StackExchange/dapper-dot-net>

- **Julie Lerman.数据点 - Dapper、Entity Framework 和混合应用（MSDN 杂志文章）**  
  <https://docs.microsoft.com/archive/msdn-magazine/2016/may/data-points-dapper-entity-framework-and-hybrid-apps>

- **使用 Swagger 的 ASP.NET Core Web API 帮助页**  
  <https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger?tabs=visual-studio>

>[!div class="step-by-step"]
>[上一页](eshoponcontainers-cqrs-ddd-microservice.md)
>[下一页](ddd-oriented-microservice.md)
