---
title: 导航 DataRelation
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: e5e673f4-9b44-45ae-aaea-c504d1cc5d3e
ms.openlocfilehash: 5eb2ee16712be5ccd5e9aa0af4dde22dcaaeea09
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91148381"
---
# <a name="navigating-datarelations"></a>导航 DataRelation

<xref:System.Data.DataRelation> 的一项主要功能就是在 <xref:System.Data.DataTable> 中从一个 <xref:System.Data.DataSet> 浏览到另一个。 <xref:System.Data.DataRow>当给定相关**DataTable**中的单个**DataRow**时，这允许您检索一个**DataTable**中的所有相关对象。 例如，在客户表和订单表之间建立 **DataRelation** 后，可以使用 **GetChildRows**检索特定客户行的所有订单行。  
  
 下面的代码示例在**Customers**表和**数据集**的**Orders**表之间创建**DataRelation** ，并返回每个客户的所有订单。  
  
 [!code-csharp[DataWorks Data.DataTableRelation#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks Data.DataTableRelation/CS/source.cs#1)]
 [!code-vb[DataWorks Data.DataTableRelation#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks Data.DataTableRelation/VB/source.vb#1)]  
  
 下一示例以上例为基础，将四个表关联在一起，并浏览这些关系。 如前面的示例所示， **CustomerID** 将 **Customers** 表与 **Orders** 表相关联。 对于 **Customers** 表中的每个客户，将确定 **orders** 表中的所有子行，以便返回特定客户具有的订单数及其 **订单** 值。  
  
 展开的示例还将返回 **OrderDetails** 和 **Products** 表中的值。 **Orders**表与**OrderDetails**表相关，使用 "订单**id** " 来确定对于每个客户订单，订购了哪些产品和数量。 由于**OrderDetails**表只包含已订购产品的**ProductID** ，因此**OrderDetails**与使用**ProductID**的**产品**相关，以便返回产品**名称**。 在此关系中， **Products** 表是父级， **Order Details** 表是子表。 因此，在循环访问 **OrderDetails** 表时，将调用 **GetParentRow** 来检索相关的 **ProductName** 值。  
  
 请注意，当为**Customers**和**Orders**表创建**DataRelation**时，没有为**createConstraints**标志指定值 (默认值为**true**) 。 这假设**Orders**表中的所有行都有一个存在于父**Customers**表中的**CustomerID**值。 如果**客户**表中不存在的**Orders**表中存在**CustomerID** ，则 <xref:System.Data.ForeignKeyConstraint> 会导致引发异常。  
  
 如果子列可能包含父列不包含的值，请在添加**DataRelation**时将**createConstraints**标志设置为**false** 。 在此示例中， **createConstraints**标志在**Orders**表和**OrderDetails**表之间的**DataRelation**设置为**false** 。 这样，应用程序便可以返回 **OrderDetails** 表中的所有记录，并只返回 **Orders** 表中记录的子集，而无需生成运行时异常。 该扩展示例生成以下格式的输出。  
  
```output  
Customer ID: NORTS  
  Order ID: 10517  
        Order Date: 4/24/1997 12:00:00 AM  
           Product: Filo Mix  
          Quantity: 6  
           Product: Raclette Courdavault  
          Quantity: 4  
           Product: Outback Lager  
          Quantity: 6  
  Order ID: 11057  
        Order Date: 4/29/1998 12:00:00 AM  
           Product: Outback Lager  
          Quantity: 3  
```  
  
 下面的代码示例是一个展开的示例，在该示例中，将返回 **OrderDetails** 表和 **Products** 表中的值，并只返回 **Orders** 表中记录的子集。  
  
 [!code-csharp[DataWorks Data.DataTableNavigation#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks Data.DataTableNavigation/CS/source.cs#1)]
 [!code-vb[DataWorks Data.DataTableNavigation#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks Data.DataTableNavigation/VB/source.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [数据集、数据表和数据视图](index.md)
- [ADO.NET 概述](../ado-net-overview.md)
