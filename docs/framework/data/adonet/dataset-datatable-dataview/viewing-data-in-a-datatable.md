---
title: 查看数据表中的数据
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 1d26e0fb-f6e0-4afa-9a9c-b8d55b8f20dc
ms.openlocfilehash: c13f0b802b2714a17ea4014625a65ebd1b0011f4
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/07/2019
ms.locfileid: "70785850"
---
# <a name="viewing-data-in-a-datatable"></a>查看数据表中的数据

您可以使用**DataTable**的**Rows**和<xref:System.Data.DataTable> **Columns**集合来访问的内容。 你还可以使用<xref:System.Data.DataTable.Select%2A>方法，根据包括搜索条件、排序顺序和行状态的条件，返回**DataTable**中的数据子集。 此外，在使用主键值<xref:System.Data.DataRowCollection.Find%2A>搜索特定行时，可以使用**DataRowCollection**的方法。

**DataTable**对象的<xref:System.Data.DataRow> **Select**方法返回一组与指定条件匹配的对象。 **Select**使用筛选表达式、排序表达式和**DataViewRowState**的可选参数。 筛选表达式基于**DataColumn**值（如`LastName = 'Smith'`）标识要返回的行。 排序表达式遵循用于为列排序的标准 SQL 约定，例如 `LastName ASC, FirstName ASC`。 有关编写表达式的规则，请参阅<xref:System.Data.DataColumn.Expression%2A> **DataColumn**类的属性。

> [!TIP]
> 如果要对**datatable** <xref:System.Data.DataView>的**Select**方法执行多个调用，可以通过首先为**datatable**创建来提高性能。 创建**DataView**会索引表中的行。 然后， **Select**方法使用该索引，大大减少了生成查询结果的时间。 有关为**DataTable**创建**DataView**的信息，请参阅[dataview](dataviews.md)。

**Select**方法确定要基于<xref:System.Data.DataViewRowState>其查看或操作的行的版本。 下表描述了可能的**DataViewRowState**枚举值。

|DataViewRowState 值|描述|
|----------------------------|-----------------|
|**CurrentRows**|当前行，包括未更改的行、已添加的行和已修改的行。|
|**删除**|已删除的行。|
|**ModifiedCurrent**|当前版本，它是原始数据的修改版本。 （请参阅**ModifiedOriginal**。）|
|**ModifiedOriginal**|所有已修改行的原始版本。 当前版本可用于**ModifiedCurrent**。|
|**已**|新行。|
|**无**|无。|
|**OriginalRows**|原始行，包括未更改的行和已删除的行。|
|**变化**|未更改的行。|

在下面的示例中，对**DataSet**对象进行了筛选，以便您只使用其**DataViewRowState**设置为**CurrentRows**的行。

```vb
Dim column As DataColumn
Dim row As DataRow

Dim currentRows() As DataRow = _
    workTable.Select(Nothing, Nothing, DataViewRowState.CurrentRows)

If (currentRows.Length < 1 ) Then
  Console.WriteLine("No Current Rows Found")
Else
  For Each column in workTable.Columns
    Console.Write(vbTab & column.ColumnName)
  Next

  Console.WriteLine(vbTab & "RowState")

  For Each row In currentRows
    For Each column In workTable.Columns
      Console.Write(vbTab & row(column).ToString())
    Next

    Dim rowState As String = _
        System.Enum.GetName(row.RowState.GetType(), row.RowState)
    Console.WriteLine(vbTab & rowState)
  Next
End If
```

```csharp
DataRow[] currentRows = workTable.Select(
    null, null, DataViewRowState.CurrentRows);

if (currentRows.Length < 1 )
  Console.WriteLine("No Current Rows Found");
else
{
  foreach (DataColumn column in workTable.Columns)
    Console.Write("\t{0}", column.ColumnName);

  Console.WriteLine("\tRowState");

  foreach (DataRow row in currentRows)
  {
    foreach (DataColumn column in workTable.Columns)
      Console.Write("\t{0}", row[column]);

    Console.WriteLine("\t" + row.RowState);
  }
}
```

**Select**方法可用于返回具有不同的**RowState**值或字段值的行。 下面的示例返回引用所有已删除行的**datarow**数组，并返回引用所有行的另一个**datarow**数组，这些行由**custlname 排序 (** 排序，其中**CustID**列大于5。 浪中的信息**已删除**行中，请参阅[行状态和行版本](row-states-and-row-versions.md)。

```vb
' Retrieve all deleted rows.
Dim deletedRows() As DataRow = workTable.Select(Nothing, Nothing, DataViewRowState.Deleted)

' Retrieve rows where CustID > 5, and order by CustLName.
Dim custRows() As DataRow = workTable.Select( _
    "CustID > 5", "CustLName ASC")
```

```csharp
// Retrieve all deleted rows.
DataRow[] deletedRows = workTable.Select(
    null, null, DataViewRowState.Deleted);

// Retrieve rows where CustID > 5, and order by CustLName.
DataRow[] custRows = workTable.Select("CustID > 5", "CustLName ASC");
```

## <a name="see-also"></a>请参阅

- <xref:System.Data.DataRow>
- <xref:System.Data.DataSet>
- <xref:System.Data.DataTable>
- <xref:System.Data.DataViewRowState>
- [操作数据表中的数据](manipulating-data-in-a-datatable.md)
- [行状态和行版本](row-states-and-row-versions.md)
- [ADO.NET 概述](../ado-net-overview.md)
