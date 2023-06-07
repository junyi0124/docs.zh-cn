---
title: 调用服务操作（WCF 数据服务）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 1767f3a7-29d2-4834-a763-7d169693fa8b
ms.openlocfilehash: ac1b28665dcaaa9f8c6ae6a6611757f6c4969adb
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91152840"
---
# <a name="calling-service-operations-wcf-data-services"></a>调用服务操作（WCF 数据服务）

OData) Open Data Protocol (定义数据服务的服务操作。 WCF 数据服务使你能够将此类操作定义为数据服务中的方法。 与其他数据服务资源一样，这些服务操作也通过 URI 进行寻址。 服务操作可以返回实体类型的集合、单个实体类型实例的集合和基元类型（如整数和字符串）的集合。 服务操作还可以返回 `null`（在 Visual Basic 中为 `Nothing`）。 WCF 数据服务客户端库可用于访问支持 HTTP GET 请求的服务操作。 这些种类的服务操作定义为应用了 <xref:System.ServiceModel.Web.WebGetAttribute> 的方法。 有关详细信息，请参阅 [服务操作](service-operations-wcf-data-services.md)。  
  
 服务操作在实现 OData 的数据服务所返回的元数据中公开。 在元数据中，服务操作表示为 `FunctionImport` 元素。 生成强类型时 <xref:System.Data.Services.Client.DataServiceContext> ，添加服务引用和 DataSvcUtil.exe 工具将忽略此元素。 因此，无法在上下文中找到一种方法用来直接调用服务操作。 不过，仍可使用 WCF 数据服务客户端通过以下两种方式之一来调用服务操作：  
  
- 通过调用 <xref:System.Data.Services.Client.DataServiceContext.Execute%2A> 的 <xref:System.Data.Services.Client.DataServiceContext> 方法，同时提供服务操作的 URI 以及任何参数。 此方法用于调用任意 GET 服务操作。  
  
- 通过对 <xref:System.Data.Services.Client.DataServiceContext.CreateQuery%2A> 使用 <xref:System.Data.Services.Client.DataServiceContext> 方法来创建 <xref:System.Data.Services.Client.DataServiceQuery%601> 对象。 在调用 <xref:System.Data.Services.Client.DataServiceContext.CreateQuery%2A> 时，该服务操作的名称将提供给 `entitySetName` 参数。 此方法返回一个 <xref:System.Data.Services.Client.DataServiceQuery%601> 对象，该对象在枚举时或在调用 <xref:System.Data.Services.Client.DataServiceQuery%601.Execute%2A> 方法时调用该服务操作。 此方法用于调用返回集合的 GET 服务操作。 可通过使用 <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> 方法来提供单个参数。 可以进一步编辑此方法返回的 <xref:System.Data.Services.Client.DataServiceQuery%601> 对象，就像对待任何查询对象一样。 有关详细信息，请参阅 [查询数据服务](querying-the-data-service-wcf-data-services.md)。  
  
## <a name="considerations-for-calling-service-operations"></a>调用服务操作的注意事项  

 使用 WCF 数据服务客户端调用服务操作时，请注意以下事项。  
  
- 异步访问数据服务时，必须在 <xref:System.Data.Services.Client.DataServiceContext.BeginExecute%2A> / <xref:System.Data.Services.Client.DataServiceContext.EndExecute%2A> 上 <xref:System.Data.Services.Client.DataServiceContext> 或 <xref:System.Data.Services.Client.DataServiceQuery%601.BeginExecute%2A> / <xref:System.Data.Services.Client.DataServiceQuery%601.EndExecute%2A> 方法上 <xref:System.Data.Services.Client.DataServiceQuery%601> 使用等效的异步方法。  
  
- WCF 数据服务客户端库无法具体化返回基元类型集合的服务操作的结果。  
  
- WCF 数据服务客户端库不支持调用后服务操作。 服务操作由 HTTP POST 调用，后者通过使用 <xref:System.ServiceModel.Web.WebInvokeAttribute> 配合 `Method="POST"` 参数定义。 要通过使用 HTTP POST 请求调用服务操作，必须使用 <xref:System.Net.HttpWebRequest>。  
  
- 您不能使用 <xref:System.Data.Services.Client.DataServiceContext.CreateQuery%2A> 调用这样的 GET 服务操作：它返回实体或基元类型的单个结果，或需要多个输入参数。 必须调用 <xref:System.Data.Services.Client.DataServiceContext.Execute%2A> 方法。  
  
- 请考虑在强类型化分部类（由工具生成）上创建扩展方法，该 <xref:System.Data.Services.Client.DataServiceContext> 分部类使用 <xref:System.Data.Services.Client.DataServiceContext.CreateQuery%2A> 或 <xref:System.Data.Services.Client.DataServiceContext.Execute%2A> 方法来调用服务操作。 这样您就可以直接从上下文调用服务操作。 有关详细信息，请参阅博客文章 [服务操作和 WCF 数据服务客户端](/archive/blogs/astoriateam/service-operations-and-the-wcf-data-services-client)。  
  
- 使用 <xref:System.Data.Services.Client.DataServiceContext.CreateQuery%2A> 调用服务操作时，客户端库会 <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> 通过对保留字符执行百分号编码（如与号 ( # A0) ），并对字符串中的单引号进行转义来自动转义提供给的字符。 但是，当您调用某一 *Execute* 方法来调用服务操作时，您必须记得对任何用户提供的字符串值执行这种转义。 URI 中的单引号转义为单引号对。  
  
## <a name="examples-of-calling-service-operations"></a>调用服务操作示例  

 本部分包含以下示例，说明如何使用 WCF 数据服务客户端库调用服务操作：  
  
- [调用 Execute &lt; T &gt; 以返回实体集合](calling-service-operations-wcf-data-services.md#ExecuteIQueryable)  
  
- [使用 Servicecontext.createquery &lt; T &gt; 返回实体集合](calling-service-operations-wcf-data-services.md#CreateQueryIQueryable)  
  
- [调用 Execute &lt; T &gt; 以返回单个实体](calling-service-operations-wcf-data-services.md#ExecuteSingleEntity)  
  
- [调用 Execute &lt; T &gt; 以返回基元值集合](calling-service-operations-wcf-data-services.md#ExecutePrimitiveCollection)  
  
- [调用 Execute &lt; T &gt; 以返回单个基元值](calling-service-operations-wcf-data-services.md#ExecutePrimitiveValue)  
  
- [调用不返回数据的服务操作](calling-service-operations-wcf-data-services.md#ExecuteVoid)  
  
- [异步调用服务操作](calling-service-operations-wcf-data-services.md#ExecuteAsync)  
  
<a name="ExecuteIQueryable"></a>

### <a name="calling-executet-to-return-a-collection-of-entities"></a>调用 Execute \<T> 以返回实体集合  

 下面的示例调用名为 GetOrdersByCity 的服务操作，该操作采用字符串参数 `city`，并返回 <xref:System.Linq.IQueryable%601>：  
  
 [!code-csharp[Astoria Northwind Client#CallServiceOperationIQueryable](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#callserviceoperationiqueryable)]
 [!code-vb[Astoria Northwind Client#CallServiceOperationIQueryable](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#callserviceoperationiqueryable)]  
  
 在此示例中，该服务操作返回 `Order` 对象的集合，其中含有相关的 `Order_Detail` 对象。  
  
<a name="CreateQueryIQueryable"></a>

### <a name="using-createqueryt-to-return-a-collection-of-entities"></a>使用 Servicecontext.createquery \<T> 返回实体集合  

 下面的示例使用 <xref:System.Data.Services.Client.DataServiceContext.CreateQuery%2A> 来返回用于调用同一 GetOrdersByCity 服务操作的 <xref:System.Data.Services.Client.DataServiceQuery%601>：  
  
 [!code-csharp[Astoria Northwind Client#CallServiceOperationCreateQuery](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#callserviceoperationcreatequery)]
 [!code-vb[Astoria Northwind Client#CallServiceOperationCreateQuery](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#callserviceoperationcreatequery)]  
  
 在此示例中，<xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> 方法用于向查询中添加参数，而 <xref:System.Data.Services.Client.DataServiceQuery%601.Expand%2A> 方法用于在结果中包括相关 Order_Details 对象。  
  
<a name="ExecuteSingleEntity"></a>

### <a name="calling-executet-to-return-a-single-entity"></a>调用 Execute \<T> 以返回单个实体  

 下面的示例调用名为 GetNewestOrder 的服务操作，该操作仅返回单个 Order 实体：  
  
 [!code-csharp[Astoria Northwind Client#CallServiceOperationSingleEntity](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#callserviceoperationsingleentity)]
 [!code-vb[Astoria Northwind Client#CallServiceOperationSingleEntity](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#callserviceoperationsingleentity)]  
  
 在此示例中，<xref:System.Linq.Enumerable.FirstOrDefault%2A> 方法用于在执行时仅请求单个 Order 实体。  
  
<a name="ExecutePrimitiveCollection"></a>

### <a name="calling-executet-to-return-a-collection-of-primitive-values"></a>调用 Execute \<T> 以返回基元值集合  

 下面的示例调用返回字符串值集合的服务操作：  
  
 [!code-csharp[Astoria Northwind Client#CallServiceOperationEnumString](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#callserviceoperationenumstring)]  
  
<a name="ExecutePrimitiveValue"></a>

### <a name="calling-executet-to-return-a-single-primitive-value"></a>调用 Execute \<T> 以返回单个基元值  

 下面的示例调用返回单个字符串值的服务操作：  
  
 [!code-csharp[Astoria Northwind Client#CallServiceOperationSingleInt](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#callserviceoperationsingleint)]
 [!code-vb[Astoria Northwind Client#CallServiceOperationSingleInt](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#callserviceoperationsingleint)]  
  
 同样，在此示例中，<xref:System.Linq.Enumerable.FirstOrDefault%2A> 方法用于在执行时仅请求单个整数值。  
  
<a name="ExecuteVoid"></a>

### <a name="calling-a-service-operation-that-returns-no-data"></a>调用不返回数据的服务操作  

 下面的示例调用不返回任何数据的服务操作：  
  
 [!code-csharp[Astoria Northwind Client#CallServiceOperationVoid](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#callserviceoperationvoid)]
 [!code-vb[Astoria Northwind Client#CallServiceOperationVoid](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#callserviceoperationvoid)]  
  
 由于不返回数据，因此不分配执行的值。 请求已成功的唯一指示是未引发 <xref:System.Data.Services.Client.DataServiceQueryException>。  
  
<a name="ExecuteAsync"></a>

### <a name="calling-a-service-operation-asynchronously"></a>异步调用服务操作  

 下面的示例通过调用 <xref:System.Data.Services.Client.DataServiceContext.BeginExecute%2A> 和 <xref:System.Data.Services.Client.DataServiceContext.EndExecute%2A> 来异步调用服务操作：  
  
 [!code-csharp[Astoria Northwind Client#CallServiceOperationAsync](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#callserviceoperationasync)]
 [!code-vb[Astoria Northwind Client#CallServiceOperationAsync](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#callserviceoperationasync)]  
  
 [!code-csharp[Astoria Northwind Client#OnAsyncExecutionComplete](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#onasyncexecutioncomplete)]
 [!code-vb[Astoria Northwind Client#OnAsyncExecutionComplete](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#onasyncexecutioncomplete)]  
  
 由于不返回数据，因此不分配由该执行返回的值。 请求已成功的唯一指示是未引发 <xref:System.Data.Services.Client.DataServiceQueryException>。  
  
 下面的示例通过使用 <xref:System.Data.Services.Client.DataServiceContext.CreateQuery%2A> 来异步调用同一服务操作：  
  
 [!code-csharp[Astoria Northwind Client#CallServiceOperationQueryAsync](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#callserviceoperationqueryasync)]
 [!code-vb[Astoria Northwind Client#CallServiceOperationQueryAsync](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#callserviceoperationqueryasync)]  
  
 [!code-csharp[Astoria Northwind Client#OnAsyncQueryExecutionComplete](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#onasyncqueryexecutioncomplete)]
 [!code-vb[Astoria Northwind Client#OnAsyncQueryExecutionComplete](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#onasyncqueryexecutioncomplete)]  
  
## <a name="see-also"></a>请参阅

- [WCF 数据服务客户端库](wcf-data-services-client-library.md)
