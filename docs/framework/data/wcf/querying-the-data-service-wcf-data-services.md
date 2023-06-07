---
title: 查询数据服务（WCF 数据服务）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, client library
- WCF Data Services, querying
- WCF Data Services, accessing data
ms.assetid: 823e9444-27aa-4f1f-be8e-0486d67f54c0
ms.openlocfilehash: 13334f6425c47e45d729d606d99602a99f35d8e6
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84286153"
---
# <a name="querying-the-data-service-wcf-data-services"></a>查询数据服务（WCF 数据服务）

WCF 数据服务客户端库允许使用熟悉的 .NET Framework 编程模式（包括使用语言集成查询（LINQ））对数据服务执行查询。 客户端库将在客户端上定义为 <xref:System.Data.Services.Client.DataServiceQuery%601> 类实例的查询转换为 HTTP GET 请求消息。 该库接收响应消息并将该消息转换为客户端数据服务类的实例。 <xref:System.Data.Services.Client.DataServiceContext> 所属的 <xref:System.Data.Services.Client.DataServiceQuery%601> 跟踪这些类。

## <a name="data-service-queries"></a>数据服务查询

<xref:System.Data.Services.Client.DataServiceQuery%601> 泛型类表示一个查询，该查询返回一个包含零个或零个以上实体类型实例的集合。 数据服务查询始终属于现有数据服务上下文。 此上下文含有撰写和执行查询所必需的服务 URI 和元数据信息。

使用**添加服务引用**对话框将数据服务添加到基于 .NET Framework 的客户端应用程序时，将创建一个从类继承的实体容器类 <xref:System.Data.Services.Client.DataServiceContext> 。 此类包括返回类型化 <xref:System.Data.Services.Client.DataServiceQuery%601> 实例的属性。 数据服务公开的每个实体集对应一个属性。 使用这些属性可以更容易地创建类型化 <xref:System.Data.Services.Client.DataServiceQuery%601> 的实例。

在以下情况下会执行查询：

