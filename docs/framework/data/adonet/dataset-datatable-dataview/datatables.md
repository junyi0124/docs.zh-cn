---
title: DataTables
description: 了解一个 ADO.NET DataTable，它代表中的一个内存中关系数据表。基于网络的应用程序所在位置。
ms.date: 03/30/2017
ms.assetid: 52ff0e32-3e5a-41de-9a3b-7b04ea52b83e
ms.openlocfilehash: d501096b4abe94653acdc5249c120abff94534d1
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91202300"
---
# <a name="datatables"></a>DataTables

<xref:System.Data.DataSet> 由表、关系和约束的集合组成。 在 ADO.NET 中， <xref:System.Data.DataTable> 对象用于表示 **数据集中**的表。 **DataTable**表示内存中关系数据的一个表;数据是的本地数据。它所在的基于网络的应用程序，但可以使用**DataAdapter**从数据源（例如 Microsoft SQL Server）进行填充。有关详细信息，请参阅[从 DataAdapter 填充数据集](../populating-a-dataset-from-a-dataadapter.md)。  
  
 **DataTable**类是 .NET Framework 类库中的**system.object**命名空间的成员。 您可以独立创建和使用 **datatable** ，或将其作为 **数据集**的成员，也可以将 **datatable** 对象与其他 .NET Framework 对象结合使用，其中包括 <xref:System.Data.DataView> 。 通过**dataset**对象的**Tables**属性访问**数据集中**的表的集合。  
  
 表的架构或结构由列和约束表示。 使用对象以及和对象定义 **DataTable** 的架构 <xref:System.Data.DataColumn> <xref:System.Data.ForeignKeyConstraint> <xref:System.Data.UniqueConstraint> 。 表中的列可以映射到数据源中的列、包含从表达式计算所得的值、自动递增它们的值，或包含主键值。  
  
 除了架构以外， **DataTable** 还必须有行来包含和排序数据。 <xref:System.Data.DataRow> 类表示表中包含的实际数据。 使用 **DataRow** 及其属性和方法可以检索、计算和处理表中的数据。 访问和更改行中的数据时， **DataRow** 对象将同时维护其当前状态和原始状态。  
  
 您可以使用表中的一个或多个相关的列来创建表与表之间的父子关系。 使用创建 **DataTable** 对象之间的关系 <xref:System.Data.DataRelation> 。 然后，可以使用**DataRelation**对象返回特定行的相关子行或父行。 有关详细信息，请参阅 [添加 datarelation](adding-datarelations.md)。  
  
## <a name="in-this-section"></a>本节内容  

 [创建数据表](creating-a-datatable.md)  
 说明如何创建 **DataTable** 并将其添加到 **数据集**。  
  
 [数据表架构定义](datatable-schema-definition.md)  
 提供有关创建和使用 **DataColumn** 对象和约束的信息。  
  
 [操作数据表中的数据](manipulating-data-in-a-datatable.md)  
 说明如何添加、修改和删除表中的数据。 说明如何使用 **DataTable** 事件来检查对表中数据的更改。  
  
 [处理数据表事件](handling-datatable-events.md)  
 提供可用于 **DataTable**的事件的相关信息，包括修改列值和添加或删除行时的事件。  
  
## <a name="related-sections"></a>相关章节  

 [ADO.NET](../index.md)  
 描述 ADO.NET 结构和组件，并说明如何用来访问现有的数据源和管理应用程序数据。  
  
 [数据集、数据表和数据视图](index.md)  
 提供有关 ADO.NET **数据集** 的信息，包括如何创建表之间的关系。  
  
 <xref:System.Data.Constraint>  
 提供有关 **约束** 对象的参考信息。  
  
 <xref:System.Data.DataColumn>  
 提供有关 **DataColumn** 对象的参考信息。  
  
 <xref:System.Data.DataSet>  
 提供有关 **DataSet** 对象的参考信息。  
  
 <xref:System.Data.DataTable>  
 提供有关 **DataTable** 对象的参考信息。  
  
 [类库概述](../../../../standard/class-library-overview.md)  
 提供 .NET Framework 类库的概述，包括 **System** 命名空间及其第二级命名空间、 **system.web**。  
  
## <a name="see-also"></a>请参阅

- [ADO.NET 概述](../ado-net-overview.md)
