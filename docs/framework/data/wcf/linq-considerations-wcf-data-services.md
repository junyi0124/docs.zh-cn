---
title: LINQ 注意事项（WCF 数据服务）
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, LINQ
- querying the data service [WCF Data Services]
- WCF Data Services, querying
ms.assetid: cc4ec9e9-348f-42a6-a78e-1cd40e370656
ms.openlocfilehash: 2523aac510516fdf19087425b10ab3f2296eb726
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91194333"
---
# <a name="linq-considerations-wcf-data-services"></a>LINQ 注意事项（WCF 数据服务）

本主题提供有关在使用 WCF 数据服务客户端时如何编写和执行 LINQ 查询的信息，以及使用 LINQ 查询实现 Open Data Protocol (OData) 的数据服务的限制。 有关对基于 OData 的数据服务编写和执行查询的详细信息，请参阅 [查询数据服务](querying-the-data-service-wcf-data-services.md)。  
  
## <a name="composing-linq-queries"></a>编写 LINQ 查询  

 LINQ 允许针对实现 <xref:System.Collections.Generic.IEnumerable%601> 的对象集合编写查询。 Visual Studio 中的 " **添加服务引用** " 对话框和 "DataSvcUtil.exe" 工具都用于生成 OData 服务的表示形式，作为从继承的实体容器类以及 <xref:System.Data.Services.Client.DataServiceContext> 表示源中返回的实体的对象。 这些工具还可为由服务作为源公开的集合的实体容器类生成属性。 封装了数据服务的类的每个属性都会返回一个 <xref:System.Data.Services.Client.DataServiceQuery%601>。 因为 <xref:System.Data.Services.Client.DataServiceQuery%601> 类实现 LINQ 定义的 <xref:System.Linq.IQueryable%601> 接口，所以可以针对数据服务公开的源编写 LINQ 查询，客户端库会将其转换为一个查询请求 URI，在执行时将此 URI 发送给数据服务。  
  
