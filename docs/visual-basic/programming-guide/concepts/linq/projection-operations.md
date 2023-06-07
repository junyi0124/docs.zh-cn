---
title: 投影运算
ms.date: 07/20/2015
ms.assetid: b8d38e6d-21cf-4619-8dbb-94476f4badc7
ms.openlocfilehash: 4795bdaba53949b34fe380ea9c51025ce43c40db
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396332"
---
# <a name="projection-operations-visual-basic"></a>投影操作（Visual Basic）

投影是指将对象转换为一种新形式的操作，该形式通常只包含那些将随后使用的属性。 通过使用投影，您可以构造从每个对象生成的新类型。 可以投影属性，并对该属性执行数学函数。 还可以在不更改原始对象的情况下投影该对象。

下面一节列出了执行投影的标准查询运算符方法。

## <a name="methods"></a>方法

|方法名|说明|Visual Basic 查询表达式语法|详细信息|
|-----------------|-----------------|------------------------------------------|----------------------|
|选择|投影基于转换函数的值。|`Select`|<xref:System.Linq.Enumerable.Select%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Select%2A?displayProperty=nameWithType>|
|SelectMany|投影基于转换函数的值序列，然后将它们展平为一个序列。|使用多个 `From` 子句|<xref:System.Linq.Enumerable.SelectMany%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.SelectMany%2A?displayProperty=nameWithType>|

## <a name="query-expression-syntax-examples"></a>查询表达式语法示例

### <a name="select"></a>选择

下面的示例使用 `Select` 子句来投影字符串列表中每个字符串的第一个字母。

```vb
Dim words = New List(Of String) From {"an", "apple", "a", "day"}

Dim query = From word In words
            Select word.Substring(0, 1)

Dim sb As New System.Text.StringBuilder()
For Each letter As String In query
    sb.AppendLine(letter)
Next

' Display the output.
MsgBox(sb.ToString())

' This code produces the following output:

' a
' a
' a
' d
```

### <a name="selectmany"></a>SelectMany

下面的示例使用多个 `From` 子句来投影字符串列表中每个字符串的每个单词。

```vb
Dim phrases = New List(Of String) From {"an apple a day", "the quick brown fox"}

Dim query = From phrase In phrases
            From word In phrase.Split(" "c)
            Select word

Dim sb As New System.Text.StringBuilder()
For Each str As String In query
    sb.AppendLine(str)
Next

' Display the output.
MsgBox(sb.ToString())

' This code produces the following output:

' an
' apple
' a
' day
' the
' quick
' brown
' fox
```

## <a name="select-versus-selectmany"></a>Select 与 SelectMany

`Select()` 和 `SelectMany()` 的工作都是依据源值生成一个或多个结果值。 `Select()` 为每个源值生成一个结果值。 因此，总体结果是一个与源集合具有相同元素数目的集合。 与之相反，`SelectMany()` 生成单个总体结果，其中包含来自每个源值的串联子集合。 作为参数传递到 `SelectMany()` 的转换函数必须为每个源值返回一个可枚举值序列。 然后，`SelectMany()` 串联这些可枚举序列，以创建一个大的序列。

下面两个插图演示了这两个方法的操作之间的概念性区别。 在每种情况下，假定选择器（转换）函数从每个源值中选择一个由花卉数据组成的数组。

下图描述 `Select()` 如何返回一个与源集合具有相同元素数目的集合。

![显示 Select() 的操作的图](./media/projection-operations/select-action-graphic.png)

下图描述 `SelectMany()` 如何将中间数组序列串联为一个最终结果值，其中包含每个中间数组中的每个值。

![显示 SelectMany() 的操作的图。](./media/projection-operations/select-many-action-graphic.png )

### <a name="code-example"></a>代码示例

下面的示例比较 `Select()` 和 `SelectMany()` 的行为。 代码通过从源集合的每个花卉名称列表中提取前两项来创建一个“花束”。 此示例中，transform 函数 <xref:System.Linq.Enumerable.Select%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2C%60%601%7D%29> 使用的“单值”本身即是值的集合。 这需要额外的 `For Each` 循环，以便枚举每个子序列中的每个字符串。

```vb
Class Bouquet
    Public Flowers As List(Of String)
End Class

Sub SelectVsSelectMany()
    Dim bouquets = New List(Of Bouquet) From {
        New Bouquet With {.Flowers = New List(Of String)(New String() {"sunflower", "daisy", "daffodil", "larkspur"})},
        New Bouquet With {.Flowers = New List(Of String)(New String() {"tulip", "rose", "orchid"})},
        New Bouquet With {.Flowers = New List(Of String)(New String() {"gladiolis", "lily", "snapdragon", "aster", "protea"})},
        New Bouquet With {.Flowers = New List(Of String)(New String() {"larkspur", "lilac", "iris", "dahlia"})}}

    Dim output As New System.Text.StringBuilder

    ' Select()
    Dim query1 = bouquets.Select(Function(b) b.Flowers)

    output.AppendLine("Using Select():")
    For Each flowerList In query1
        For Each str As String In flowerList
            output.AppendLine(str)
        Next
    Next

    ' SelectMany()
    Dim query2 = bouquets.SelectMany(Function(b) b.Flowers)

    output.AppendLine(vbCrLf & "Using SelectMany():")
    For Each str As String In query2
        output.AppendLine(str)
    Next

    ' Display the output
    MsgBox(output.ToString())

    ' This code produces the following output:
    '
    ' Using Select():
    ' sunflower
    ' daisy
    ' daffodil
    ' larkspur
    ' tulip
    ' rose
    ' orchid
    ' gladiolis
    ' lily
    ' snapdragon
    ' aster
    ' protea
    ' larkspur
    ' lilac
    ' iris
    ' dahlia

    ' Using SelectMany()
    ' sunflower
    ' daisy
    ' daffodil
    ' larkspur
    ' tulip
    ' rose
    ' orchid
    ' gladiolis
    ' lily
    ' snapdragon
    ' aster
    ' protea
    ' larkspur
    ' lilac
    ' iris
    ' dahlia

End Sub
```

## <a name="see-also"></a>另请参阅

- <xref:System.Linq>
- [标准查询运算符概述 (Visual Basic)](standard-query-operators-overview.md)
- [Select 子句](../../../language-reference/queries/select-clause.md)
- [如何：使用联接合并数据](../../language-features/linq/how-to-combine-data-with-linq-by-using-joins.md)
- [如何：从多个源填充对象集合（LINQ）（Visual Basic）](how-to-populate-object-collections-from-multiple-sources-linq.md)
- [如何：以特定类型返回 LINQ 查询结果](../../language-features/linq/how-to-return-a-linq-query-result-as-a-specific-type.md)
- [如何：使用组将一个文件拆分成多个文件（LINQ）（Visual Basic）](how-to-split-a-file-into-many-files-by-using-groups-linq.md)
