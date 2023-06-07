---
title: 新增功能
description: 了解 .NET Framework 4.5 中 ADO.NET 的新增功能，包括 SqlClient 数据提供程序和 ADO.NET 的新功能实体框架。
ms.date: 03/30/2017
ms.assetid: 3bb65d38-cce2-46f5-b979-e5c505e95e10
ms.openlocfilehash: b34a27574b6aab75539f9ab30e2978e45b4ad9e3
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90553852"
---
# <a name="whats-new-in-adonet"></a>ADO.NET 新增功能

以下功能是 .NET Framework 4.5 中 ADO.NET 的新增功能。

## <a name="sqlclient-data-provider"></a>SqlClient Data Provider

以下功能是 .NET Framework 4.5 中 SQL Server 的 .NET Framework 数据提供程序中的新增功能：

- ConnectRetryCount 和 ConnectRetryInterval 连接字符串关键字 (<xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A>) 可以让你控制空闲连接复原功能。

- 从 SQL Server 到应用程序的流式处理支持支持服务器上的数据是非结构化的情况。  有关详细信息，请参阅 [SqlClient 流支持](sqlclient-streaming-support.md) 。

- 已添加了异步编程支持。  有关详细信息，请参阅 [异步编程](asynchronous-programming.md) 。

- 连接故障现在将记录在扩展事件日志中。 有关详细信息，请参阅 [ADO.NET 中的数据跟踪](data-tracing.md)。

- SqlClient 现在支持 SQL Server 的高可用性、灾难恢复功能、AlwaysOn。 有关详细信息，请参阅 [SqlClient 对高可用性、灾难恢复的支持](./sql/sqlclient-support-for-high-availability-disaster-recovery.md)。

- 使用 SQL Server 身份验证时，可以将密码作为传递 <xref:System.Security.SecureString> 。 有关更多信息，请参见<xref:System.Data.SqlClient.SqlCredential>。

- 如果 `TrustServerCertificate` 为 false 且 `Encrypt` 为 true，则 SQL Server SSL 证书中的服务器名称 (或 ip) 地址必须与连接字符串中指定的服务器名称 (或 ip) 地址完全匹配。 否则，连接尝试将失败。 有关更多信息，请参见 `Encrypt` 中 <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A> 选项的说明。

  如果此更改导致现有应用程序不再连接，可以通过以下方法之一修复应用程序：

  - 颁发证书，以在公用名 (CN) 或主题备用名称 (SAN) 字段中指定短名称。 此解决方案将适用于数据库镜像。

  - 添加别名，将短名称映射到完全限定的域名。

  - 在连接字符串中使用完全限定的域名。

- SqlClient 支持扩展保护。 有关扩展保护的详细信息，请参阅 [使用扩展保护连接到数据库引擎](/sql/database-engine/configure-windows/connect-to-the-database-engine-using-extended-protection)。

- SqlClient 支持连接到 LocalDB 数据库。 有关详细信息，请参阅 [SqlClient 对 LocalDB 的支持](./sql/sqlclient-support-for-localdb.md)。

- `Type System Version=SQL Server 2012;` 是传递给 `Type System Version` 连接属性的新值。 `Type System Version=Latest;` 值现已过时，它与 `Type System Version=SQL Server 2008;` 等效。 有关详细信息，请参阅 <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A>。

- SqlClient 为稀疏列（SQL Server 2008 中新增的功能）提供额外支持。 如果应用程序已访问使用稀疏列的表中的数据，应看到性能有所提高。 <xref:System.Data.SqlClient.SqlDataReader.GetSchemaTable%2A> 的 IsColumnSet 列指示某列是否为属于列集成员的稀疏列。 <xref:System.Data.SqlClient.SqlConnection.GetSchema%2A> 指示列是否为稀疏列 (参阅 [SQL Server 架构集合](sql-server-schema-collections.md) 以获取) 详细信息。 有关稀疏列的详细信息，请参阅 [使用稀疏列](/sql/relational-databases/tables/use-sparse-columns)。

- 包含空间数据类型的程序集 Microsoft.SqlServer.Types.dll 已从 10.0 版本升级到版本 11.0。 引用此程序集的应用程序可能失败。 有关详细信息，请参阅 [数据库引擎功能的重大更改](/previous-versions/sql/sql-server-2012/ms143179(v=sql.110))。

## <a name="adonet-entity-framework"></a>ADO.NET 实体框架

.NET Framework 4.5 添加了在使用实体框架5.0 时启用新方案的 Api。 有关添加到实体框架5.0 的改进和功能的详细信息，请参阅以下主题： [新增功能](/previous-versions/gg696190(v=vs.103)) 和 [实体框架版本和版本控制](/ef/ef6/what-is-new/past-releases)。

## <a name="see-also"></a>请参阅

- [ADO.NET](index.md)
- [ADO.NET 概述](ado-net-overview.md)
- [SQL Server 和 ADO.NET](./sql/index.md)
- [WCF 数据服务5.0 的新增功能](/previous-versions/dotnet/wcf-data-services/ee373845(v=vs.103))
