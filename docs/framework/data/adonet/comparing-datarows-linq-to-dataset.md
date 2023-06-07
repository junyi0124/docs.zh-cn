---
title: 比较 DataRow (LINQ to DataSet)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 8fe0eadf-297b-487c-8d4b-7816753c2883
ms.openlocfilehash: 8cce52734c83e42312d71806d4151ef21e4df0ba
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203795"
---
# <a name="comparing-datarows-linq-to-dataset"></a>比较 DataRow (LINQ to DataSet)

 (LINQ) 的语言集成查询定义了用于比较源元素的各种集运算符，以确定它们是否相等。 LINQ 提供以下集运算符：  
  
- <xref:System.Linq.Enumerable.Distinct%2A>  
  
- <xref:System.Linq.Enumerable.Union%2A>  
  
- <xref:System.Linq.Enumerable.Intersect%2A>  
  
- <xref:System.Linq.Enumerable.Except%2A>  
  
 这些运算符通过对每个元素集合调用 <xref:System.Collections.Generic.IEqualityComparer%601.GetHashCode%2A> 和 <xref:System.Collections.Generic.IEqualityComparer%601.Equals%2A> 方法来比较源元素。 对于 <xref:System.Data.DataRow>，这些运算符执行引用比较，在对表格格式数据执行集合操作的情况下，这通常不是理想的行为。 对于集合运算，通常需要确定元素值而不是元素引用是否相等。 因此， <xref:System.Data.DataRowComparer> 类已添加到 LINQ to DataSet。 此类可用于比较行值。  
  
 <xref:System.Data.DataRowComparer> 类包含 <xref:System.Data.DataRow> 的值比较实现，所以此类可用于设置集合操作，例如 <xref:System.Linq.Enumerable.Distinct%2A>。 此类不能直接实例化，而必须使用 <xref:System.Data.DataRowComparer.Default%2A> 属性返回 <xref:System.Data.DataRowComparer%601> 的实例。 然后调用 <xref:System.Data.DataRowComparer%601.Equals%2A> 方法并作为输入参数传入要进行比较的两个 <xref:System.Data.DataRow> 对象。 如果两个 <xref:System.Data.DataRowComparer%601.Equals%2A> 对象中排序的列值集合相等，则 `true` 方法返回 <xref:System.Data.DataRow>，否则返回 `false`。  
  
## <a name="example"></a>示例  

 此示例使用 `Intersect` 返回两个表中都存在的联系人。  
  
 [!code-csharp[DP LINQ to DataSet Examples#Intersect2](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/CS/Program.cs#intersect2)]
 [!code-vb[DP LINQ to DataSet Examples#Intersect2](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/VB/Module1.vb#intersect2)]  
  
### <a name="example"></a>示例  

 下面的示例比较两个行并获取它们的哈希代码。  
  
 [!code-vb[DP LINQ to DataSet Examples#CompareDifferentRows](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/VB/Module1.vb#comparedifferentrows)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Data.DataRowComparer>
- [将数据加载到数据集中](loading-data-into-a-dataset.md)
- [LINQ to DataSet 示例](linq-to-dataset-examples.md)
