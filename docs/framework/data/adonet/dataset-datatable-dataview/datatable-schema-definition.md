---
title: 数据表架构定义
ms.date: 03/30/2017
ms.assetid: efbcdda4-f5a9-421d-8be2-4c194c74552f
ms.openlocfilehash: 0c14c6012c65039e1e2e7e2d2d8c38c4202b45a7
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91153243"
---
# <a name="datatable-schema-definition"></a>数据表架构定义

表的架构（即结构）由列和约束表示。 使用 <xref:System.Data.DataTable> 对象以及 <xref:System.Data.DataColumn> 和 <xref:System.Data.ForeignKeyConstraint> 对象定义 <xref:System.Data.UniqueConstraint> 的架构。 表中的列可以映射到数据源中的列、包含从表达式计算所得的值、自动递增它们的值，或包含主键值。  
  
 按名称引用表中的列、关系和约束是区分大小写的。 因此，一个表中可以存在两个或两个以上名称相同（但大小写不同）的列、关系或约束。 例如，您可以具有 **col1** 和 **col1**。 在这种情况下，按名称引用某一列就必须完全符合该列名的大小写，否则会引发异常。 例如，如果表 **myTable** 包含列 **Col1** 和 **col1**，则可以按名称将 **col1** 称为 **列 ["col1"]**，将 **col1** 引用为 mytable 列 **["col1"]**。 尝试将任一列作为 **myTable 引用。列 ["COL1"]** 将生成异常。  
  
 如果某个特定名称只存在一个列、关系或约束，则不应用区分大小写规则。 也就是说，如果表中没有其他的列、关系或约束对象与该特定列、关系或约束对象的名称匹配，您就可以使用任意的大小写来按名称引用该对象，并且不会引发异常。 例如，如果表只有 **Col1**，则可以使用 my 来引用它 **。列 ["COL1"]**。  
  
> [!NOTE]
> <xref:System.Data.DataTable.CaseSensitive%2A> **DataTable**的属性不影响此行为。 **CaseSensitive**属性应用于表中的数据，并影响排序、搜索、筛选、强制约束等，但不影响对列、关系和约束的引用。  
  
## <a name="in-this-section"></a>本节内容  

 [向数据表添加列](adding-columns-to-a-datatable.md)  
 介绍如何使用 **DataColumn** 对象定义表中的列。  
  
 [创建表达式列](creating-expression-columns.md)  
 介绍如何使用列的 **Expression** 属性根据行中的其他列中的值计算值。  
  
 [创建 AutoIncrement 列](creating-autoincrement-columns.md)  
 说明如何将列设置为自动递增数值，以确保每行都有唯一的列值。  
  
 [定义主键](defining-primary-keys.md)  
 描述如何从一个或多个 **DataColumn** 对象中指定表的主键。  
  
 [数据表约束](datatable-constraints.md)  
 说明如何为表中的列定义外键和唯一约束。  
  
## <a name="see-also"></a>请参阅

- [DataTables](datatables.md)
- [ADO.NET 概述](../ado-net-overview.md)
