---
title: 数据表编辑
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: f08008a9-042e-4de9-94f3-4f0e502b1eb5
ms.openlocfilehash: 4fdb19e7fa014bf4a7c924b1fbae53fa44de6e3c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91153256"
---
# <a name="datatable-edits"></a>数据表编辑

当您在 <xref:System.Data.DataRow> 中更改列值时，所做更改会立即置于行的当前状态中。 然后，将 <xref:System.Data.DataRowState> 设置为 "**已修改**"，并使用 DataRow 的或方法来接受或拒绝更改 <xref:System.Data.DataRow.AcceptChanges%2A> <xref:System.Data.DataRow.RejectChanges%2A> 。 **DataRow** **DataRow**还提供了三种方法，可用于在编辑行时挂起行的状态。 这三个方法是 <xref:System.Data.DataRow.BeginEdit%2A>、<xref:System.Data.DataRow.EndEdit%2A> 和 <xref:System.Data.DataRow.CancelEdit%2A>。  
  
 当直接在 **datarow** 中修改列值时， **Datarow** 会使用 **Current**、 **Default**和 **原始** 行版本来管理列值。 除了这些行版本之外， **BeginEdit**、 **EndEdit**和 **CancelEdit** 方法使用第四个行版本： "已 **建议**"。 有关行版本的详细信息，请参阅 [行状态和行版本](row-states-and-row-versions.md)。  
  
 **建议**的行版本在编辑操作期间存在，该操作通过调用**BeginEdit**开始，并通过使用**EndEdit**或**CancelEdit**或通过调用**AcceptChanges**或**RejectChanges**结束。  
  
 在编辑操作期间，可以通过在**DataTable**的**ColumnChanged**事件中计算**ProposedValue** ，将验证逻辑应用于单个列。 **ColumnChanged**事件保存**DataColumnChangeEventArgs** ，可保持对正在更改的列和**ProposedValue**的引用。 计算了建议值后，可以对其进行修改或取消编辑。 当编辑结束时，该行将移出 **建议** 状态。  
  
 可以通过调用 **EndEdit**来确认编辑，也可以通过调用 **CancelEdit**来取消编辑。 请注意，尽管**EndEdit**确认了您的编辑，但在调用**AcceptChanges**之前，**数据集**并不会实际接受更改。 另请注意，如果在使用**EndEdit**或**CancelEdit**结束编辑之前调用**AcceptChanges** ，编辑将结束，同时将为**当前**行版本和**原始**行版本接受**建议**的行值。 同样，调用 **RejectChanges** 会结束编辑并放弃 **当前** 和 **建议** 的行版本。 调用**AcceptChanges**或**RejectChanges**后，调用**EndEdit**或**CancelEdit**不起作用，因为编辑已结束。  
  
 下面的示例演示如何将 **BeginEdit** 与 **EndEdit** 和 **CancelEdit**一起使用。 该示例还检查**ColumnChanged**事件中的**ProposedValue** ，并决定是否取消编辑。  
  
```vb  
Dim workTable As DataTable = New DataTable  
workTable.Columns.Add("LastName", Type.GetType("System.String"))  
  
AddHandler workTable.ColumnChanged, _  
  New DataColumnChangeEventHandler(AddressOf OnColumnChanged)  
  
Dim workRow As DataRow = workTable.NewRow()  
workRow(0) = "Smith"  
workTable.Rows.Add(workRow)  
  
workRow.BeginEdit()  
' Causes the ColumnChanged event to write a message and cancel the edit.  
workRow(0) = ""
workRow.EndEdit()  
  
' Displays "Smith, New".  
Console.WriteLine("{0}, {1}", workRow(0), workRow.RowState)  
  
Private Shared Sub OnColumnChanged( _  
  sender As Object, args As DataColumnChangeEventArgs)  
  If args.Column.ColumnName = "LastName" Then  
    If args.ProposedValue.ToString() = "" Then  
      Console.WriteLine("Last Name cannot be blank.  Edit canceled.")  
      args.Row.CancelEdit()  
    End If  
  End If  
End Sub  
```  
  
```csharp  
DataTable workTable  = new DataTable();  
workTable.Columns.Add("LastName", typeof(String));  
  
workTable.ColumnChanged +=
  new DataColumnChangeEventHandler(OnColumnChanged);  
  
DataRow workRow = workTable.NewRow();  
workRow[0] = "Smith";  
workTable.Rows.Add(workRow);  
  
workRow.BeginEdit();  
// Causes the ColumnChanged event to write a message and cancel the edit.  
workRow[0] = "";
workRow.EndEdit();  
  
// Displays "Smith, New".  
Console.WriteLine("{0}, {1}", workRow[0], workRow.RowState);
  
protected static void OnColumnChanged(  
  Object sender, DataColumnChangeEventArgs args)  
{  
  if (args.Column.ColumnName == "LastName")  
    if (args.ProposedValue.ToString() == "")  
    {  
      Console.WriteLine("Last Name cannot be blank. Edit canceled.");  
      args.Row.CancelEdit();  
    }  
}  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Data.DataRow>
- <xref:System.Data.DataTable>
- <xref:System.Data.DataRowVersion>
- [操作数据表中的数据](manipulating-data-in-a-datatable.md)
- [处理数据表事件](handling-datatable-events.md)
- [ADO.NET 概述](../ado-net-overview.md)
