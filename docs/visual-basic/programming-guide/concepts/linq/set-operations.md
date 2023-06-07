---
title: Set 运算
ms.date: 07/20/2015
ms.assetid: 2b06e822-e030-438f-9db7-ee402bd3a706
ms.openlocfilehash: b6ff14794343ae7623ee38894cef02cfc0a2a597
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406815"
---
# <a name="set-operations-visual-basic"></a>设置操作（Visual Basic）

LINQ 中的集运算是指根据相同或不同集合（或集）中是否存在等效元素来生成结果集的查询运算。

下节列出了执行集运算的标准查询运算符方法。

## <a name="methods"></a>方法

|方法名|说明|Visual Basic 查询表达式语法|详细信息|
|-----------------|-----------------|------------------------------------------|----------------------|
|Distinct|删除集合中的重复值。|`Distinct`|<xref:System.Linq.Enumerable.Distinct%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Distinct%2A?displayProperty=nameWithType>|
|Except|返回差集，差集指位于一个集合但不位于另一个集合的元素。|不适用。|<xref:System.Linq.Enumerable.Except%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Except%2A?displayProperty=nameWithType>|
|Intersect|返回交集，交集指同时出现在两个集合中的元素。|不适用。|<xref:System.Linq.Enumerable.Intersect%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Intersect%2A?displayProperty=nameWithType>|
|Union|返回并集，并集指位于两个集合中任一集合的唯一的元素。|不适用。|<xref:System.Linq.Enumerable.Union%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Union%2A?displayProperty=nameWithType>|

## <a name="comparison-of-set-operations"></a>比较集运算

### <a name="distinct"></a>Distinct

下图演示字符序列上 <xref:System.Linq.Enumerable.Distinct%2A?displayProperty=nameWithType> 方法的行为。 返回的序列包含输入序列的唯一元素。

![显示 Distinct() 的行为的图。](./media/set-operations/distinct-method-behavior.png)

### <a name="except"></a>Except

下图演示 <xref:System.Linq.Enumerable.Except%2A?displayProperty=nameWithType> 的行为。 返回的序列只包含位于第一个输入序列但不位于第二个输入序列的元素。

![显示 Except() 的操作的图](./media/set-operations/except-behavior-graphic.png "显示 Except 的行为。")

### <a name="intersect"></a>Intersect

下图演示 <xref:System.Linq.Enumerable.Intersect%2A?displayProperty=nameWithType> 的行为。 返回的序列包含两个输入序列共有的元素。

![显示两个序列的交集的图。](./media/set-operations/intersection-two-sequences.png)

### <a name="union"></a>Union

下图演示对两个字符序列执行的联合操作。 返回的序列包含两个输入序列的唯一元素。

![显示两个序列的并集的图。](./media/set-operations/union-operation-two-sequences.png)

## <a name="query-expression-syntax-example"></a>查询表达式语法示例

下面的示例 `Distinct` 在 LINQ 查询中使用子句从整数列表返回唯一的数字。

[!code-vb[CsLINQSetOps#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/CsLINQSetOps/VB/setops.vb#1)]

## <a name="see-also"></a>另请参阅

- <xref:System.Linq>
- [标准查询运算符概述 (Visual Basic)](standard-query-operators-overview.md)
- [Distinct 子句](../../../language-reference/queries/distinct-clause.md)
- [如何：合并和比较字符串集合（LINQ）（Visual Basic）](how-to-combine-and-compare-string-collections-linq.md)
- [如何：查找两个列表之间的差集（LINQ）（Visual Basic）](how-to-find-the-set-difference-between-two-lists-linq.md)
