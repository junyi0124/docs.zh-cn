---
title: 创建 AutoIncrement 列
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: cf09732a-ab54-4d98-89e2-4d0a1f28fbce
ms.openlocfilehash: 9a979f39003e60c70c03206bd886bdd6827c82e6
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203717"
---
# <a name="creating-autoincrement-columns"></a>创建 AutoIncrement 列

要确保列值的唯一，可将列值设置为在表中添加新行时自动递增。 若要创建自动增量 <xref:System.Data.DataColumn> ，请将 <xref:System.Data.DataColumn.AutoIncrement%2A> 列的属性设置为 **true**。 然后，在 <xref:System.Data.DataColumn> 属性中定义的值开始 <xref:System.Data.DataColumn.AutoIncrementSeed%2A> ，每添加一行， **自动增量** 列的值将按列的属性中定义的值增加 <xref:System.Data.DataColumn.AutoIncrementStep%2A> 。  
  
 对于 **自动增量** 列，建议 <xref:System.Data.DataColumn.ReadOnly%2A> 将 **DataColumn** 的属性设置为 **true**。  
  
 以下示例演示了如何创建从值 200 开始并以 3 为增量递增的列。  
  
```vb  
Dim workColumn As DataColumn = workTable.Columns.Add( _  
    "CustomerID", typeof(Int32))  
workColumn.AutoIncrement = true  
workColumn.AutoIncrementSeed = 200  
workColumn.AutoIncrementStep = 3  
```  
  
```csharp  
DataColumn workColumn = workTable.Columns.Add(  
    "CustomerID", typeof(Int32));  
workColumn.AutoIncrement = true;  
workColumn.AutoIncrementSeed = 200;  
workColumn.AutoIncrementStep = 3;  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Data.DataColumn>
- [数据表架构定义](datatable-schema-definition.md)
- [DataTables](datatables.md)
- [ADO.NET 概述](../ado-net-overview.md)
