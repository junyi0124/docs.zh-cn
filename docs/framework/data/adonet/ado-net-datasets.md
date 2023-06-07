---
title: DataSets
description: 了解数据集，它是一种内存驻留数据表示形式，它提供一致的关系编程模型，而不考虑 ADO.NET 中的数据源。
ms.date: 03/30/2017
ms.assetid: 82b641bb-6001-4512-bf1a-2830acdd92ab
ms.openlocfilehash: d78918773c3cc88d8a925788d81cdafdc0a2db27
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91153581"
---
# <a name="adonet-datasets"></a>ADO.NET 数据集

<xref:System.Data.DataSet>对象是支持通过 ADO.NET 断开连接的分布式数据方案的核心。 数据 **集** 是数据的内存驻留表示形式，可提供一致的关系编程模型，而不考虑数据源。 它可以用于多种不同的数据源，用于 XML 数据，或用于管理应用程序本地的数据。 **数据集**表示一组完整的数据，包括相关表、约束和表之间的关系。 下图显示了 **数据集** 对象模型。  
  
 ![ADO.Net 图](./media/ado-1-bpuedev11.png "ado_1_bpuedev11")  
DataSet 对象模型  
  
 **数据集中**的方法和对象与关系数据库模型中的方法和对象一致。  
  
 **数据集**还可以将其内容保存并重新加载为 xml，并将其架构作为 xml 架构定义语言 (XSD) 架构。 有关详细信息，请参阅[在数据集中使用 XML](./dataset-datatable-dataview/using-xml-in-a-dataset.md)。  
  
## <a name="the-datatablecollection"></a>DataTableCollection  

 ADO.NET **数据集** 包含由对象表示的零个或多个表的集合 <xref:System.Data.DataTable> 。 <xref:System.Data.DataTableCollection>包含**数据集中**的所有**DataTable**对象。  
  
 **DataTable**在命名空间中定义 <xref:System.Data> ，表示内存驻留数据的单个表。 其中包含由 <xref:System.Data.DataColumnCollection> 表示的列集合以及由 <xref:System.Data.ConstraintCollection> 表示的约束集合，这两个集合共同定义表的架构。 **DataTable**还包含由表示的行的集合 <xref:System.Data.DataRowCollection> ，其中包含表中的数据。 除了其当前状态之前，<xref:System.Data.DataRow> 还会保留其当前版本和初始版本，以标识对行中存储的值的更改。  
  
## <a name="the-dataview-class"></a>DataView 类  

 您可以利用 <xref:System.Data.DataView> 创建存储在 <xref:System.Data.DataTable>（一种通常在数据绑定应用程序中使用的功能）中的数据的不同视图。 通过使用 <xref:System.Data.DataView>，您可以使用不同的排序顺序公开表中的数据，并且可以按行状态或基于筛选器表达式来筛选数据。 有关详细信息，请参阅 [dataview](./dataset-datatable-dataview/dataviews.md)。  
  
## <a name="the-datarelationcollection"></a>DataRelationCollection  

 **数据集**在其对象中包含关系 <xref:System.Data.DataRelationCollection> 。 由对象表示的关系将 <xref:System.Data.DataRelation> 一个 **datatable** 中的行与另一个 **datatable**中的行相关联。 关系类似于可能存在于关系数据库中的主键列和外键列之间的联接路径。 **DataRelation**标识**数据集**的两个表中的匹配列。  
  
 关系允许在 **数据集中**从一个表导航到另一个表。 **DataRelation**的基本元素为关系的名称、相关表的名称以及每个表中的相关列。 关系可以通过一个表的多个列来生成，方法是将一组 <xref:System.Data.DataColumn> 对象指定为键列。 向添加关系时 <xref:System.Data.DataRelationCollection> ，可以根据需要添加 **UniqueKeyConstraint** 和 **ForeignKeyConstraint** ，以在对相关列值进行更改时强制实施完整性约束。  
  
 有关详细信息，请参阅 [添加 datarelation](./dataset-datatable-dataview/adding-datarelations.md)。  
  
## <a name="xml"></a>XML  

 您可以从 XML 流或文档填充 **数据集** 。 可以使用 XML 流或文档向数据 **集** 提供数据和/或架构信息。 从 XML 流或文档中提供的信息可以与 **数据集中**已存在的现有数据或架构信息组合在一起。 有关详细信息，请参阅[在数据集中使用 XML](./dataset-datatable-dataview/using-xml-in-a-dataset.md)。  
  
## <a name="extendedproperties"></a>ExtendedProperties  

 **数据集**、 **DataTable**和**DataColumn**都具有**ExtendedProperties**属性。 **ExtendedProperties** 是一个 **PropertyCollection** ，您可以在其中放置自定义信息，例如用于生成结果集的 SELECT 语句或生成数据的时间。 **ExtendedProperties**集合随**数据集**的架构信息一起保存。  
  
## <a name="linq-to-dataset"></a>LINQ to DataSet  

 LINQ to DataSet 为数据集中存储的已断开连接的数据提供语言集成查询功能。 LINQ to DataSet 使用标准 LINQ 语法，并在使用 Visual Studio IDE 时提供编译时语法检查、静态类型和 IntelliSense 支持。  
  
 有关详细信息，请参阅 [LINQ to DataSet](linq-to-dataset.md)。  
  
## <a name="see-also"></a>请参阅

- [ADO.NET 概述](ado-net-overview.md)
- [数据集、数据表和数据视图](./dataset-datatable-dataview/index.md)
- [在 ADO.NET 中检索和修改数据](retrieving-and-modifying-data.md)
