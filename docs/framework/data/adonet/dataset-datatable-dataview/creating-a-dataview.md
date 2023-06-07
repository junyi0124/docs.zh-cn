---
title: 创建数据视图
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: b1cc02d1-23b1-4439-a998-0da1899f3442
ms.openlocfilehash: 539e9763c8aa4affdb6f3bd219a99dbca50cee01
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91202338"
---
# <a name="creating-a-dataview"></a>创建数据视图

创建 <xref:System.Data.DataView> 的方法有两种。 您可以使用 **DataView** 构造函数，也可以创建对的属性的引用 <xref:System.Data.DataTable.DefaultView%2A> <xref:System.Data.DataTable> 。 **DataView**构造函数可以为空，也可以采用**datatable**作为单个参数，或将**datatable**与筛选条件、排序条件和行状态筛选器一起使用。 有关可用于 **DataView**的其他参数的详细信息，请参阅对 [数据进行排序和筛选](sorting-and-filtering-data.md)。  
  
 由于在创建**dataview**时以及在修改任何**sort**、 **RowFilter**或**RowStateFilter**属性时都会生成**dataview**的索引，因此在创建**dataview**时，通过将任何初始排序顺序或筛选条件作为构造函数参数提供，可以获得最佳性能。 如果在不指定排序或筛选条件的情况下创建 **DataView** ，然后设置 **sort**、 **RowFilter**或 **RowStateFilter** 属性，稍后将导致索引至少生成两次：创建 **dataview** 时，以及在修改任何排序或筛选属性时。  
  
 请注意，如果使用不采用任何参数的构造函数来创建**dataview** ，则在设置**表**属性之前，将无法使用**dataview** 。  
  
 下面的代码示例演示如何使用**dataview**构造函数创建**dataview** 。 **RowFilter**、 **Sort**列和**DataViewRowState**随**DataTable**一起提供。  
  
```vb  
Dim custDV As DataView = New DataView(custDS.Tables("Customers"), _  
    "Country = 'USA'", _  
    "ContactName", _  
    DataViewRowState.CurrentRows)  
```  
  
```csharp  
DataView custDV = new DataView(custDS.Tables["Customers"],
    "Country = 'USA'",
    "ContactName",
    DataViewRowState.CurrentRows);  
```  
  
 下面的代码示例演示如何使用表的**DefaultView**属性获取对**DataTable**的默认**DataView**的引用。  
  
```vb  
Dim custDV As DataView = custDS.Tables("Customers").DefaultView  
```  
  
```csharp  
DataView custDV = custDS.Tables["Customers"].DefaultView;  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Data.DataTable>
- <xref:System.Data.DataView>
- [DataView](dataviews.md)
- [对数据进行排序和筛选](sorting-and-filtering-data.md)
- [DataTables](datatables.md)
- [ADO.NET 概述](../ado-net-overview.md)
