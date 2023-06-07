---
title: DataTableReader
ms.date: 03/30/2017
ms.assetid: 97546ae2-0e42-4d26-961d-e0b244d81ded
ms.openlocfilehash: 911a45dad3d5d82ab5e5752b1c12f7d8a6ab1563
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91202313"
---
# <a name="datatablereaders"></a>DataTableReader

<xref:System.Data.DataTableReader> 以一个或多个只读、只进结果集的形式提供 <xref:System.Data.DataTable> 或的 <xref:System.Data.DataSet> 的内容。  
  
 从**DataTable**创建**DataTableReader**时，生成的**DataTableReader**对象包含一个结果集，该结果集的数据与创建它时所用的**数据表**相同，但已标记为已删除的任何行除外。 列的显示顺序与原始 **DataTable**中的顺序相同。  
  
 如果 **DataTableReader** 是通过调用创建的，则可以包含多个结果集 <xref:System.Data.DataSet.CreateDataReader%2A> 。 结果与**DataSet**对象的集合中的**数据表**顺序相同 <xref:System.Data.DataSet.Tables%2A> 。  
  
## <a name="in-this-section"></a>本节内容  

 [创建 DataReader](creating-a-datareader.md)  
 讨论如何创建 **DataTableReader** 对象。  
  
 [导航数据表](navigating-datatables.md)  
 介绍如何使用 **Read** 方法在 **DataTableReader**的内容中移动。  
  
## <a name="see-also"></a>请参阅

- [在 ADO.NET 中检索和修改数据](../retrieving-and-modifying-data.md)
- [ADO.NET 概述](../ado-net-overview.md)
