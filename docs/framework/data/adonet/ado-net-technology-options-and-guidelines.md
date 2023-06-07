---
title: 技术选项和指南
ms.date: 03/30/2017
ms.assetid: c8577281-38e6-4ce5-b036-572039a4c3d8
ms.openlocfilehash: c91eff55a322bfbe5675395a54c2b294d67d909a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91185309"
---
# <a name="adonet-technology-options-and-guidelines"></a>ADO.NET 技术选项和准则

ADO.NET 数据平台是一种多版本策略，通过使开发人员能够针对概念性实体数据模型进行编程，减少其所需的编码和维护工作量。 此平台包括 ADO.NET 实体框架和相关技术。  
  
## <a name="entity-framework"></a>Entity Framework  

 ADO.NET 实体框架专门用于让开发人员能够通过针对概念应用程序模型进行编程（而不是直接针对关系存储架构进行编程）来创建数据访问应用程序。 这样做的目的是减少面向数据的应用程序所需的编码和维护工作。 有关详细信息，请参阅 [ADO.NET 实体框架](./ef/index.md)。  
  
### <a name="entity-data-model-edm"></a>实体数据模型 (EDM)  

 实体数据模型 (EDM) 是一种将应用程序数据定义为多组实体和关系的设计规范。 此模型中的数据支持跨应用程序边界的数据关系映射和数据可编程性。  
  
### <a name="object-services"></a>Object Services — 对象服务  

 对象服务允许程序员通过一组公共语言运行库 (CLR) 类与概念模型进行交互。 这些类既可以从概念模型自动生成，也可以单独开发以反映概念模型的结构。 对象服务还为实体框架提供基础结构支持，包括状态管理、更改跟踪、标识解析、加载和导航关系、将对象更改传播到数据库修改和实体 SQL 查询生成支持等服务。 有关详细信息，请参阅[对象服务概述 (Entity Framework)](/previous-versions/bb386871(v=vs.100))。  
  
### <a name="linq-to-entities"></a>LINQ to Entities  

 LINQ to Entities 是一种 (LINQ) 实现的语言集成查询，它允许开发人员通过使用 LINQ 表达式和 LINQ 标准查询运算符为实体框架对象上下文创建强类型查询。 LINQ to Entities 允许开发人员使用跨 Microsoft SQL Server 和第三方数据库的灵活对象关系映射来处理概念模型。 有关详细信息，请参阅 [LINQ to Entities](./ef/language-reference/linq-to-entities.md)。  
  
### <a name="entity-sql"></a>Entity SQL  

 实体 SQL 是一种基于文本的查询语言，专门用于与实体数据模型进行交互。 实体 SQL 是一种 SQL 变体，其中包含针对更高级别的建模概念的查询构造，例如继承、复杂类型和显式关系。 开发人员也可以直接将实体 SQL 与对象服务一起使用。 有关详细信息，请参阅 [实体 SQL 语言](./ef/language-reference/entity-sql-language.md)。  
  
### <a name="entityclient"></a>EntityClient  

 EntityClient 是一种用于与实体数据模型交互的新的 .NET Framework 数据提供程序。 EntityClient 遵循 .NET Framework 数据提供程序模式，公开可返回 <xref:System.Data.EntityClient.EntityConnection> 的 <xref:System.Data.EntityClient.EntityCommand> 对象和 <xref:System.Data.EntityClient.EntityDataReader> 对象。 EntityClient 与实体 SQL 语言一起使用，可提供与特定于存储的数据提供程序的灵活映射。 有关详细信息，请参阅 [用于 Entity Framework 的 EntityClient 提供程序](./ef/entityclient-provider-for-the-entity-framework.md)。  
  
### <a name="entity-data-model-tools"></a>实体数据模型工具  

 实体框架提供了命令行工具、向导和设计器来帮助生成 EDM 应用程序。 EntityDataSource 控件支持基于 EDM 的数据绑定方案。 EntityDataSource 控件的编程接口与 Visual Studio 中的其他数据源控件类似。 有关详细信息，请参阅 [ADO.NET 实体数据模型工具](/previous-versions/dotnet/netframework-4.0/bb399249(v=vs.100))。  
  
## <a name="linq-to-sql"></a>LINQ to SQL  

 LINQ to SQL 是一种对象关系映射 (OR/M) 实现，它使您能够使用 .NET Framework 类为 SQL Server 数据建模。 LINQ to SQL 允许使用 LINQ 查询数据库，以及更新、插入和删除数据。 LINQ to SQL 支持事务、视图和存储过程，因此可以方便地将数据验证和业务逻辑规则集成到数据模型中。 您可以使用对象关系设计器（O/R 设计器），为基于数据库中对象的实体类和关联建模。 有关详细信息，请参阅 [Visual Studio 中的 LINQ to SQL 工具](/visualstudio/data-tools/linq-to-sql-tools-in-visual-studio2)。  
  
## <a name="wcf-data-services"></a>WCF 数据服务  

 WCF 数据服务在 Web 或 intranet 上部署数据服务。 这些数据将按照实体数据模型的规范组织成不同的实体和关系。 在此模型上部署的数据可通过标准的 HTTP 协议进行寻址。 有关详细信息，请参阅 [WCF Data Services 4.5](../wcf/index.md)。  
  
## <a name="see-also"></a>请参阅

- [ADO.NET 概述](ado-net-overview.md)
- [ADO.NET 新增功能](whats-new.md)
