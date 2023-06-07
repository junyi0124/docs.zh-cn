---
title: 使用 DataView 进行排序 (LINQ to DataSet)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 885b3b7b-51c1-42b3-bb29-b925f4f69a6f
ms.openlocfilehash: d80c00a4b06a31f61a521e7206c204c02106748a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91175273"
---
# <a name="sorting-with-dataview-linq-to-dataset"></a>使用 DataView 进行排序 (LINQ to DataSet)

基于特定条件对数据进行排序，然后通过 UI 控件向客户端表示该数据的能力是数据绑定的一个重要特征。 <xref:System.Data.DataView> 提供多种方式对数据进行排序并返回按指定排序条件排序的数据行。 除了基于字符串的排序功能以外， <xref:System.Data.DataView> 还使您能够将语言集成查询用于排序条件 (LINQ) 表达式。 LINQ 表达式允许执行比基于字符串的排序更复杂而功能更强大的排序操作。 本主题介绍这两种使用 <xref:System.Data.DataView> 的排序方法。  
  
## <a name="creating-dataview-from-a-query-with-sorting-information"></a>通过具有排序信息的查询创建 DataView  

 <xref:System.Data.DataView>可以通过 LINQ to DataSet 查询来创建对象。 如果查询包含 <xref:System.Linq.Enumerable.OrderBy%2A>、<xref:System.Linq.Enumerable.OrderByDescending%2A>、<xref:System.Linq.Enumerable.ThenBy%2A> 或 <xref:System.Linq.Enumerable.ThenByDescending%2A> 子句，则这些子句中的表达式将用作对 <xref:System.Data.DataView> 中的数据进行排序的基础。 例如，如果查询包含 `Order By…` 和 `Then By…` 子句，则生成的 <xref:System.Data.DataView> 将按指定的两个列对数据进行排序。  
  
 基于表达式的排序具有比基于字符串的简单排序更强大、更复杂的排序功能。 请注意，基于字符串和基于表达式的排序是互相排斥的。 如果在通过查询创建 <xref:System.Data.DataView.Sort%2A> 后设置基于字符串的 <xref:System.Data.DataView>，则会清除从查询推断的基于表达式的筛选，并且无法重置。  
  
 创建 <xref:System.Data.DataView> 及修改任何排序或筛选信息时，均会生成 <xref:System.Data.DataView> 的索引。 您可以通过在 LINQ to DataSet 查询中提供排序条件来获得最佳性能，该查询 <xref:System.Data.DataView> 是从创建的，而不是在以后修改排序信息。 有关详细信息，请参阅 [DataView 性能](dataview-performance.md)。  
  
> [!NOTE]
> 在大多数情况下，用于排序的表达式不应有副作用且必须是确定的。 另外，表达式不应包含依赖于固定执行次数的任何逻辑，因为排序操作可能会执行任意次。  
  
