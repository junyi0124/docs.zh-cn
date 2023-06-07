---
title: 行状态和行版本
ms.date: 07/19/2018
dev_langs:
- csharp
- vb
ms.assetid: 2e6642c9-bfc6-425c-b3a7-e4912ffa6c1f
ms.openlocfilehash: 1b80ae78fad22989f99fb1e992d4978a192e0c66
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204523"
---
# <a name="row-states-and-row-versions"></a>行状态和行版本

ADO.NET 用行状态和行版本管理表中的行。 行状态指示行的状态；行版本在修改行中存储的值时维护各个阶段的值，包括当前值、原始值和默认值。 例如，在修改了行中的某列后，该行的行状态将为 `Modified`，并且有两个行版本：`Current`（包含行的当前值）和 `Original`（包含列修改前行的值）。  
  
 每个 <xref:System.Data.DataRow> 对象都具有 <xref:System.Data.DataRow.RowState%2A> 属性，您可以检查此属性来确定行的当前状态。 下表提供了对每个 `RowState` 枚举值的简短说明。  
  
|RowState 值|描述|  
|--------------------|-----------------|  
|<xref:System.Data.DataRowState.Unchanged>|自上次调用 `AcceptChanges` 以来或由 `DataAdapter.Fill` 创建该行以来，没有进行任何更改。|  
|<xref:System.Data.DataRowState.Added>|已将该行添加到表中，但尚未调用 `AcceptChanges`。|  
|<xref:System.Data.DataRowState.Modified>|已更改了行的某个元素。|  
|<xref:System.Data.DataRowState.Deleted>|已从表中删除该行，并且尚未调用 `AcceptChanges`。|  
|<xref:System.Data.DataRowState.Detached>|该行不是任何 `DataRowCollection` 的一部分。 新创建的行的 `RowState` 设置为 `Detached`。 通过调用 `DataRow` 方法将新的 `DataRowCollection` 添加到 `Add` 后，`RowState` 属性的值设置为 `Added`。<br /><br /> 将使用 `Detached` 方法，或使用 `DataRowCollection` 方法接着使用 `Remove` 方法从 `Delete` 中移除的行也设置为 `AcceptChanges`。|  
  
 对 `AcceptChanges`、<xref:System.Data.DataSet> 或 <xref:System.Data.DataTable> 调用 <xref:System.Data.DataRow> 时，会移除行状态为 `Deleted` 的所有行。 剩余行的行状态为 `Unchanged`，并且 `Original` 行版本中的值将被 `Current` 行版本值覆盖。 调用 `RejectChanges` 时，会移除行状态为 `Added` 的所有行。 剩余行的行状态为 `Unchanged`，并且 `Current` 行版本中的值将被 `Original` 行版本值覆盖。  
  
 通过列引用来传递 <xref:System.Data.DataRowVersion> 参数，您可以查看行的不同行版本，如下面的示例所示。  
  
```vb  
Dim custRow As DataRow = custTable.Rows(0)  
Dim custID As String = custRow("CustomerID", DataRowVersion.Original).ToString()  
```  
  
```csharp  
DataRow custRow = custTable.Rows[0];  
string custID = custRow["CustomerID", DataRowVersion.Original].ToString();  
```  
  
 下表提供了对每个 `DataRowVersion` 枚举值的简短说明。  
  
|DataRowVersion 值|描述|  
|--------------------------|-----------------|  
|<xref:System.Data.DataRowVersion.Current>|行的当前值。 如果行的 `RowState` 为 `Deleted`，则不存在此行版本。|  
|<xref:System.Data.DataRowVersion.Default>|特定行的默认行版本。 `Added`、`Modified` 或 `Deleted` 行的默认行版本是 `Current`。 `Detached` 行的默认行版本是 `Proposed`。|  
|<xref:System.Data.DataRowVersion.Original>|行的原始值。 如果行的 `RowState` 为 `Added`，则不存在此行版本。|  
|<xref:System.Data.DataRowVersion.Proposed>|行的建议值。 在对行进行编辑操作的过程中，或者对于不属于 `DataRowCollection` 的行，存在此行版本。|  
  
 通过调用 `DataRow` 方法并将 <xref:System.Data.DataRow.HasVersion%2A> 作为参数传递，您可以测试 `DataRowVersion` 是否具有特定的行版本。 例如，在调用 `DataRow.HasVersion(DataRowVersion.Original)` 之前，`false` 对新添加的行将返回 `AcceptChanges`。  
  
 下面的代码示例演示表中所有已删除行中的值。 `Deleted` 行没有 `Current` 行版本，因此在访问列值时必须传递 `DataRowVersion.Original`。  
  
```vb  
Dim catTable As DataTable = catDS.Tables("Categories")  
  
Dim delRows() As DataRow = catTable.Select(Nothing, Nothing, DataViewRowState.Deleted)  
  
Console.WriteLine("Deleted rows:" & vbCrLf)  
  
Dim catCol As DataColumn  
Dim delRow As DataRow  
  
For Each catCol In catTable.Columns  
  Console.Write(catCol.ColumnName & vbTab)  
Next  
Console.WriteLine()  
  
For Each delRow In delRows  
  For Each catCol In catTable.Columns  
    Console.Write(delRow(catCol, DataRowVersion.Original) & vbTab)  
  Next  
  Console.WriteLine()  
Next  
```  
  
```csharp  
DataTable catTable = catDS.Tables["Categories"];  
  
DataRow[] delRows = catTable.Select(null, null, DataViewRowState.Deleted);  
  
Console.WriteLine("Deleted rows:\n");  
  
foreach (DataColumn catCol in catTable.Columns)  
  Console.Write(catCol.ColumnName + "\t");  
Console.WriteLine();  
  
foreach (DataRow delRow in delRows)  
{  
  foreach (DataColumn catCol in catTable.Columns)  
    Console.Write(delRow[catCol, DataRowVersion.Original] + "\t");  
  Console.WriteLine();  
}  
```  
  
## <a name="see-also"></a>请参阅

- [操作数据表中的数据](manipulating-data-in-a-datatable.md)
- [数据集、数据表和数据视图](index.md)
- [DataAdapter 和 DataReader](../dataadapters-and-datareaders.md)
- [ADO.NET 概述](../ado-net-overview.md)
