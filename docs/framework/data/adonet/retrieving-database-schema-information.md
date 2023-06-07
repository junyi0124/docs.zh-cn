---
title: 检索数据库架构信息
ms.date: 03/30/2017
ms.assetid: 79038d52-f122-4fd4-9bfb-aaa22d6a114b
ms.openlocfilehash: c0aaadc82771d1c2a36d797bc157d88b8d3cacdc
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91177353"
---
# <a name="retrieving-database-schema-information"></a>检索数据库架构信息

从数据库获取架构信息通过架构发现过程来完成。 架构发现允许应用程序请求托管提供程序查找并返回有关给定数据库的数据库架构的信息（也称为 *元数据*）。 不同的数据库架构元素（例如表、列和存储过程）通过架构集合进行公开。 每个架构集合包含所使用的提供程序特定的各种架构信息。  
  
 每个 .NET Framework 托管提供程序都实现**连接**类中的**GetSchema**方法， **GetSchema**方法返回的架构信息采用的形式 <xref:System.Data.DataTable> 。 **GetSchema**方法是一种重载方法，该方法提供了用于指定要返回的架构集合以及限制返回的信息量的可选参数。  
  
 用于 OLE DB、ODBC、Oracle 和 SqlClient 的 .NET Framework 数据提供程序提供了一个 **GetSchemaTable** 方法，该方法可返回描述 **DataReader**的列元数据的 DataTable。  
  
 适用于 OLE DB 的 .NET Framework 数据提供程序还使用 <xref:System.Data.OleDb.OleDbConnection.GetOleDbSchemaTable%2A> 对象的 <xref:System.Data.OleDb.OleDbConnection> 方法来公开架构信息。 作为参数， **GetOleDbSchemaTable** 采用 <xref:System.Data.OleDb.OleDbSchemaGuid> 标识要返回的架构信息的，以及对那些返回列的限制数组。 **GetOleDbSchemaTable** 返回 <xref:System.Data.DataTable> 用请求的架构信息填充的。  
  
## <a name="in-this-section"></a>本节内容  

 [GetSchema 和架构集合](getschema-and-schema-collections.md)  
 介绍 **GetSchema** 方法以及如何使用它来从数据库中检索和限制架构信息。  
  
 架构限制  
 介绍可用于 **GetSchema**的架构限制。  
  
 [公共架构集合](common-schema-collections.md)  
 描述所有 .NET Framework 托管提供程序均支持的所有通用架构集合。  
  
 [SQL Server 架构集合](sql-server-schema-collections.md)  
 描述适用于 SQL Server 的 .NET Framework 提供程序支持的架构集合。  
  
 [Oracle 架构集合](oracle-schema-collections.md)  
 描述适用于 Oracle 的 SQL Server .NET Framework 提供程序支持的架构集合。  
  
 [ODBC 架构集合](odbc-schema-collections.md)  
 描述 ODBC 驱动程序的架构集合。  
  
 [OLE DB 架构集合](ole-db-schema-collections.md)  
 描述 OLE DB 提供程序的架构集合。  
  
## <a name="reference"></a>参考  

 <xref:System.Data.Common.DbConnection.GetSchema%2A>  
 描述类的 **GetSchema** 方法 <xref:System.Data.Common.DbConnection> 。  
  
 <xref:System.Data.Odbc.OdbcConnection.GetSchema%2A>  
 描述类的 **GetSchema** 方法 <xref:System.Data.Odbc.OdbcConnection> 。  
  
 <xref:System.Data.OleDb.OleDbConnection.GetSchema%2A>  
 描述类的 **GetSchema** 方法 <xref:System.Data.OleDb.OleDbConnection> 。  
  
 <xref:System.Data.OracleClient.OracleConnection.GetSchema%2A>  
 描述类的 **GetSchema** 方法 <xref:System.Data.OracleClient.OracleConnection> 。  
  
 <xref:System.Data.SqlClient.SqlConnection.GetSchema%2A>  
 描述类的 **GetSchema** 方法 <xref:System.Data.SqlClient.SqlConnection> 。  
  
 <xref:System.Data.Common.DbDataReader.GetSchemaTable%2A>  
 描述类的 **GetSchemaTable** 方法 <xref:System.Data.Common.DbDataReader> 。  
  
 <xref:System.Data.Odbc.OdbcDataReader.GetSchemaTable%2A>  
 描述类的 **GetSchemaTable** 方法 <xref:System.Data.Odbc.OdbcDataReader> 。  
  
 <xref:System.Data.OleDb.OleDbDataReader.GetSchemaTable%2A>  
 描述类的 **GetSchemaTable** 方法 <xref:System.Data.OleDb.OleDbDataReader> 。  
  
 <xref:System.Data.OracleClient.OracleDataReader.GetSchemaTable%2A>  
 描述类的 **GetSchemaTable** 方法 <xref:System.Data.OracleClient.OracleDataReader> 。  
  
 <xref:System.Data.SqlClient.SqlDataReader.GetSchemaTable%2A>  
 描述类的 **GetSchemaTable** 方法 <xref:System.Data.SqlClient.SqlDataReader> 。  
  
## <a name="see-also"></a>请参阅

- [在 ADO.NET 中检索和修改数据](retrieving-and-modifying-data.md)
- [ADO.NET 概述](ado-net-overview.md)
