---
title: 概述
description: 阅读 .NET Framework 的 ADO.NET 概述，并阅读有关详细说明和示例的资源。
ms.date: 03/30/2017
ms.assetid: ee3bc1d8-11db-4be4-89eb-c708cf04117d
ms.openlocfilehash: 459e4a548a4d1358b196dc41ec495921833728d4
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91153490"
---
# <a name="adonet-overview"></a>ADO.NET 概述

ADO.NET 提供对诸如 SQL Server 和 XML 这样的数据源以及通过 OLE DB 和 ODBC 公开的数据源的一致访问。 共享数据的使用方应用程序可以使用 ADO.NET 连接到这些数据源，并可以检索、处理和更新其中包含的数据。  
  
 ADO.NET 通过数据处理将数据访问分解为多个可以单独使用或一前一后使用的不连续组件。 ADO.NET 包含用于连接到数据库、执行命令和检索结果的 .NET Framework 数据提供程序。 这些结果或者被直接处理，放在 ADO.NET <xref:System.Data.DataSet> 对象中以便以特别的方式向用户公开，并与来自多个源的数据组合；或者在层之间传递。 `DataSet` 对象也可以独立于 .NET Framework 数据提供程序，用于管理应用程序本地的数据或源自 XML 的数据。  
  
 ADO.NET 类位于 System.Data.dll 中，并与 System.Xml.dll 中的 XML 类集成。 有关连接到数据库的示例代码，从该代码中检索数据，然后在控制台窗口中显示该数据，请参阅 [ADO.NET 代码示例](ado-net-code-examples.md)。  
  
 ADO.NET 向编写托管代码的开发人员提供类似于 ActiveX 数据对象 (ADO) 向本机组件对象模型 (COM) 开发人员提供的功能。 建议您在 .NET 应用程序中使用 ADO.NET 而不使用 ADO 来访问数据。  
  
 ADO.NET 在 .NET Framework 中提供最直接的数据访问方法。 若要使应用程序能够使用概念模型而不是基础存储模型，请参阅 [ADO.NET 实体框架](./ef/index.md)。  
  
 **隐私声明**： System.Data.dll、System.Data.Design.dll、System.Data.OracleClient.dll、System.Data.SqlXml.dll、System.Data.Linq.dll、System.Data.SqlServerCe.dll 和 System.Data.DataSetExtensions.dll 程序集不区分用户的私有数据和非私有数据。  这些程序集不收集、存储或传输任何用户隐私数据。 不过，第三方应用程序可能会使用这些程序集收集、存储或传输用户的隐私数据。  
  
## <a name="in-this-section"></a>本节内容  

 [ADO.NET 体系结构](ado-net-architecture.md)  
 提供 ADO.NET 结构和组件的概述。  
  
 [ADO.NET 技术选项和准则](ado-net-technology-options-and-guidelines.md)  
 描述实体数据平台附带的产品和技术。  
  
 [LINQ 和 ADO.NET](linq-and-ado-net.md)  
 描述如何在 ADO.NET 中实现语言集成查询 (LINQ) 并提供指向相关主题的链接。  
  
 [.NET Framework 数据提供程序](data-providers.md)  
 提供有关 .NET Framework 数据提供程序的设计的概述以及有关随 ADO.NET 提供的 .NET Framework 数据提供程序的概述。  
  
 [ADO.NET 数据集](ado-net-datasets.md)  
 提供有关 `DataSet` 设计和组件的概述。  
  
 [ADO.NET 中的并行执行](side-by-side-execution.md)  
 讨论 ADO.NET 各个版本之间的区别及其对并行执行和应用程序兼容性的影响。  
  
 [ADO.NET 代码示例](ado-net-code-examples.md)  
 提供使用 ADO.NET 数据提供程序检索数据的代码示例。  
  
## <a name="related-sections"></a>相关章节  

 [ADO.NET 新增功能](whats-new.md)  
 介绍 ADO.NET 中的新增功能。  
  
 [保证 ADO.NET 应用程序的安全](securing-ado-net-applications.md)  
 描述使用 ADO.NET 时的安全编码做法。  
  
 [ADO.NET 中的数据类型映射](data-type-mappings-in-ado-net.md)  
 描述 .NET Framework 数据类型与 .NET Framework 数据提供程序之间的数据类型映射。  
  
 [在 ADO.NET 中检索和修改数据](retrieving-and-modifying-data.md)  
 说明如何连接到数据源、检索数据和修改数据。 这包括 `DataReaders` 和 `DataAdapters`。  
  
## <a name="see-also"></a>请参阅

- [ADO.NET](index.md)
- [在 Visual Studio 中访问数据](/visualstudio/data-tools/accessing-data-in-visual-studio)
