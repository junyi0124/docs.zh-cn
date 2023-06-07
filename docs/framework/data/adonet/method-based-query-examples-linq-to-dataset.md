---
title: 基于方法的查询示例 (LINQ to DataSet)
ms.date: 03/30/2017
ms.assetid: d340775c-7f39-4087-a290-5cbec6cfa68e
ms.openlocfilehash: 1c1ef8f1b6c05415ac6f5ee59d9a415bfab2c410
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91175377"
---
# <a name="method-based-query-examples-linq-to-dataset"></a>基于方法的查询示例 (LINQ to DataSet)

本节提供使用标准查询运算符的基于方法的查询语法中的 LINQ to DataSet 编程示例。 <xref:System.Data.DataSet>这些示例中使用的是使用方法填充的 `FillDataSet` ，该方法是在将[数据加载到数据集](loading-data-into-a-dataset.md)时指定的。 有关详细信息，请参阅 [标准查询运算符概述 (c # ) ](../../../csharp/programming-guide/concepts/linq/standard-query-operators-overview.md) 或 [标准查询运算符概述 (Visual Basic) ](../../../visual-basic/programming-guide/concepts/linq/standard-query-operators-overview.md)。  
  
## <a name="in-this-section"></a>本节内容  

 [投影](method-based-query-syntax-examples-projection.md)  
 本主题中的示例演示如何使用 <xref:System.Linq.Enumerable.Select%2A> 和 <xref:System.Linq.Enumerable.SelectMany%2A> 方法来查询 <xref:System.Data.DataSet>。  
  
 [分区](method-based-query-syntax-examples-partitioning-linq.md)  
 本主题中的示例演示如何使用 <xref:System.Linq.Enumerable.Skip%2A> 和 <xref:System.Linq.Enumerable.Take%2A> 方法来查询 <xref:System.Data.DataSet> 并分隔结果。  
  
 [中间件排序](method-based-query-syntax-examples-ordering-linq-to-dataset.md)  
 本主题中的示例演示如何使用 <xref:System.Linq.Enumerable.OrderBy%2A>、<xref:System.Linq.Enumerable.OrderByDescending%2A>、<xref:System.Linq.Enumerable.Reverse%2A> 和 <xref:System.Linq.Enumerable.ThenByDescending%2A> 方法来查询 <xref:System.Data.DataSet> 并排序结果。  
  
 [集合运算符](method-based-query-syntax-examples-set-operators.md)  
 本主题中的示例演示如何使用 <xref:System.Linq.Enumerable.Distinct%2A>、<xref:System.Linq.Enumerable.Except%2A>、<xref:System.Linq.Enumerable.Intersect%2A> 和 <xref:System.Linq.Enumerable.Union%2A> 运算符对多组数据行执行基于值的比较运算。  
  
 [转换运算符](method-based-query-syntax-examples-conversion-operators.md)  
 本主题中的示例演示如何使用 <xref:System.Linq.Enumerable.ToArray%2A>、<xref:System.Linq.Enumerable.ToDictionary%2A> 和 <xref:System.Linq.Enumerable.ToList%2A> 方法立即执行查询表达式。  
  
 [元素运算符](method-based-query-syntax-examples-element-operators.md)  
 本主题中的示例演示如何使用 <xref:System.Linq.Enumerable.First%2A> 和 <xref:System.Linq.Enumerable.ElementAt%2A> 方法来获取 <xref:System.Data.DataRow> 中的 <xref:System.Data.DataSet> 元素。  
  
 [聚合运算符](method-based-query-syntax-examples-aggregate-operators.md)  
 本主题中的示例演示如何使用 <xref:System.Linq.Enumerable.Average%2A>、<xref:System.Linq.Enumerable.Count%2A>、<xref:System.Linq.Enumerable.Max%2A>、<xref:System.Linq.Enumerable.Min%2A> 和 <xref:System.Linq.Enumerable.Sum%2A> 方法来查询 <xref:System.Data.DataSet> 并聚合数据。  
  
 [Join](method-based-query-syntax-examples-join-linq-to-dataset.md)  
 本主题中的示例演示如何使用 <xref:System.Linq.Enumerable.GroupJoin%2A> 和 <xref:System.Linq.Enumerable.Join%2A> 方法来查询 <xref:System.Data.DataSet>。  
  
## <a name="see-also"></a>请参阅

- [查询表达式示例](query-expression-examples-linq-to-dataset.md)
- [数据集特定的运算符示例](dataset-specific-operator-examples-linq-to-dataset.md)
- [LINQ to DataSet 示例](linq-to-dataset-examples.md)
