---
title: 添加 DataRelation
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: a4a564fb-c1c4-4135-b6c2-b030e51195e4
ms.openlocfilehash: 5fe2bd45e0abada1f9ec7071e3863da853479b51
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91202378"
---
# <a name="adding-datarelations"></a>添加 DataRelation

在包含多个 <xref:System.Data.DataSet> 对象的 <xref:System.Data.DataTable> 中，可以使用 <xref:System.Data.DataRelation> 对象来使一个表与另一个表相关，在多个表之间导航，以及从相关表中返回子行或父行。  
  
 创建 **datarelation** 所需的参数是要创建的 **datarelation** 的名称以及 <xref:System.Data.DataColumn> 对用作关系中父列和子列的列的一个或多个引用的数组。 创建 **DataRelation**后，可以使用它在表之间导航和检索值。  
  
 默认情况下，将 **DataRelation** 添加到将向 <xref:System.Data.DataSet> <xref:System.Data.UniqueConstraint> 父表和 <xref:System.Data.ForeignKeyConstraint> 子表添加。 有关这些默认约束的详细信息，请参阅 [DataTable 约束](datatable-constraints.md)。  
  
 下面的代码示例使用中的两个对象创建 **DataRelation** <xref:System.Data.DataTable> <xref:System.Data.DataSet> 。 每个都 <xref:System.Data.DataTable> 包含一个名为 **CustID**的列，它用作两个对象之间的链接 <xref:System.Data.DataTable> 。 该示例将一个 **DataRelation** 添加到的 **关系** 集合 <xref:System.Data.DataSet> 。 该示例中的第一个参数指定所创建的 **DataRelation** 的名称。 第二个参数设置父 **datacolumn** ，第三个参数设置子 **datacolumn**。  
  
```vb  
customerOrders.Relations.Add("CustOrders", _  
  customerOrders.Tables("Customers").Columns("CustID"), _  
  customerOrders.Tables("Orders").Columns("CustID"))  
```  
  
```csharp  
customerOrders.Relations.Add("CustOrders",  
  customerOrders.Tables["Customers"].Columns["CustID"],  
  customerOrders.Tables["Orders"].Columns["CustID"]);  
```  
  
 **DataRelation**还具有一个**嵌套**属性，当设置为**true**时，将导致子表中的行嵌套在使用编写为 XML 元素的父表中的关联行内 <xref:System.Data.DataSet.WriteXml%2A> 。 有关详细信息，请参阅[在数据集中使用 XML](using-xml-in-a-dataset.md)。  
  
## <a name="see-also"></a>请参阅

- [数据集、数据表和数据视图](index.md)
- [ADO.NET 概述](../ado-net-overview.md)
