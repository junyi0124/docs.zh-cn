---
title: AcceptChange 和 RejectChange
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: e2d1a6fe-31f9-4b83-9728-06c406a3394e
ms.openlocfilehash: e29d2404d6d593b9a5b905206af3cdd3bc1a3e51
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91177587"
---
# <a name="acceptchanges-and-rejectchanges"></a>AcceptChange 和 RejectChange

在验证对中的数据所做更改的准确性之后， <xref:System.Data.DataTable> 您可以使用、或的方法接受更改， <xref:System.Data.DataRow.AcceptChanges%2A> <xref:System.Data.DataRow> <xref:System.Data.DataTable> <xref:System.Data.DataSet> 这会将 **当前** 行值设置为 **原始** 值，并将 **RowState** 属性设置为 **不变**。 接受或拒绝更改将清除所有 **RowError** 信息，并将 **HasErrors** 属性设置为 **false**。 接受或拒绝更改还可以影响在数据源中更新数据。 有关详细信息，请参阅 [用 Dataadapter 更新数据源](../updating-data-sources-with-dataadapters.md)。  
  
 如果**DataTable**中存在外键约束，则使用**AcceptChanges**和**RejectChanges**接受或拒绝的更改会根据**ForeignKeyConstraint**传播到**DataRow**的子行。 有关详细信息，请参阅 [DataTable 约束](datatable-constraints.md)。  
  
 以下示例检查有错误的行，如果可以会解决错误，拒绝无法解决错误的行。 请注意，对于已解决的错误， **RowError** 值会重置为空字符串，导致 **HasErrors** 属性设置为 **false**。 在已解决或拒绝所有包含错误的行时，将调用 **AcceptChanges** 来接受对整个 **DataTable**的所有更改。  
  
```vb  
If workTable.HasErrors Then  
  Dim errRow As DataRow  
  
  For Each errRow in workTable.GetErrors()  
  
    If errRow.RowError = "Total cannot exceed 1000." Then  
      errRow("Total") = 1000  
      errRow.RowError = ""    ' Clear the error.  
    Else  
      errRow.RejectChanges()  
    End If  
  Next  
End If  
  
workTable.AcceptChanges()  
```  
  
```csharp  
if (workTable.HasErrors)  
{  
  
  foreach (DataRow errRow in workTable.GetErrors())  
  {  
    if (errRow.RowError == "Total cannot exceed 1000.")  
    {  
      errRow["Total"] = 1000;  
      errRow.RowError = "";    // Clear the error.  
    }  
    else  
      errRow.RejectChanges();  
  }  
}  
  
workTable.AcceptChanges();  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Data.DataRow>
- <xref:System.Data.DataSet>
- <xref:System.Data.DataTable>
- [操作数据表中的数据](manipulating-data-in-a-datatable.md)
- [ADO.NET 概述](../ado-net-overview.md)
