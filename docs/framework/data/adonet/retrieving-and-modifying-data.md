---
title: 检索和修改数据
description: 在 .NET Framework 中，ADO.NET 中的数据提供程序充当应用程序和数据源之间的桥梁，用于读取和更新数据。
ms.date: 03/30/2017
ms.assetid: 722e7f87-3691-46c6-87e8-7d159722d675
ms.openlocfilehash: 7620843e77b25606b2dec2bf6eae3a4f40d1b9fb
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91150669"
---
# <a name="retrieving-and-modifying-data-in-adonet"></a>在 ADO.NET 中检索和修改数据

任何数据库应用程序的一项主要功能是连接数据源并检索数据源中包含的数据。 ADO.NET 的 .NET Framework 数据提供程序充当应用程序和数据源之间的桥梁，使你能够执行命令以及使用 **DataReader** 或 **DataAdapter**检索数据。 任何数据库应用程序的一项关键功能是更新数据库中存储的数据的能力。 在 ADO.NET 中，更新数据涉及使用 **DataAdapter** 和和 <xref:System.Data.DataSet> **Command** 对象; 它还可能涉及使用事务。  
  
## <a name="in-this-section"></a>本节内容  

 [连接数据源](connecting-to-a-data-source.md)  
 说明如何建立到数据源的连接及如何使用连接事件。  
  
 [连接字符串](connection-strings.md)  
 包含说明使用连接字符串（包括连接字符串关键字、安全信息以及存储和检索连接字符串）的各个方面的主题。  
  
 [连接池](connection-pooling.md)  
 描述 .NET Framework 数据提供程序的连接池。  
  
 [命令和参数](commands-and-parameters.md)  
 包含说明如何创建命令和命令生成器、配置参数以及如何执行命令来检索和修改数据的主题。  
  
 [DataAdapter 和 DataReader](dataadapters-and-datareaders.md)  
 包含说明 DataReader、DataAdapter、参数、处理 DataAdapter 事件和执行批操作的主题。  
  
 [事务和并发性](transactions-and-concurrency.md)  
 包含说明如何执行本地事务、分布式事务及使用开放式并发的主题。  
  
 [检索标识或自动编号值](retrieving-identity-or-autonumber-values.md)  
 提供一个示例，该示例将为 SQL Server 表中的 **标识** 列或 Microsoft Access 表中的 **Autonumber** 字段生成的值映射到表中插入行的列。 讨论在 `DataTable` 中合并标识值。  
  
 [检索二进制数据](retrieving-binary-data.md)  
 介绍如何使用检索二进制数据或大数据结构 `CommandBehavior` 。`SequentialAccess` 修改的默认行为 `DataReader` 。  
  
 [使用存储过程修改数据](modifying-data-with-stored-procedures.md)  
 说明如何使用存储过程的输入参数和输出参数在数据库中插入行，同时返回新标识值。  
  
 [检索数据库架构信息](retrieving-database-schema-information.md)  
 说明如何获取可用数据库或编录、数据库中的表和视图、表存在的约束以及数据源中的其他架构信息。  
  
 [DbProviderFactories](dbproviderfactories.md)  
 描述提供程序工厂模型及说明如何在 `System.Data.Common` 命名空间中使用基类。  
  
 [ADO.NET 中的数据跟踪](data-tracing.md)  
 说明 ADO.NET 如何提供内置的数据跟踪功能。  
  
 [性能计数器](performance-counters.md)  
 说明 `SqlClient` 和 `OracleClient` 可用的性能计数器。  
  
 [异步编程](asynchronous-programming.md)  
 介绍异步编程的 ADO.NET 支持。  
  
 [SqlClient 流支持](sqlclient-streaming-support.md)  
 讨论如何编写从 SQL Server 流式传输数据的应用程序，而无需将其完全加载到内存中。  
  
## <a name="see-also"></a>请参阅

- [ADO.NET 中的数据类型映射](data-type-mappings-in-ado-net.md)
- [数据集、数据表和数据视图](./dataset-datatable-dataview/index.md)
- [保证 ADO.NET 应用程序的安全](securing-ado-net-applications.md)
- [SQL Server 和 ADO.NET](./sql/index.md)
- [ADO.NET 概述](ado-net-overview.md)
