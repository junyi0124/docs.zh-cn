---
title: 聚合运算
ms.date: 07/20/2015
ms.assetid: 0f47e92c-5dd2-4007-baf4-c5fe5dc3b4a8
ms.openlocfilehash: 1cf82d8acfdb1f8b0fc33c324064574b0dd01f4a
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91078393"
---
# <a name="aggregation-operations-visual-basic"></a>聚合操作 (Visual Basic) 

聚合运算从值的集合中计算出单个值。 例如，从一个月累计的每日温度值计算出日平均温度值就是一个聚合运算。  
  
 下图显示对数字序列进行两种不同聚合操作所得结果。 第一个操作累加数字。 第二个操作返回序列中的最大值。  
  
 ![显示 LINQ 聚合运算的插图。](./media/aggregation-operations/linq-aggregation-operations.png)  
  
 下节列出了执行聚合运算的标准查询运算符方法。  
  
## <a name="methods"></a>方法  
  
|方法名|描述|Visual Basic 查询表达式语法|详细信息|  
|-----------------|-----------------|------------------------------------------|----------------------|  
|聚合|对集合的值执行自定义聚合运算。|不适用。|<xref:System.Linq.Enumerable.Aggregate%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Aggregate%2A?displayProperty=nameWithType>|  
|平均值|计算值集合的平均值。|`Aggregate … In … Into Average()`|<xref:System.Linq.Enumerable.Average%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Average%2A?displayProperty=nameWithType>|  
|计数|对集合中元素计数，可选择仅对满足谓词函数的元素计数。|`Aggregate … In … Into Count()`|<xref:System.Linq.Enumerable.Count%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Count%2A?displayProperty=nameWithType>|  
|LongCount|对大型集合中元素计数，可选择仅对满足谓词函数的元素计数。|`Aggregate … In … Into LongCount()`|<xref:System.Linq.Enumerable.LongCount%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.LongCount%2A?displayProperty=nameWithType>|  
|最大值|确定集合中的最大值。|`Aggregate … In … Into Max()`|<xref:System.Linq.Enumerable.Max%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Max%2A?displayProperty=nameWithType>|  
|最小值|确定集合中的最小值。|`Aggregate … In … Into Min()`|<xref:System.Linq.Enumerable.Min%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Min%2A?displayProperty=nameWithType>|  
|Sum|对集合中的值求和。|`Aggregate … In … Into Sum()`|<xref:System.Linq.Enumerable.Sum%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Sum%2A?displayProperty=nameWithType>|  
  
## <a name="query-expression-syntax-examples"></a>查询表达式语法示例  
  
### <a name="average"></a>平均值  

 下面的代码示例使用 `Aggregate Into Average` Visual Basic 中的子句来计算表示温度的数值数组中的平均温度。  
  
 [!code-vb[CsLINQAggregating#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/CsLINQAggregating/VB/Aggregating.vb#1)]  
  
### <a name="count"></a>Count  

 下面的代码示例使用 `Aggregate Into Count` Visual Basic 中的子句来计算数组中大于或等于80的值的数目。  
  
 [!code-vb[CsLINQAggregating#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/CsLINQAggregating/VB/Aggregating.vb#2)]  
  
### <a name="longcount"></a>LongCount  

 下面的代码示例使用 `Aggregate Into LongCount` 子句来计算数组中的值的数目。  
  
 [!code-vb[CsLINQAggregating#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/CsLINQAggregating/VB/Aggregating.vb#3)]  
  
### <a name="max"></a>最大值  

 下面的代码示例使用 `Aggregate Into Max` 子句来计算表示温度的数字数组中的最大温度。  
  
 [!code-vb[CsLINQAggregating#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/CsLINQAggregating/VB/Aggregating.vb#4)]  
  
### <a name="min"></a>Min  

 下面的代码示例使用 `Aggregate Into Min` 子句来计算表示温度的数字数组中的最小温度。  
  
 [!code-vb[CsLINQAggregating#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/CsLINQAggregating/VB/Aggregating.vb#5)]  
  
### <a name="sum"></a>Sum  

 下面的代码示例使用 `Aggregate Into Sum` 子句来计算表示支出的值数组中的总支出量。  
  
 [!code-vb[CsLINQAggregating#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/CsLINQAggregating/VB/Aggregating.vb#6)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.Linq>
- [标准查询运算符概述 (Visual Basic)](standard-query-operators-overview.md)
- [Aggregate Clause](../../../language-reference/queries/aggregate-clause.md)
- [如何：在 CSV 文本文件中计算列值 (LINQ)  (Visual Basic) ](how-to-compute-column-values-in-a-csv-text-file-linq.md)
- [如何：对数据进行计数、求和与求平均值计算](../../language-features/linq/how-to-count-sum-or-average-data-by-using-linq.md)
- [如何：查找查询结果中的最小值或最大值](../../language-features/linq/how-to-find-the-minimum-or-maximum-value-in-a-query-result.md)
- [如何：查询目录树中的一个或多个最大的文件 (LINQ) (Visual Basic)](how-to-query-for-the-largest-file-or-files-in-a-directory-tree.md)
- [如何：查询一组文件夹中的总字节数 (LINQ)  (Visual Basic) ](how-to-query-for-the-total-number-of-bytes-in-a-set-of-folders.md)
