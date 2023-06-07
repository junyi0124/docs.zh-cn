---
title: 对数据进行排序和筛选
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: fdd9c753-39df-48cd-9822-2781afe76200
ms.openlocfilehash: 89e2fdf656fb06ee545ba936f033646ad86182d4
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91183372"
---
# <a name="sorting-and-filtering-data"></a>对数据进行排序和筛选

<xref:System.Data.DataView> 为在 <xref:System.Data.DataTable> 中对数据排序和筛选提供了多种方法：  
  
- 可以使用 <xref:System.Data.DataView.Sort%2A> 属性指定单个或多个列排序顺序并包含 ASC（升序）和 DESC（降序）参数。  
  
- 可以使用 <xref:System.Data.DataView.ApplyDefaultSort%2A> 属性自动以升序创建基于表的一个或多个主键列的排序顺序。 <xref:System.Data.DataView.ApplyDefaultSort%2A> 仅当 **Sort** 属性为空引用或空字符串时以及表已定义主键时，才适用。  
  
- 可以使用 <xref:System.Data.DataView.RowFilter%2A> 属性根据行的列值来指定行的子集。 有关 **RowFilter** 属性的有效表达式的详细信息，请参阅类的属性的参考信息 <xref:System.Data.DataColumn.Expression%2A> <xref:System.Data.DataColumn> 。  
  
     如果要返回数据的特定查询的结果，而不是提供数据子集的动态视图，请使用 <xref:System.Data.DataView.Find%2A> DataView 的或 <xref:System.Data.DataView.FindRows%2A> 方法来实现最佳性能，而不是设置**DataView** **RowFilter**属性。 设置 **RowFilter** 属性会重新生成数据的索引，从而增加应用程序的系统开销并降低性能。 在绑定控件显示筛选结果的数据绑定应用程序中，最好使用 **RowFilter** 属性。 **Find**和**FindRows**方法利用当前索引，而无需重新生成索引。 有关 **Find** 和 **FindRows** 方法的详细信息，请参阅 [查找行](finding-rows.md)。  
  
- 可以使用 <xref:System.Data.DataView.RowStateFilter%2A> 属性指定要查看的行版本。 **DataView**根据基础行的**RowState**隐式地管理要公开的行版本。 例如，如果**RowStateFilter**设置为**DataViewRowState**，则**DataView**将公开所有**已删除**行的**原始**行版本，因为没有**当前**行版本。 您可以使用**DataRowView**的**RowVersion**属性来确定要公开行的哪个行版本。  
  
     下表显示了 **DataViewRowState**的选项。  
  
    |DataViewRowState 选项|描述|  
    |------------------------------|-----------------|  
    |**CurrentRows**|所有**未更改**的、**已添加**的行和**已修改**的行的**当前**行版本。 这是默认设置。|  
    |**已添加**|所有**已添加**行的**当前**行版本。|  
    |**已删除**|所有**已删除**行的**原始**行版本。|  
    |**ModifiedCurrent**|所有**已修改**行的**当前**行版本。|  
    |**ModifiedOriginal**|所有**已修改**行的**原始**行版本。|  
    |无|没有行。|  
    |**OriginalRows**|所有**未更改**、**已修改**和**已删除**行的**原始**行版本。|  
    |**不变**|所有**未更改**的行的**当前**行版本。|  
  
 有关行状态和行版本的详细信息，请参阅 [行状态和行版本](row-states-and-row-versions.md)。  
  
 以下代码示例创建一个视图，该视图显示所有库存量小于或等于再订购量的产品，这些产品首先按供应商 ID 排序，然后按产品名称排序。  
  
```vb  
Dim prodView As DataView = New DataView(prodDS.Tables("Products"), _  
   "UnitsInStock <= ReorderLevel", _  
   "SupplierID, ProductName", _  
   DataViewRowState.CurrentRows)  
```  
  
```csharp  
DataView prodView = new DataView(prodDS.Tables["Products"],  
   "UnitsInStock <= ReorderLevel",  
   "SupplierID, ProductName",  
   DataViewRowState.CurrentRows);  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Data.DataViewRowState>
- <xref:System.Data.DataColumn.Expression%2A?displayProperty=nameWithType>
- <xref:System.Data.DataTable>
- <xref:System.Data.DataView>
- [DataView](dataviews.md)
- [ADO.NET 概述](../ado-net-overview.md)
