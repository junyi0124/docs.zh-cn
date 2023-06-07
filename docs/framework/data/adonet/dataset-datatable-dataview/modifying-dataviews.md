---
title: 修改 DataView
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 697a3991-b660-4a5a-8a54-1a2304ff158e
ms.openlocfilehash: 8e3a3f92fe8ecc94a041fbcb1540bae18a41dbef
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203678"
---
# <a name="modifying-dataviews"></a>修改 DataView

可以使用 <xref:System.Data.DataView> 在基础表中添加、删除或修改数据行。 使用 **dataview** 修改基础表中数据的功能是通过设置 **dataview**的三个布尔属性之一来控制的。 这三个属性是 <xref:System.Data.DataView.AllowNew%2A>、<xref:System.Data.DataView.AllowEdit%2A> 和 <xref:System.Data.DataView.AllowDelete%2A>。 默认情况下，它们设置为 **true** 。  
  
 如果 **AllowNew** 为 **true**，则可以使用 <xref:System.Data.DataView.AddNew%2A> **DataView** 的方法来创建新的 <xref:System.Data.DataRowView> 。 请注意，在 <xref:System.Data.DataTable> <xref:System.Data.DataRowView.EndEdit%2A> 调用 **DataRowView** 的方法之前，新行实际上不会添加到基础中。 如果 <xref:System.Data.DataRowView.CancelEdit%2A> 调用 **DataRowView** 的方法，则将丢弃新行。 另请注意，一次只能编辑一个 **DataRowView** 。 如果在存在挂起行时调用**DataRowView**的**AddNew**或**BeginEdit**方法，则会对挂起行隐式调用**EndEdit** 。 调用**EndEdit**时，所做的更改将应用于基础**DataTable** ，以后可以使用**DataTable**、 **DataSet**或**DataRow**对象的**AcceptChanges**或**RejectChanges**方法来提交或拒绝这些更改。 如果**AllowNew**为**false**，则当调用**DataRowView**的**AddNew**方法时，将引发异常。  
  
 如果**AllowEdit**为**true**，则可以通过**DataRowView**修改**DataRow**的内容。 您可以使用 **DataRowView** 确认对基础行所做的更改，或使用 **DataRowView**拒绝更改。 注意，一次只能编辑一行。 如果在存在挂起行时调用**DataRowView**的**AddNew**或**BeginEdit**方法，则会对挂起行隐式调用**EndEdit** 。 调用**EndEdit**时，建议的更改将放置在基础**DataRow**的**当前**行版本中，以后可以使用**DataTable**、 **DataSet**或**DataRow**对象的**AcceptChanges**或**RejectChanges**方法来提交或拒绝建议的更改。 如果 **AllowEdit** 为 **false**，则当你尝试修改 **DataView**中的值时，将引发异常。  
  
 正在编辑现有的 **DataRowView** 时，仍将使用建议的更改引发基础 **DataTable** 的事件。 请注意，如果对基础**DataRow**调用**EndEdit**或**CancelEdit** ，则将应用或取消挂起的更改，而不考虑是否在**DataRowView**上调用**EndEdit**或**CancelEdit** 。  
  
 如果**AllowDelete**为**true**，则可以使用**Dataview**或**DataRowView**对象的**delete**方法从**dataview**删除行，并从基础**DataTable**中删除这些行。 稍后可以分别使用 **AcceptChanges** 或 **RejectChanges** 提交或拒绝删除。 如果**AllowDelete**为**false**，则在调用**DataView**或**DataRowView**的**Delete**方法时，将引发异常。  
  
 下面的代码示例禁止使用 **dataview** 删除行并使用 **dataview**向基础表中添加新行。  
  
```vb  
Dim custTable As DataTable = custDS.Tables("Customers")  
Dim custView As DataView = custTable.DefaultView  
custView.Sort = "CompanyName"  
  
custView.AllowDelete = False  
  
Dim newDRV As DataRowView = custView.AddNew()  
newDRV("CustomerID") = "ABCDE"  
newDRV("CompanyName") = "ABC Products"  
newDRV.EndEdit()  
```  
  
```csharp  
DataTable custTable = custDS.Tables["Customers"];  
DataView custView = custTable.DefaultView;  
custView.Sort = "CompanyName";  
  
custView.AllowDelete = false;  
  
DataRowView newDRV = custView.AddNew();  
newDRV["CustomerID"] = "ABCDE";  
newDRV["CompanyName"] = "ABC Products";  
newDRV.EndEdit();  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Data.DataTable>
- <xref:System.Data.DataView>
- <xref:System.Data.DataRowView>
- [DataView](dataviews.md)
- [ADO.NET 概述](../ado-net-overview.md)
