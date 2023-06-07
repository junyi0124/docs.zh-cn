---
title: 数据集、数据表和数据视图
description: 了解使用 ADO.NET 数据集（提供一致的关系编程模型的数据的内存驻留表示形式）的几种方法。
ms.date: 03/30/2017
ms.assetid: 6d4c4b69-8919-4224-8a65-6cca1c61b48f
ms.openlocfilehash: 4e1c0ea5f1de1715ad8e862e6a3ed7370b53c6ce
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90555859"
---
# <a name="datasets-datatables-and-dataviews"></a>数据集、数据表和数据视图

ADO.NET <xref:System.Data.DataSet> 是数据的一种内存驻留表示形式，无论它包含的数据来自什么数据源，都会提供一致的关系编程模型。 <xref:System.Data.DataSet> 表示整个数据集，其中包含对数据进行包含、排序和约束的表以及表间的关系。  
  
使用 <xref:System.Data.DataSet> 的方法有若干种，这些方法可以单独应用，也可以结合应用。 你可以：  
  
- 以编程方式在 <xref:System.Data.DataTable> 中创建 <xref:System.Data.DataRelation>、<xref:System.Data.Constraint> 和 <xref:System.Data.DataSet>，并使用数据填充表。  
  
- 通过 <xref:System.Data.DataSet> 用现有关系数据源中的数据表填充 `DataAdapter`。  
  
- 使用 XML 加载和保持 <xref:System.Data.DataSet> 内容。 有关详细信息，请参阅[在数据集中使用 XML](using-xml-in-a-dataset.md)。  
  
强类型化的 <xref:System.Data.DataSet> 也可以使用 XML Web services 来进行传输。 <xref:System.Data.DataSet> 的设计使其成为使用 XML Web services 传输数据的理想选择。 有关 XML Web service 的概述，请参阅 [XML Web service 概述](/previous-versions/dotnet/netframework-4.0/w9fdtx28(v=vs.100))。 有关通过 XML Web service 使用 <xref:System.Data.DataSet> 的示例，请参阅[通过 XML Web service 使用数据集](consuming-a-dataset-from-an-xml-web-service.md)。  
  
## <a name="in-this-section"></a>在本节中

 [安全指南](security-guidance.md)  
 提供和的安全 <xref:System.Data.DataSet> 指南 <xref:System.Data.DataTable> 。

 [创建数据集](creating-a-dataset.md)  
 描述创建 <xref:System.Data.DataSet> 实例的语法。  
  
 [将数据表添加到数据集中](adding-a-datatable-to-a-dataset.md)  
 描述如何创建表和列并将其添加到 <xref:System.Data.DataSet> 中。  
  
 [添加 DataRelation](adding-datarelations.md)  
 描述如何创建 <xref:System.Data.DataSet> 中表之间的关系。  
  
 [导航 DataRelation](navigating-datarelations.md)  
 描述如何使用 <xref:System.Data.DataSet> 中表之间的关系来返回具有父子关系的子行或父行。  
  
 [合并数据集内容](merging-dataset-contents.md)  
 描述如何将一个 <xref:System.Data.DataSet>、<xref:System.Data.DataTable> 或 <xref:System.Data.DataRow> 数组的内容合并到另一个 <xref:System.Data.DataSet> 中。  
  
 [复制数据集内容](copying-dataset-contents.md)  
 描述如何创建可包含架构和指定数据的 <xref:System.Data.DataSet> 副本。  
  
 [处理数据集事件](handling-dataset-events.md)  
 描述 <xref:System.Data.DataSet> 的事件并说明如何使用这些事件。  
  
 [类型化数据集](typed-datasets.md)  
 描述类型化 <xref:System.Data.DataSet> 的定义并说明如何创建和使用。  
  
 [DataTables](datatables.md)  
 描述如何创建 <xref:System.Data.DataTable>、定义架构和处理数据。  
  
 [DataTableReader](datatablereaders.md)  
 描述如何创建和使用 <xref:System.Data.DataTableReader>。  
  
 [DataView](dataviews.md)  
 描述如何创建和使用 `DataViews` 以及如何使用 <xref:System.Data.DataView> 事件。  
  
 [在数据集中使用 XML](using-xml-in-a-dataset.md)  
 描述 <xref:System.Data.DataSet> 如何作为数据源与 XML 进行交互（包括以 XML 数据的形式加载和保持 <xref:System.Data.DataSet> 的内容）。  
  
 [通过 XML Web 服务使用数据集](consuming-a-dataset-from-an-xml-web-service.md)  
 描述如何创建使用 <xref:System.Data.DataSet> 来传输数据的 XML Web services。  
  
## <a name="related-sections"></a>相关章节

 [ADO.NET 新增功能](../whats-new.md)  
 介绍 ADO.NET 中的新增功能。  
  
 [ADO.NET 概述](../ado-net-overview.md)  
 提供对 ADO.NET 设计和组件的介绍。  
  
 [从 DataAdapter 填充数据集](../populating-a-dataset-from-a-dataadapter.md)  
 描述如何从数据源加载包含数据的**数据集**。  
  
 [使用 DataAdapter 更新数据源](../updating-data-sources-with-dataadapters.md)  
 描述如何将对**数据集**中的数据的更改解析回数据源。  
  
 [将现有约束添加到数据集](../adding-existing-constraints-to-a-dataset.md)  
 描述如何使用数据源中的主键信息填充**数据集**。  
  
## <a name="see-also"></a>请参阅

- [ADO.NET](../index.md)
- [ADO.NET 概述](../ado-net-overview.md)
