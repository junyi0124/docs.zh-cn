---
title: 编译的查询 (LINQ to Entities)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 8025ba1d-29c7-4407-841b-d5a3bed40b7a
ms.openlocfilehash: b8bed63cda505ad8c26c9c69d880a077053b8d2e
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91153048"
---
# <a name="compiled-queries--linq-to-entities"></a>已编译查询 (LINQ to Entities) 

如果应用程序需要在实体框架中多次执行结构类似的查询，通常可以通过仅编译查询一次并在每次执行时使用不同参数的方法来提高性能。 例如，某应用程序要检索特定城市的所有客户，而该城市是运行时由用户在窗体中指定的。 LINQ to Entities 支持将已编译的查询用于此目的。  
  
 从 .NET Framework 4.5 开始，将自动缓存 LINQ 查询。 但是，您仍可以使用已编译的 LINQ 查询来降低后续执行中的这一开销，编译的查询比自动缓存的 LINQ 查询效率更高。 `Enumerable.Contains`不自动缓存将运算符应用于内存中集合的 LINQ to Entities 查询。 此外，不允许在已编译的 LINQ 查询中参数化内存中集合。  
  
 <xref:System.Data.Objects.CompiledQuery> 类提供查询的编译和缓存以供重复使用。 从概念上讲，此类包含 <xref:System.Data.Objects.CompiledQuery> 的 `Compile` 方法以及若干重载。 调用 `Compile` 方法可以创建新的委托来表示已编译的查询。 给定 `Compile` 及其参数值，<xref:System.Data.Objects.ObjectContext> 方法将返回生成某个结果的委托（例如 <xref:System.Linq.IQueryable%601> 实例）。 只在第一次执行的过程中查询才编译一次。 编译时为查询设置的合并选项在以后无法更改。 编译查询后，只能提供基元类型的参数，但不能替换将更改生成的 SQL 的查询部分。 有关详细信息，请参阅 [EF 合并选项和已编译的查询](/archive/blogs/dsimmons/ef-merge-options-and-compiled-queries)。
  
 用于编译的方法的 LINQ to Entities 查询表达式由 <xref:System.Data.Objects.CompiledQuery> `Compile` 一个泛型委托（如）表示 `Func` <xref:System.Func%605> 。 查询表达式最多可以包装一个 `ObjectContext` 参数、一个返回参数和 16 个查询参数。 如果需要的查询参数不止 16 个，则可以创建一个结构并用其属性表示查询参数。 然后，该结构上的这些属性在经过设置之后就可以用于查询表达式中。  
  
## <a name="example-1"></a>示例 1  

 下面的示例将编译并调用一个查询，该查询接受 <xref:System.Decimal> 输入参数，并返回一个应付款总额大于或等于 200.00 美元的订单序列：  
  
 [!code-csharp[DP L2E Conceptual Examples - compiled query 2](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#compiledquery2)]
 [!code-vb[DP L2E Conceptual Examples - compiled query2](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#compiledquery2)]  
  
## <a name="example-2"></a>示例 2  

 下面的示例将编译并调用一个返回 <xref:System.Data.Objects.ObjectQuery%601> 实例的查询：  
  
 [!code-csharp[DP L2E Conceptual Examples - compiled query1_MQ](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#compiledquery1_mq)]
 [!code-vb[DP L2E Conceptual Examples - compiled query1_MQ](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#compiledquery1_mq)]  
  
## <a name="example-3"></a>示例 3  

 下面的示例将编译并调用一个返回 <xref:System.Decimal> 形式产品标价平均值的查询：  
  
 [!code-csharp[DP L2E Conceptual Examples - compiled query3_MQ](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#compiledquery3_mq)]
 [!code-vb[DP L2E Conceptual Examples - compiled query3_MQ](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#compiledquery3_mq)]  
  
## <a name="example-4"></a>示例 4  

 下面的示例将编译并调用一个查询，该查询接受 <xref:System.String> 输入参数，然后返回一个 `Contact` 其电子邮件地址以指定的字符串开头的：  
  
 [!code-csharp[DP L2E Conceptual Examples - compiled query4_MQ](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#compiledquery4_mq)]
 [!code-vb[DP L2E Conceptual Examples - compiled query4_MQ](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#compiledquery4_mq)]  
  
## <a name="example-5"></a>示例 5  

 下面的示例将编译并调用一个查询，该查询接受 <xref:System.DateTime> 和 <xref:System.Decimal> 输入参数，并返回一个订单日期晚于 2003 年 3 月 8 日且应付款总额少于 300.00 美元的订单序列：  
  
 [!code-csharp[DP L2E Conceptual Examples - compiled query5](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#compiledquery5)]
 [!code-vb[DP L2E Conceptual Examples - compiled query5](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#compiledquery5)]  
  
## <a name="example-6"></a>示例 6  

 下面的示例将编译并调用一个查询，该查询接受 <xref:System.DateTime> 输入参数，并返回一个订单日期迟于 2004 年 3 月 8 日的订单序列： 此查询将订单信息返回为匿名类型序列。 匿名类型由编译器推断，因此不能在 <xref:System.Data.Objects.CompiledQuery> 的 `Compile` 方法中指定类型参数，该类型只能在查询本身中定义。  
  
 [!code-csharp[DP L2E Conceptual Examples - compiled query6](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#compiledquery6)]
 [!code-vb[DP L2E Conceptual Examples - compiled query6](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#compiledquery6)]  
  
## <a name="example-7"></a>示例 7  

 下面的示例编译并调用一个查询，该查询接受用户定义的结构输入参数并返回一个订单序列。 该结构定义开始日期、结束日期和应付款总额查询参数，该查询返回在 2003 年 3 月 3 日到 3 月 8 日之间发货的应付款总额超过 700.00 美元的订单。  
  
 [!code-csharp[DP L2E Conceptual Examples - compiled query7](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#compiledquery7)]
 [!code-vb[DP L2E Conceptual Examples - compiled query7](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#compiledquery7)]  
  
 定义查询参数的结构：  
  
 [!code-csharp[DP L2E Conceptual Examples - MyParamsStruct](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#myparamsstruct)]
 [!code-vb[DP L2E Conceptual Examples - MyParamsStruct](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#myparamsstruct)]  
  
## <a name="see-also"></a>请参阅

- [ADO.NET 实体框架](../index.md)
- [LINQ to Entities](linq-to-entities.md)
- [EF 合并选项和已编译的查询](/archive/blogs/dsimmons/ef-merge-options-and-compiled-queries)
