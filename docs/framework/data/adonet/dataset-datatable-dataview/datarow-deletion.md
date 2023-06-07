---
title: DataRow 删除
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: c34f531d-4b9b-4071-b2d7-342c402aa586
ms.openlocfilehash: 2092d7319a398bbdeaef764d677818f78ddf9de9
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91153347"
---
# <a name="datarow-deletion"></a>DataRow 删除

可以使用两种方法 <xref:System.Data.DataRow> 从对象中删除对象 <xref:System.Data.DataTable> ：对象的 **Remove** 方法 <xref:System.Data.DataRowCollection> 和 <xref:System.Data.DataRow.Delete%2A> **DataRow** 对象的方法。 此 <xref:System.Data.DataRowCollection.Remove%2A> 方法从**DataRowCollection**中删除**DataRow** ，而 <xref:System.Data.DataRow.Delete%2A> 方法仅将行标记为要删除。 当应用程序调用 **AcceptChanges** 方法时，将发生实际的删除。 通过使用 <xref:System.Data.DataRow.Delete%2A>，您可以在实际删除行之前，先以编程方式来检查哪些行已标记为删除。 如果将行标记为删除，则该行的 <xref:System.Data.DataRow.RowState%2A> 属性会设置为 <xref:System.Data.DataRow.Delete%2A>。  
  
 在 foreach 循环中，不会调用 <xref:System.Data.DataRow.Delete%2A> 和 <xref:System.Data.DataRowCollection.Remove%2A>，而是循环访问 <xref:System.Data.DataRowCollection> 对象。 <xref:System.Data.DataRow.Delete%2A> 和 <xref:System.Data.DataRowCollection.Remove%2A> 不会修改该集合的状态。  
  
 当将 <xref:System.Data.DataSet> 或**DataTable**与**DataAdapter**和关系数据源结合使用时，请使用**DataRow**的**Delete**方法删除该行。 **Delete**方法会将该行标记为**已**在**DataSet**或**DataTable**中删除，但不会将其删除。 相反，当 **DataAdapter** 遇到标记为 **已删除**的行时，它将执行其 **DeleteCommand** 方法以删除数据源中的行。 然后，可以使用 **AcceptChanges** 方法永久删除该行。 如果使用 " **删除** " 来删除该行，则会完全从表中删除行，但 **DataAdapter** 不会删除数据源中的行。  
  
 **DataRowCollection**的**Remove**方法采用**DataRow**作为参数，并将其从集合中移除，如下例所示。  
  
```vb  
workTable.Rows.Remove(workRow)  
```  
  
```csharp  
workTable.Rows.Remove(workRow);  
```  
  
 与此相反，下面的示例演示如何对**DataRow**调用**Delete**方法，以将其**RowState**更改为 "**已删除**"。  
  
```vb  
workRow.Delete  
```  
  
```csharp  
workRow.Delete();  
```  
  
 如果将某行标记为删除，并且调用**datatable**对象的**AcceptChanges**方法，则将从**datatable**中删除该行。 与此相反，如果调用 **RejectChanges**，则行的 **RowState** 会恢复为标记为 **已删除**之前的内容。  
  
> [!NOTE]
> 如果**添加**了**DataRow**的**RowState** ，这意味着它刚刚添加到表中，然后将其标记为**已删除**，则会将其从表中删除。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Data.DataRow>
- <xref:System.Data.DataRowCollection>
- <xref:System.Data.DataTable>
- [操作数据表中的数据](manipulating-data-in-a-datatable.md)
- [ADO.NET 概述](../ado-net-overview.md)