### <a name="example"></a>示例  

 下面的示例查询 SalesOrderHeader 表并按订单日期对返回的行排序，通过查询创建 <xref:System.Data.DataView>，并将 <xref:System.Data.DataView> 绑定到 <xref:System.Windows.Forms.BindingSource>。  
  
 [!code-csharp[DP DataView Samples#CreateLDVFromQueryOrderBy](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#createldvfromqueryorderby)]
 [!code-vb[DP DataView Samples#CreateLDVFromQueryOrderBy](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#createldvfromqueryorderby)]  
  
### <a name="example"></a>示例  

 下面的示例查询 SalesOrderHeader 表并按总应付金额对返回的行排序，通过查询创建 <xref:System.Data.DataView>，并将 <xref:System.Data.DataView> 绑定到 <xref:System.Windows.Forms.BindingSource>。  
  
 [!code-csharp[DP DataView Samples#CreateLDVFromQueryOrderBy2](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#createldvfromqueryorderby2)]
 [!code-vb[DP DataView Samples#CreateLDVFromQueryOrderBy2](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#createldvfromqueryorderby2)]  
  
### <a name="example"></a>示例  

 下面的示例查询 SalesOrderDetail 表并先按订单数量，然后按销售订单 ID 对返回的行排序，通过查询创建 <xref:System.Data.DataView>，并将 <xref:System.Data.DataView> 绑定到 <xref:System.Windows.Forms.BindingSource>。  
  
 [!code-csharp[DP DataView Samples#CreateLDVFromQueryOrderByThenBy](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#createldvfromqueryorderbythenby)]
 [!code-vb[DP DataView Samples#CreateLDVFromQueryOrderByThenBy](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#createldvfromqueryorderbythenby)]  
  
## <a name="using-the-string-based-sort-property"></a>使用基于字符串的排序属性  

 基于字符串的排序功能 <xref:System.Data.DataView> 仍适用于 LINQ to DataSet。 <xref:System.Data.DataView>从 LINQ to DataSet 查询创建后，可以使用 <xref:System.Data.DataView.Sort%2A> 属性来设置对的排序 <xref:System.Data.DataView> 。  
  
 基于字符串和基于表达式的排序功能是互相排斥的。 设置 <xref:System.Data.DataView.Sort%2A> 属性将清除从创建 <xref:System.Data.DataView> 的查询中继承的基于表达式的排序。  
  
 有关基于字符串的筛选的详细信息 <xref:System.Data.DataView.Sort%2A> ，请参阅对 [数据进行排序和筛选](./dataset-datatable-dataview/sorting-and-filtering-data.md)。  
  
### <a name="example"></a>示例  

 下面的示例从 Contact 表创建 <xref:System.Data.DataView> 并先按姓氏以降序，然后按名字以升序对行进行排序：  
  
 [!code-csharp[DP DataView Samples#LDVStringSort](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvstringsort)]
 [!code-vb[DP DataView Samples#LDVStringSort](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvstringsort)]  
  
### <a name="example"></a>示例  

 下面的示例查询 Contact 表中以字母“S”开头的姓氏。  通过该查询创建 <xref:System.Data.DataView> 并将其绑定到 <xref:System.Windows.Forms.BindingSource> 对象。  
  
 [!code-csharp[DP DataView Samples#CreateLDVFromQueryStringSort](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#createldvfromquerystringsort)]
 [!code-vb[DP DataView Samples#CreateLDVFromQueryStringSort](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#createldvfromquerystringsort)]  
  
## <a name="clearing-the-sort"></a>清除排序  

 有关 <xref:System.Data.DataView> 的排序信息可以在使用 <xref:System.Data.DataView.Sort%2A> 属性设置后清除。 清除 <xref:System.Data.DataView> 中的排序信息有两种方式：  
  
- 将 <xref:System.Data.DataView.Sort%2A> 属性设置为 `null`。  
  
- 将 <xref:System.Data.DataView.Sort%2A> 属性设置为一个空字符串。  
  
### <a name="example"></a>示例  

 下面的示例通过查询创建一个 <xref:System.Data.DataView> 并通过将 <xref:System.Data.DataView.Sort%2A> 属性设置为空字符串清除排序。  
  
 [!code-csharp[DP DataView Samples#LDVClearSort](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvclearsort)]
 [!code-vb[DP DataView Samples#LDVClearSort](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvclearsort)]  
  
### <a name="example"></a>示例  

 下面的示例从 Contact 表创建 <xref:System.Data.DataView> 并将 <xref:System.Data.DataView.Sort%2A> 属性设置为按姓氏以降序排序。 然后通过将 <xref:System.Data.DataView.Sort%2A> 属性设置为 `null` 来清除排序信息：  
  
 [!code-csharp[DP DataView Samples#LDVClearSort2](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP DataView Samples/CS/Form1.cs#ldvclearsort2)]
 [!code-vb[DP DataView Samples#LDVClearSort2](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP DataView Samples/VB/Form1.vb#ldvclearsort2)]  
  
## <a name="see-also"></a>请参阅

- [数据绑定和 LINQ to DataSet](data-binding-and-linq-to-dataset.md)
- [使用 DataView 进行筛选](filtering-with-dataview-linq-to-dataset.md)
- [对数据进行排序](/previous-versions/visualstudio/visual-studio-2013/bb546145(v=vs.120))