- 隐式枚举结果时，如：

  - <xref:System.Data.Services.Client.DataServiceContext> 上的某个属性表示并枚举实体集时，如在 `foreach` (C#) 或 `For Each` (Visual Basic) 循环期间。

  - 将查询赋给 `List` 集合时。

- 显式调用 <xref:System.Data.Services.Client.DataServiceQuery%601.Execute%2A> 或 <xref:System.Data.Services.Client.DataServiceQuery%601.BeginExecute%2A> 方法时。

- 调用 LINQ 查询执行运算符（例如 <xref:System.Linq.Enumerable.First%2A> 或 <xref:System.Linq.Enumerable.Single%2A>）时。

下面的查询在执行时返回罗斯文数据服务中的所有 `Customers` 实体：

[!code-csharp[Astoria Northwind Client#GetAllCustomersSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#getallcustomersspecific)]
[!code-vb[Astoria Northwind Client#GetAllCustomersSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#getallcustomersspecific)]

有关详细信息，请参阅[如何：执行数据服务查询](how-to-execute-data-service-queries-wcf-data-services.md)。

WCF 数据服务客户端支持对后期绑定对象的查询，例如在 c # 中使用*动态*类型时。 但是，出于性能方面的考虑，应始终针对数据服务编写强类型查询。 客户端不支持 <xref:System.Tuple> 类型和动态对象。

## <a name="linq-queries"></a>LINQ 查询

由于 <xref:System.Data.Services.Client.DataServiceQuery%601> 类实现 <xref:System.Linq.IQueryable%601> linq 定义的接口，因此 WCF 数据服务的客户端库可以将针对实体集数据的 LINQ 查询转换为一个 URI，该 URI 表示针对数据服务资源计算的查询表达式。 下面的示例是一个等效于之前 <xref:System.Data.Services.Client.DataServiceQuery%601> 的 LINQ 查询，它返回运费成本超过 30 美元的 `Orders` 并按运费成本对结果进行排序：

[!code-csharp[Astoria Northwind Client#AddQueryOptionsLinqSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addqueryoptionslinqspecific)]
[!code-vb[Astoria Northwind Client#AddQueryOptionsLinqSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addqueryoptionslinqspecific)]

此 LINQ 查询转换为对基于 Northwind 的[快速入门](quickstart-wcf-data-services.md)数据服务执行的以下查询 URI：

```http
http://localhost:12345/Northwind.svc/Orders?Orderby=ShippedDate&?filter=Freight gt 30
```

> [!NOTE]
> 可以采用 LINQ 语法表示的查询集大于数据服务使用的基于具象状态传输 (REST) 的 URI 语法所支持的查询集。 如果无法将查询映射到目标数据服务中的 URI，则会引发 <xref:System.NotSupportedException>。

有关详细信息，请参阅[LINQ 注意事项](linq-considerations-wcf-data-services.md)。

## <a name="adding-query-options"></a>添加查询选项

数据服务查询支持 WCF 数据服务提供的所有查询选项。 调用 <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> 方法可向 <xref:System.Data.Services.Client.DataServiceQuery%601> 实例追加查询选项。 <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> 返回一个新的 <xref:System.Data.Services.Client.DataServiceQuery%601> 实例，该实例等效于原始查询，但带有新的查询选项集。 下面的查询在执行时会返回按 `Orders` 值进行筛选并按 `Freight` 降序排序的 `OrderID`：

[!code-csharp[Astoria Northwind Client#AddQueryOptionsSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addqueryoptionsspecific)]
[!code-vb[Astoria Northwind Client#AddQueryOptionsSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addqueryoptionsspecific)]

可以使用 `$orderby` 查询选项基于单个属性对查询进行排序和筛选，如下面的示例所示，该示例基于 `Orders` 属性的值对返回的 `Freight` 对象进行筛选和排序：

[!code-csharp[Astoria Northwind Client#OrderWithFilter](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#orderwithfilter)]
[!code-vb[Astoria Northwind Client#OrderWithFilter](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#orderwithfilter)]

可以连续调用 <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> 方法来构造复杂的查询表达式。 有关详细信息，请参阅[如何：将查询选项添加到数据服务查询](how-to-add-query-options-to-a-data-service-query-wcf-data-services.md)。

查询选项为表示 LINQ 查询的语法成分提供了另一种方法。 有关详细信息，请参阅[LINQ 注意事项](linq-considerations-wcf-data-services.md)。

> [!NOTE]
> 不能使用 `$select` 方法将 <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> 查询选项添加到查询 URI 中。 我们建议使用 LINQ <xref:System.Linq.Enumerable.Select%2A> 方法，让客户端在请求 URI 中生成 `$select` 查询选项。

<a name="executingQueries"></a>

## <a name="client-versus-server-execution"></a>客户端执行与服务器执行

客户端分两个部分来执行查询。 只要有可能，将首先在客户端上计算查询中的表达式，然后生成基于 URI 的查询并将其发送至数据服务，以针对服务中的数据进行计算。 请考虑以下 LINQ 查询：

[!code-csharp[Astoria Northwind Client#LinqQueryClientEvalSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqqueryclientevalspecific)]
[!code-vb[Astoria Northwind Client#LinqQueryClientEvalSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqqueryclientevalspecific)]

在本例中，表达式 `(basePrice – (basePrice * discount))` 在客户端上进行计算。 因此，发送至数据服务的实际查询 URI `http://localhost:12345/northwind.svc/Products()?$filter=(UnitPrice gt 90.00M) and substringof('bike',ProductName)` 将在筛选子句中包含已计算的十进制值 `90`。 数据服务将计算筛选表达式的其他部分，包括子字符串表达式。 在客户端上计算的表达式遵循公共语言运行时（CLR）语义，而发送到数据服务的表达式依赖于 OData 协议的数据服务实现。 还应当注意这种分别计算会导致意外结果的情况，例如当客户端和服务同时在不同时区中执行基于时间的计算时。

## <a name="query-responses"></a>查询响应

执行 <xref:System.Data.Services.Client.DataServiceQuery%601> 时将返回所请求的实体类型的 <xref:System.Collections.Generic.IEnumerable%601>。 可以将此查询结果强制转换为 <xref:System.Data.Services.Client.QueryOperationResponse%601> 对象，如下面的示例所示：

[!code-csharp[Astoria Northwind Client#GetResponseSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#getresponsespecific)]
[!code-vb[Astoria Northwind Client#GetResponseSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#getresponsespecific)]

表示数据服务中的实体的实体类型实例是在客户端上由称为对象具体化的过程创建的。 有关详细信息，请参阅[对象具体化](object-materialization-wcf-data-services.md)。 <xref:System.Data.Services.Client.QueryOperationResponse%601> 对象实现 <xref:System.Collections.Generic.IEnumerable%601> 以提供对查询结果的访问。

<xref:System.Data.Services.Client.QueryOperationResponse%601> 还包括下列成员，它们可用来访问有关查询结果的其他信息：

- <xref:System.Data.Services.Client.OperationResponse.Error%2A> - 获取由操作引发的错误（如果发生了这样的错误）。

- <xref:System.Data.Services.Client.OperationResponse.Headers%2A> - 包含与查询响应关联的 HTTP 响应标头的集合。

- <xref:System.Data.Services.Client.QueryOperationResponse.Query%2A> - 获取生成了 <xref:System.Data.Services.Client.DataServiceQuery%601> 的原始 <xref:System.Data.Services.Client.QueryOperationResponse%601>。

- <xref:System.Data.Services.Client.OperationResponse.StatusCode%2A> - 获取查询响应的 HTTP 响应代码。

- <xref:System.Data.Services.Client.QueryOperationResponse%601.TotalCount%2A> - 获取在对 <xref:System.Data.Services.Client.DataServiceQuery%601.IncludeTotalCount%2A> 调用 <xref:System.Data.Services.Client.DataServiceQuery%601> 方法时，实体集中的实体总数。

- <xref:System.Data.Services.Client.QueryOperationResponse.GetContinuation%2A> - 返回一个 <xref:System.Data.Services.Client.DataServiceQueryContinuation> 对象，该对象包含下一页结果的 URI。

默认情况下，WCF 数据服务仅返回查询 URI 显式选择的数据。 这样即提供了在需要时从数据服务显式加载其他数据的选项。 每次从数据服务显式加载数据时都会向数据服务发送一个请求。 可以显式加载的数据包括相关实体、分页响应数据以及二进制数据流。

> [!NOTE]
> 由于数据服务可能返回分页响应，因此建议您的应用程序使用编程模式来处理分页的数据服务响应。 有关详细信息，请参阅[加载延迟的内容](loading-deferred-content-wcf-data-services.md)。

还可以通过指定在响应中仅返回某个实体的某些属性来减少查询返回的数据量。 有关详细信息，请参阅[查询投影](query-projections-wcf-data-services.md)。

## <a name="getting-a-count-of-the-total-number-of-entities-in-the-set"></a>获取集合中实体的总数

在某些方案中，不仅知道查询返回的实体数，还要知道实体集中实体的总数，这一点非常有帮助。 调用 <xref:System.Data.Services.Client.DataServiceQuery%601.IncludeTotalCount%2A> 上的 <xref:System.Data.Services.Client.DataServiceQuery%601> 方法可请求在查询结果中包含集中实体的总数。 在这种情况下，返回的 <xref:System.Data.Services.Client.QueryOperationResponse%601.TotalCount%2A> 的 <xref:System.Data.Services.Client.QueryOperationResponse%601> 属性返回集中实体的总数。

还可以仅获取集合中实体的总数，该总数可作为 <xref:System.Int32> 或 <xref:System.Int64> 值，方法是分别调用 <xref:System.Linq.Enumerable.Count%2A> 或 <xref:System.Linq.Enumerable.LongCount%2A> 方法。 调用这些方法时，不会返回 <xref:System.Data.Services.Client.QueryOperationResponse%601>；仅返回计数值。 有关详细信息，请参阅[如何：确定查询返回的实体数](number-of-entities-returned-by-a-query-wcf.md)。

## <a name="in-this-section"></a>本节内容

- [查询投影](query-projections-wcf-data-services.md)

- [对象具体化](object-materialization-wcf-data-services.md)

- [LINQ 注意事项](linq-considerations-wcf-data-services.md)

- [如何：执行数据服务查询](how-to-execute-data-service-queries-wcf-data-services.md)

- [如何：将查询选项添加到数据服务查询](how-to-add-query-options-to-a-data-service-query-wcf-data-services.md)

- [如何：确定由查询返回的实体数](number-of-entities-returned-by-a-query-wcf.md)

- [如何：为数据服务请求指定客户端凭据](specify-client-creds-for-a-data-service-request-wcf.md)

- [如何：设置客户端请求中的标头](how-to-set-headers-in-the-client-request-wcf-data-services.md)

- [如何：投影查询结果](how-to-project-query-results-wcf-data-services.md)

## <a name="see-also"></a>另请参阅

- [WCF 数据服务客户端库](wcf-data-services-client-library.md)
