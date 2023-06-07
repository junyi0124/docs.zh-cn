---
title: 对数据进行排序
ms.date: 07/20/2015
ms.assetid: 6f81065c-0c89-4bf3-a6d8-442273f8810e
ms.openlocfilehash: a5ccff745995ed7f41731cf98fb7c49d3247d994
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406789"
---
# <a name="sorting-data-visual-basic"></a>对数据进行排序（Visual Basic）

排序操作基于一个或多个属性对序列的元素进行排序。 第一个排序条件对元素执行主要排序。 通过指定第二个排序条件，您可以对每个主要排序组内的元素进行排序。

下图演示对一个字符序列执行按字母排序操作的结果。

![展示按字母顺序排序操作的图。](./media/sorting-data/alphabetical-sort-operation.png)

下节列出了对数据进行排序的标准查询运算符方法。

## <a name="methods"></a>方法

|方法名|说明|Visual Basic 查询表达式语法|详细信息|
|-----------------|-----------------|------------------------------------------|----------------------|
|OrderBy|按升序对值排序。|`Order By`|<xref:System.Linq.Enumerable.OrderBy%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.OrderBy%2A?displayProperty=nameWithType>|
|OrderByDescending|按降序对值排序。|`Order By … Descending`|<xref:System.Linq.Enumerable.OrderByDescending%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.OrderByDescending%2A?displayProperty=nameWithType>|
|ThenBy|按升序执行次要排序。|`Order By …, …`|<xref:System.Linq.Enumerable.ThenBy%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.ThenBy%2A?displayProperty=nameWithType>|
|ThenByDescending|按降序执行次要排序。|`Order By …, … Descending`|<xref:System.Linq.Enumerable.ThenByDescending%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.ThenByDescending%2A?displayProperty=nameWithType>|
|Reverse|反转集合中元素的顺序。|不适用。|<xref:System.Linq.Enumerable.Reverse%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Reverse%2A?displayProperty=nameWithType>|

## <a name="query-expression-syntax-examples"></a>查询表达式语法示例

### <a name="primary-sort-examples"></a>主要排序示例

#### <a name="primary-ascending-sort"></a>主要升序排序

下面的示例演示如何在 LINQ 查询中使用 `Order By` 子句按字符串长度对数组中的字符串进行升序排序。

```vb
Dim words = {"the", "quick", "brown", "fox", "jumps"}

Dim sortQuery = From word In words
                Order By word.Length
                Select word

Dim sb As New System.Text.StringBuilder()
For Each str As String In sortQuery
    sb.AppendLine(str)
Next

' Display the results.
MsgBox(sb.ToString())

' This code produces the following output:

' the
' fox
' quick
' brown
' jumps
```

#### <a name="primary-descending-sort"></a>主要降序排序

下面的示例演示如何在 LINQ 查询中使用 `Order By Descending` 子句按字符串的第一个字母对字符串进行降序排序。

```vb
Dim words = {"the", "quick", "brown", "fox", "jumps"}

Dim sortQuery = From word In words
                Order By word.Substring(0, 1) Descending
                Select word

Dim sb As New System.Text.StringBuilder()
For Each str As String In sortQuery
    sb.AppendLine(str)
Next

' Display the results.
MsgBox(sb.ToString())

' This code produces the following output:

' the
' quick
' jumps
' fox
' brown
```

### <a name="secondary-sort-examples"></a>次要排序示例

#### <a name="secondary-ascending-sort"></a>次要升序排序

下面的示例演示如何在 LINQ 查询中使用 `Order By` 子句对数组中的字符串执行主要和次要排序。 首先按字符串长度，其次按字符串的第一个字母，对字符串进行升序排序。

```vb
Dim words = {"the", "quick", "brown", "fox", "jumps"}

Dim sortQuery = From word In words
                Order By word.Length, word.Substring(0, 1)
                Select word

Dim sb As New System.Text.StringBuilder()
For Each str As String In sortQuery
    sb.AppendLine(str)
Next

' Display the results.
MsgBox(sb.ToString())

' This code produces the following output:

' fox
' the
' brown
' jumps
' quick
```

#### <a name="secondary-descending-sort"></a>次要降序排序

下面的示例演示如何在 LINQ 查询中使用 `Order By Descending` 子句按升序执行主要排序，按降序执行次要排序。 首先按字符串长度，其次按字符串的第一个字母，对字符串进行排序。

```vb
Dim words = {"the", "quick", "brown", "fox", "jumps"}

Dim sortQuery = From word In words
                Order By word.Length, word.Substring(0, 1) Descending
                Select word

Dim sb As New System.Text.StringBuilder()
For Each str As String In sortQuery
    sb.AppendLine(str)
Next

' Display the results.
MsgBox(sb.ToString())

' This code produces the following output:

' fox
' the
' quick
' jumps
' brown
```

## <a name="see-also"></a>另请参阅

- <xref:System.Linq>
- [标准查询运算符概述 (Visual Basic)](standard-query-operators-overview.md)
- [Order By 子句](../../../language-reference/queries/order-by-clause.md)
- [如何：对查询结果进行排序](../../language-features/linq/how-to-sort-query-results-by-using-linq.md)
- [如何：按任意词或字段对文本数据进行排序或筛选 (LINQ) (Visual Basic)](how-to-sort-or-filter-text-data-by-any-word-or-field-linq.md)
