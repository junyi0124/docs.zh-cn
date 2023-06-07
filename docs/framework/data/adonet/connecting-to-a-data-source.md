---
title: 连接数据源
deescription: Learn about Connection objects, used to connect to data sources in ADO.NET. The Connection object you choose depends on the type of data source.
ms.date: 03/30/2017
ms.assetid: 9abc3f92-1be3-4e1a-b360-762dc689650e
ms.openlocfilehash: b9e69b029ad37c583e51c219f87ff9d7d8e7315c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203769"
---
# <a name="connecting-to-a-data-source-in-adonet"></a>连接到 ADO.NET 中的数据源

在 ADO.NET 中，通过在连接字符串中提供必要的身份验证信息，使用 **连接** 对象连接到特定数据源。 使用的 **连接** 对象取决于数据源的类型。  
  
 随 .NET Framework 提供的每个 .NET Framework 数据提供程序都具有一个 <xref:System.Data.Common.DbConnection> 对象：适用于 OLE DB 的 .NET Framework 数据提供程序包括一个 <xref:System.Data.OleDb.OleDbConnection> 对象，适用于 SQL Server 的 .NET Framework 数据提供程序包括一个 <xref:System.Data.SqlClient.SqlConnection> 对象，适用于 ODBC 的 .NET Framework 数据提供程序包括一个 <xref:System.Data.Odbc.OdbcConnection> 对象，适用于 Oracle 的 .NET Framework 数据提供程序包括一个 <xref:System.Data.OracleClient.OracleConnection> 对象。  
  
## <a name="in-this-section"></a>本节内容  

 [建立连接](establishing-the-connection.md)\
 介绍如何使用 **连接** 对象建立与数据源的连接。  
  
 [连接事件](connection-events.md)\
 介绍如何使用 **InfoMessage** 事件从数据源中检索信息性消息。  
  
## <a name="see-also"></a>请参阅

- [连接字符串](connection-strings.md)
- [连接池](connection-pooling.md)
- [命令和参数](commands-and-parameters.md)
- [DataAdapter 和 DataReader](dataadapters-and-datareaders.md)
- [事务和并发性](transactions-and-concurrency.md)
- [ADO.NET 概述](ado-net-overview.md)
