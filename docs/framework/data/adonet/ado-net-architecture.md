---
title: 体系结构
description: 了解 ADO.NET 体系结构，其中包括用于访问和操作数据的两个主要组件： .NET Framework 数据提供程序和数据集。
ms.date: 03/30/2017
ms.assetid: fcd45b99-ae8f-45ab-8b97-d887beda734e
ms.openlocfilehash: 8d45c1d0b33f3e7bf86a42561bfaa13772aa5a31
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91166906"
---
# <a name="adonet-architecture"></a>ADO.NET 体系结构

以前，数据处理主要依赖于基于连接的双层模型。 随着数据处理越来越多地使用多层体系结构，程序员正在向断开方法转换，以便为他们的应用程序提供更好的可伸缩性。  
  
## <a name="adonet-components"></a>ADO.NET 组件  

 用于访问和操作数据的 ADO.NET 的两个主要组件是 .NET Framework 的数据访问接口和 <xref:System.Data.DataSet> 。  
  
### <a name="net-framework-data-providers"></a>.NET Framework 数据提供程序  

 .NET Framework 数据提供程序是专门为数据操作以及快速、只进、只读访问数据而设计的组件。 `Connection` 对象提供到数据源的连接。 使用 `Command` 对象可以访问用于返回数据、修改数据、运行存储过程以及发送或检索参数信息的数据库命令。 `DataReader` 可从数据源提供高性能的数据流。 最后，`DataAdapter` 在 `DataSet` 对象和数据源之间起到桥梁作用。 `DataAdapter` 使用 `Command` 对象在数据源中执行 SQL 命令以向 `DataSet` 中加载数据，并将对 `DataSet` 中数据的更改协调回数据源。 有关详细信息，请参阅 [.NET Framework 数据访问接口](data-providers.md) 和 [检索和修改 ADO.NET 中的数据](retrieving-and-modifying-data.md)。  
  
### <a name="the-dataset"></a>DataSet  

 ADO.NET `DataSet` 是专门为独立于任何数据源的数据访问而设计的。 因此，它可以用于多种不同的数据源，用于 XML 数据，或用于管理应用程序本地的数据。 `DataSet` 包含一个或多个 <xref:System.Data.DataTable> 对象的集合，这些对象由数据行和数据列以及有关 `DataTable` 对象中数据的主键、外键、约束和关系信息组成。 有关详细信息，请参阅 [数据集、数据表和 dataview](./dataset-datatable-dataview/index.md)。  
  
 下图说明了 .NET Framework 数据访问接口与之间的关系 `DataSet` 。  
  
 ![ADO.Net 图](./media/ado-1-bpuedev11.png "ado_1_bpuedev11")  
ADO.NET 体系结构  
  
### <a name="choosing-a-datareader-or-a-dataset"></a>选择 DataReader 或 DataSet  

 如果确定应用程序是否应使用 `DataReader` (请参阅 [使用 DataReader 检索数据](retrieving-data-using-a-datareader.md)) 或 `DataSet` (查看数据 [集、数据表和 dataview](./dataset-datatable-dataview/index.md)) ，请考虑应用程序所需的功能类型。 使用 `DataSet` 可执行以下操作：  
  
- 在应用程序中将数据缓存在本地，以便可以对数据进行处理。 如果只需要读取查询结果，则 `DataReader` 是更好的选择。  
  
- 在层间或从 XML Web services 对数据进行远程处理。  
  
- 与数据进行动态交互，例如绑定到 Windows 窗体控件或组合并关联来自多个源的数据。  
  
- 对数据执行大量的处理，而不需要与数据源保持打开的连接，从而将该连接释放给其他客户端使用。  
  
 如果不需要 `DataSet` 所提供的功能，则可以通过使用 `DataReader` 以只进、只读方式返回数据，从而提高应用程序的性能。 尽管 `DataAdapter` 使用 `DataReader` 填充 (的内容 `DataSet` ，请参阅使用 [从 DataAdapter 填充数据集](populating-a-dataset-from-a-dataadapter.md)) 中，你可以通过使用来 `DataReader` 提高性能，因为你将保存将使用的内存 `DataSet` ，并避免创建和填充的内容所需的处理 `DataSet` 。  
  
## <a name="linq-to-dataset"></a>LINQ to DataSet  

 LINQ to DataSet 提供对在 DataSet 对象中缓存的数据的查询功能和编译时类型检查。 它使您可以使用一种 .NET Framework 开发语言（例如 C# 或 Visual Basic）来编写查询。 有关详细信息，请参阅 [LINQ to DataSet](linq-to-dataset.md)。  
  
## <a name="linq-to-sql"></a>LINQ to SQL  

 LINQ to SQL 支持查询无需使用中间概念模型即可映射到关系数据库数据结构的对象模型。 每个表均由独立的类表示，从而使对象模型与关系数据库架构紧密地耦合在一起。 LINQ to SQL 可将对象模型中的语言集成查询转换为 Transact-SQL 并将其发送到数据库以便执行。 当数据库返回结果时，LINQ to SQL 将结果转换回对象。 有关详细信息，请参阅 [LINQ to SQL](./sql/linq/index.md)。  
  
## <a name="adonet-entity-framework"></a>ADO.NET 实体框架  

 ADO.NET 实体框架专门用于让开发人员能够通过针对概念应用程序模型进行编程（而不是直接针对关系存储架构进行编程）来创建数据访问应用程序。 这样做的目的是减少面向数据的应用程序所需的编码和维护工作。 有关详细信息，请参阅 [ADO.NET 实体框架](./ef/index.md)。  
  
## <a name="wcf-data-services"></a>WCF 数据服务  

 WCF 数据服务用于在 Web 或 intranet 上部署数据服务。 这些数据将按照实体数据模型的规范组织成不同的实体和关系。 在此模型上部署的数据可通过标准的 HTTP 协议进行寻址。 有关详细信息，请参阅 [WCF Data Services 4.5](../wcf/index.md)。  
  
## <a name="xml-and-adonet"></a>XML 和 ADO.NET  

 ADO.NET 利用 XML 的强大功能来提供对数据的断开连接访问。 ADO.NET 是与 .NET Framework 中的 XML 类一起设计的：两者都是单个体系结构的组成部分。  
  
 .NET Framework 聚合在对象中的 ADO.NET 和中的 XML 类 `DataSet` 。 无论 XML 源是文件还是 XML 流，都可以用其中的数据来填充 `DataSet`。 无论 `DataSet` 中数据的源是什么，都可以将 `DataSet` 作为符合万维网联合会 (W3C) 的 XML 进行编写，其架构作为 XML 架构定义语言 (XSD) 架构。 由于 `DataSet` 的本机序列化格式为 XML，因此它是用于在层间移动数据的绝佳媒介，这使 `DataSet` 成为了与 XML Web 服务之间远程处理数据和架构上下文的最佳选择。 有关详细信息，请参阅 [XML 文档和数据](../../../standard/data/xml/index.md)。  
  
## <a name="see-also"></a>请参阅

- [ADO.NET 概述](ado-net-overview.md)