> [!IMPORTANT]
> LINQ 语法中可表达的查询集比 OData 数据服务使用的 URI 语法中启用的查询更广泛。 如果无法将查询映射到目标数据服务中的 URI，则会引发 <xref:System.NotSupportedException>。 有关详细信息，请参阅本主题中的 [不受支持的 LINQ 方法](linq-considerations-wcf-data-services.md#unsupportedMethods) 。  
  
 下面的示例是一个 LINQ 查询，它返回运费成本超过 30 美元的 `Orders` 并按装运日期对结果进行排序，最近的装运日期排在最前面：  
  
[!code-csharp[Astoria Northwind Client#AddQueryOptionsLinqSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addqueryoptionslinqspecific)]
[!code-vb[Astoria Northwind Client#AddQueryOptionsLinqSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addqueryoptionslinqspecific)]
  
 此 LINQ 查询转换为对基于 Northwind 的 [快速入门](quickstart-wcf-data-services.md) 数据服务执行的以下查询 URI：  
  
```http
http://localhost:12345/Northwind.svc/Orders?Orderby=ShippedDate&?filter=Freight gt 30  
```  
  
 有关 LINQ 的更多常规信息，请参阅 [语言集成查询 (linq) c #](../../../csharp/programming-guide/concepts/linq/index.md) 或 [语言集成查询 (LINQ) -Visual Basic](../../../visual-basic/programming-guide/concepts/linq/index.md)。  
  
 LINQ 允许使用语言特定声明查询语法（如上例所示）和一组称为标准查询运算符的查询方法编写查询。 通过仅使用基于方法的语法可以编写与上例等效的查询，如下面的示例所示：  
  
[!code-csharp[Astoria Northwind Client#AddQueryOptionsLinqExpressionSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#addqueryoptionslinqexpressionspecific)]
[!code-vb[Astoria Northwind Client#AddQueryOptionsLinqExpressionSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#addqueryoptionslinqexpressionspecific)]
  
 WCF 数据服务客户端可以将这两种组合查询转换为查询 URI，并且可以通过将查询方法追加到查询表达式来扩展 LINQ 查询。 通过向查询表达式或 <xref:System.Data.Services.Client.DataServiceQuery%601> 追加方法语法来编写 LINQ 查询时，运算将按照方法的调用顺序添加到查询 URI 中。 这等效于调用 <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> 方法将每个查询选项添加到查询 URI 中。  
  
## <a name="executing-linq-queries"></a>执行 LINQ 查询  

 某些 LINQ 查询方法（如 <xref:System.Linq.Enumerable.First%60%601%28System.Collections.Generic.IEnumerable%7B%60%600%7D%29> 或 <xref:System.Linq.Enumerable.Single%60%601%28System.Collections.Generic.IEnumerable%7B%60%600%7D%29>）在追加到查询中时会导致执行查询。 在隐式枚举结果时也会执行查询，例如在 `foreach` 循环过程中或将查询分配给一个 `List` 集合时。 有关详细信息，请参阅 [查询数据服务](querying-the-data-service-wcf-data-services.md)。  
  
 客户端分两个部分来执行 LINQ 查询。 只要有可能，将首先在客户端上计算查询中的 LINQ 表达式，然后生成基于 URI 的查询并将其发送至数据服务，以针对服务中的数据进行计算。 有关详细信息，请参阅[查询数据服务](querying-the-data-service-wcf-data-services.md)中的[客户端与服务器执行](querying-the-data-service-wcf-data-services.md#executingQueries)的部分。  
  
 如果 LINQ 查询无法在符合 OData 的查询 URI 中进行转换，则在尝试执行时将引发异常。 有关详细信息，请参阅 [查询数据服务](querying-the-data-service-wcf-data-services.md)。  
  
## <a name="linq-query-examples"></a>LINQ 查询示例  

 以下部分中的示例演示了可以对 OData 服务执行的 LINQ 查询的类型。  
  
<a name="filtering"></a>

### <a name="filtering"></a>Filtering  

 本小节中的 LINQ 查询示例将筛选由服务返回的源中的数据。  
  
 下面这些示例是等效的查询，它们将筛选返回的 `Orders` 实体并仅返回运费成本高于 30 美元的订单：  
  
- 使用 LINQ 查询语法：  
  
[!code-csharp[Astoria Northwind Client#LinqWhereClauseSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqwhereclausespecific)]
[!code-vb[Astoria Northwind Client#LinqWhereClauseSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqwhereclausespecific)]
  
- 使用 LINQ 查询方法：  
  
[!code-csharp[Astoria Northwind Client#LinqWhereMethodSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqwheremethodspecific)]
[!code-vb[Astoria Northwind Client#LinqWhereMethodSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqwheremethodspecific)]
  
- URI 查询字符串 `$filter` 选项：  
  
[!code-csharp[Astoria Northwind Client#ExplicitQueryWhereMethodSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#explicitquerywheremethodspecific)]
[!code-vb[Astoria Northwind Client#ExplicitQueryWhereMethodSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#explicitquerywheremethodspecific)]
  
 前面的所有示例都将转换为查询 URI：`http://localhost:12345/northwind.svc/Orders()?$filter=Freight gt 30M`。  
  
<a name="sorting"></a>

### <a name="sorting"></a>排序  

 下面的示例显示了按公司名称和邮政编码降序排序返回的数据的等效查询：  
  
- 使用 LINQ 查询语法：  
  
[!code-csharp[Astoria Northwind Client#LinqOrderByClauseSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqorderbyclausespecific)]
[!code-vb[Astoria Northwind Client#LinqOrderByClauseSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqorderbyclausespecific)]
  
- 使用 LINQ 查询方法：  
  
[!code-csharp[Astoria Northwind Client#LinqOrderByMethodSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqorderbymethodspecific)]
[!code-vb[Astoria Northwind Client#LinqOrderByMethodSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqorderbymethodspecific)]
  
- URI 查询字符串 `$orderby` 选项：  
  
[!code-csharp[Astoria Northwind Client#ExplicitQueryOrderByMethodSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#explicitqueryorderbymethodspecific)]
[!code-vb[Astoria Northwind Client#ExplicitQueryOrderByMethodSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#explicitqueryorderbymethodspecific)]
  
 前面的所有示例都将转换为查询 URI：`http://localhost:12345/northwind.svc/Customers()?$orderby=CompanyName,PostalCode desc`。  
  
<a name="projection"></a>

### <a name="projection"></a>投影  

 下面的示例显示了将返回的数据投影到较窄的 `CustomerAddress` 类型的等效查询：  
  
- 使用 LINQ 查询语法：  
  
[!code-csharp[Astoria Northwind Client#LinqSelectClauseSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqselectclausespecific)]
[!code-vb[Astoria Northwind Client#LinqSelectClauseSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqselectclausespecific)]
  
- 使用 LINQ 查询方法：  
  
[!code-csharp[Astoria Northwind Client#LinqSelectMethodSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqselectmethodspecific)]
[!code-vb[Astoria Northwind Client#LinqSelectMethodSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqselectmethodspecific)]

> [!NOTE]
> 不能使用 `$select` 方法将 <xref:System.Data.Services.Client.DataServiceQuery%601.AddQueryOption%2A> 查询选项添加到查询 URI 中。 我们建议使用 LINQ <xref:System.Linq.Enumerable.Select%2A> 方法，让客户端在请求 URI 中生成 `$select` 查询选项。  
  
 前面的两个示例都将转换为查询 URI：`"http://localhost:12345/northwind.svc/Customers()?$filter=Country eq 'GerGerm'&$select=CustomerID,Address,City,Region,PostalCode,Country"`。  
  
<a name="paging"></a>

### <a name="client-paging"></a>客户端分页  

 下面所示的等效查询示例将请求返回一页包含 25 个订单的排序订单实体，并跳过前 50 个订单：  
  
- 为 LINQ 查询应用查询方法：  
  
[!code-csharp[Astoria Northwind Client#LinqSkipTakeMethodSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqskiptakemethodspecific)]
[!code-vb[Astoria Northwind Client#LinqSkipTakeMethodSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqskiptakemethodspecific)]
  
- URI 查询字符串 `$skip` 和 `$top` 选项：  
  
[!code-csharp[Astoria Northwind Client#ExplicitQuerySkipTakeMethodSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#explicitqueryskiptakemethodspecific)]
[!code-vb[Astoria Northwind Client#ExplicitQuerySkipTakeMethodSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#explicitqueryskiptakemethodspecific)]
  
 前面的两个示例都将转换为查询 URI：`http://localhost:12345/northwind.svc/Orders()?$orderby=OrderDate desc&$skip=50&$top=25`。  
  
<a name="expand"></a>

### <a name="expand"></a>展开  

 查询 OData 数据服务时，您可以请求将与查询针对的实体相关的实体包含在返回的源中。 针对 LINQ 查询的目标实体集调用 <xref:System.Data.Services.Client.DataServiceQuery%601.Expand%2A> 的 <xref:System.Data.Services.Client.DataServiceQuery%601> 方法，相关实体集名称作为 `path` 参数提供。 有关详细信息，请参阅 [加载延迟的内容](loading-deferred-content-wcf-data-services.md)。  
  
 下面的示例显示了在查询中使用 <xref:System.Data.Services.Client.DataServiceQuery%601.Expand%2A> 方法的等效方式：  
  
- 在 LINQ 查询语法中：  
  
[!code-csharp[Astoria Northwind Client#LinqQueryExpandSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqqueryexpandspecific)]
[!code-vb[Astoria Northwind Client#LinqQueryExpandSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqqueryexpandspecific)]  
  
- 使用 LINQ 查询方法：  

[!code-csharp[Astoria Northwind Client#LinqQueryExpandMethodSpecific](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/source.cs#linqqueryexpandmethodspecific)]
[!code-vb[Astoria Northwind Client#LinqQueryExpandMethodSpecific](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/source.vb#linqqueryexpandmethodspecific)]

 前面的两个示例都将转换为查询 URI：`http://localhost:12345/northwind.svc/Orders()?$filter=CustomerID eq 'ALFKI'&$expand=Order_Details`。  
  
<a name="unsupportedMethods"></a>

## <a name="unsupported-linq-methods"></a>不支持的 LINQ 方法  

 下表包含不支持的 LINQ 方法的类，并且不能包含在针对 OData 服务执行的查询中：  
  
|操作类型|不支持的方法|  
|--------------------|------------------------|  
|集合运算符|所有集合运算符都不能用于 <xref:System.Data.Services.Client.DataServiceQuery%601>，其中包括：<br /><br /> -   <xref:System.Linq.Enumerable.All%2A><br />-   <xref:System.Linq.Enumerable.Any%2A><br />-   <xref:System.Linq.Enumerable.Concat%2A><br />-   <xref:System.Linq.Enumerable.DefaultIfEmpty%2A><br />-   <xref:System.Linq.Enumerable.Distinct%2A><br />-   <xref:System.Linq.Enumerable.Except%2A><br />-   <xref:System.Linq.Enumerable.Intersect%2A><br />-   <xref:System.Linq.Enumerable.Union%2A><br />-   <xref:System.Linq.Enumerable.Zip%2A>|  
|排序运算符|要求 <xref:System.Collections.Generic.IComparer%601> 的以下排序运算符不能用于 <xref:System.Data.Services.Client.DataServiceQuery%601>：<br /><br /> -   <xref:System.Linq.Enumerable.OrderBy%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2C%60%601%7D%2CSystem.Collections.Generic.IComparer%7B%60%601%7D%29><br />-   <xref:System.Linq.Enumerable.OrderByDescending%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2C%60%601%7D%2CSystem.Collections.Generic.IComparer%7B%60%601%7D%29><br />-   <xref:System.Linq.Enumerable.ThenBy%60%602%28System.Linq.IOrderedEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2C%60%601%7D%2CSystem.Collections.Generic.IComparer%7B%60%601%7D%29><br />-   <xref:System.Linq.Enumerable.ThenByDescending%60%602%28System.Linq.IOrderedEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2C%60%601%7D%2CSystem.Collections.Generic.IComparer%7B%60%601%7D%29>|  
|投影和筛选运算符|以下接受位置自变量的投影和筛选运算符不能用于 <xref:System.Data.Services.Client.DataServiceQuery%601>：<br /><br /> -   <xref:System.Linq.Enumerable.Join%60%604%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Collections.Generic.IEnumerable%7B%60%601%7D%2CSystem.Func%7B%60%600%2C%60%602%7D%2CSystem.Func%7B%60%601%2C%60%602%7D%2CSystem.Func%7B%60%600%2C%60%601%2C%60%603%7D%2CSystem.Collections.Generic.IEqualityComparer%7B%60%602%7D%29><br />-   <xref:System.Linq.Enumerable.Select%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2CSystem.Int32%2C%60%601%7D%29><br />-   <xref:System.Linq.Enumerable.SelectMany%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2CSystem.Collections.Generic.IEnumerable%7B%60%601%7D%7D%29><br />-   <xref:System.Linq.Enumerable.SelectMany%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2CSystem.Int32%2CSystem.Collections.Generic.IEnumerable%7B%60%601%7D%7D%29><br />-   <xref:System.Linq.Enumerable.SelectMany%60%603%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2CSystem.Collections.Generic.IEnumerable%7B%60%601%7D%7D%2CSystem.Func%7B%60%600%2C%60%601%2C%60%602%7D%29><br />-   <xref:System.Linq.Enumerable.SelectMany%60%603%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2CSystem.Int32%2CSystem.Collections.Generic.IEnumerable%7B%60%601%7D%7D%2CSystem.Func%7B%60%600%2C%60%601%2C%60%602%7D%29><br />-   <xref:System.Linq.Enumerable.Where%60%601%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2CSystem.Int32%2CSystem.Boolean%7D%29>|  
|分组运算符|所有分组运算符都不能用于 <xref:System.Data.Services.Client.DataServiceQuery%601>，其中包括：<br /><br /> -   <xref:System.Linq.Enumerable.GroupBy%2A><br />-   <xref:System.Linq.Enumerable.GroupJoin%2A><br /><br /> 分组运算符必须在客户端上执行。|  
|聚合运算符|所有聚合运算符都不能用于 <xref:System.Data.Services.Client.DataServiceQuery%601>，其中包括：<br /><br /> -   <xref:System.Linq.Enumerable.Aggregate%2A><br />-   <xref:System.Linq.Enumerable.Average%2A><br />-   <xref:System.Linq.Enumerable.Count%2A><br />-   <xref:System.Linq.Enumerable.LongCount%2A><br />-   <xref:System.Linq.Enumerable.Max%2A><br />-   <xref:System.Linq.Enumerable.Min%2A><br />-   <xref:System.Linq.Enumerable.Sum%2A><br /><br /> 聚合运算符必须在客户端上执行或由服务操作封装。|  
|分页运算符|以下分页运算符不能用于 <xref:System.Data.Services.Client.DataServiceQuery%601>：<br /><br /> -   <xref:System.Linq.Enumerable.ElementAt%2A><br />-   <xref:System.Linq.Enumerable.Last%2A><br />-   <xref:System.Linq.Enumerable.LastOrDefault%2A><br />-   <xref:System.Linq.Enumerable.SkipWhile%2A><br />-   <xref:System.Linq.Enumerable.TakeWhile%2A><br/><br/>**注意：**  对空序列执行的分页运算符将返回 null。|  
|其他运算符|还不支持以下运算符 <xref:System.Data.Services.Client.DataServiceQuery%601> ：<br /><br /> - <xref:System.Linq.Enumerable.Empty%2A><br />- <xref:System.Linq.Enumerable.Range%2A><br />- <xref:System.Linq.Enumerable.Repeat%2A><br />- <xref:System.Linq.Enumerable.ToDictionary%2A><br />- <xref:System.Linq.Enumerable.ToLookup%2A>|  
  
<a name="supportedExpressions"></a>

## <a name="supported-expression-functions"></a>支持的表达式函数  

 支持以下公共语言运行时 (CLR) 方法和属性，因为这些方法和属性可以在查询表达式中进行转换，以便将其包含在请求 URI 中以包含在 OData 服务中：  
  
|<xref:System.String> 成员|支持的 OData 函数|  
|-----------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|  
|<xref:System.String.Concat%28System.String%2CSystem.String%29>|`string concat(string p0, string p1)`|  
|<xref:System.String.Contains%28System.String%29>|`bool substringof(string p0, string p1)`|  
|<xref:System.String.EndsWith%28System.String%29>|`bool endswith(string p0, string p1)`|  
|<xref:System.String.IndexOf%28System.String%2CSystem.Int32%29>|`int indexof(string p0, string p1)`|  
|<xref:System.String.Length>|`int length(string p0)`|  
|<xref:System.String.Replace%28System.String%2CSystem.String%29>|`string replace(string p0, string find, string replace)`|  
|<xref:System.String.Substring%28System.Int32%29>|`string substring(string p0, int pos)`|  
|<xref:System.String.Substring%28System.Int32%2CSystem.Int32%29>|`string substring(string p0, int pos, int length)`|  
|<xref:System.String.ToLower>|`string tolower(string p0)`|  
|<xref:System.String.ToUpper>|`string toupper(string p0)`|  
|<xref:System.String.Trim>|`string trim(string p0)`|  
  
|<xref:System.DateTime> 成员<sup>1</sup>|支持的 OData 函数|  
|-------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|  
|<xref:System.DateTime.Day>|`int day(DateTime p0)`|  
|<xref:System.DateTime.Hour>|`int hour(DateTime p0)`|  
|<xref:System.DateTime.Minute>|`int minute(DateTime p0)`|  
|<xref:System.DateTime.Month>|`int month(DateTime p0)`|  
|<xref:System.DateTime.Second>|`int second(DateTime p0)`|  
|<xref:System.DateTime.Year>|`int year(DateTime p0)`|  
  
 <sup>1</sup>和 Visual Basic 中的等效的日期和时间属性 <xref:Microsoft.VisualBasic.DateAndTime?displayProperty=nameWithType> <xref:Microsoft.VisualBasic.DateAndTime.DatePart%2A> 也受支持。  
  
|<xref:System.Math> 成员|支持的 OData 函数|  
|---------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|  
|<xref:System.Math.Ceiling%28System.Decimal%29>|`decimal ceiling(decimal p0)`|  
|<xref:System.Math.Ceiling%28System.Double%29>|`double ceiling(double p0)`|  
|<xref:System.Math.Floor%28System.Decimal%29>|`decimal floor(decimal p0)`|  
|<xref:System.Math.Floor%28System.Double%29>|`double floor(double p0)`|  
|<xref:System.Math.Round%28System.Decimal%29>|`decimal round(decimal p0)`|  
|<xref:System.Math.Round%28System.Double%29>|`double round(double p0)`|  
  
|<xref:System.Linq.Expressions.Expression> 成员|支持的 OData 函数|  
|---------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------|  
|<xref:System.Linq.Expressions.Expression.TypeIs%28System.Linq.Expressions.Expression%2CSystem.Type%29>|`bool isof(type p0)`|  
  
 客户端或许还可以在客户端上计算其他 CLR 函数。 对于无法在客户端上计算以及无法转换为有效请求 URI 以便在服务器上计算的任何表达式，将引发 <xref:System.NotSupportedException>。  
  
## <a name="see-also"></a>请参阅

- [查询数据服务](querying-the-data-service-wcf-data-services.md)
- [查询投影](query-projections-wcf-data-services.md)
- [对象具体化](object-materialization-wcf-data-services.md)
- [OData：URI 约定](https://www.odata.org/documentation/odata-version-2-0/uri-conventions/)
