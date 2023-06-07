---
title: DataAdapter 和 DataReader
description: 了解用于从数据库中检索数据的 ADO.NET DataReader 以及用于从数据源检索数据并填充数据集的 DataAdapter。
ms.date: 03/30/2017
ms.assetid: cc952ca2-ec19-46ab-9189-15174b52cb74
ms.openlocfilehash: 2584f8b382dd90f2f8b4554663dc545b9ccceb62
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91177600"
---
# <a name="dataadapters-and-datareaders"></a>DataAdapter 和 DataReader

您可以使用 ADO.NET **DataReader** 从数据库中检索只读、只进数据流。 结果将在执行查询时返回，并存储在客户端的网络缓冲区中，直到您使用**DataReader**的**Read**方法请求它们。 使用 **DataReader** 可以通过在数据可用时立即检索数据来提高应用程序的性能，并 (默认情况下) 在内存中一次只存储一行，从而降低系统开销。  
  
 <xref:System.Data.Common.DataAdapter> 用于从数据源检索数据并填充 <xref:System.Data.DataSet> 中的表。 `DataAdapter` 还可将对 `DataSet` 所做的更改解析回数据源。 `DataAdapter` 使用 .NET Framework 数据提供程序的 `Connection` 对象连接到数据源，并使用 `Command` 对象从数据源检索数据以及将更改解析回数据源。  
  
 随 .NET Framework 提供的每个 .NET Framework 数据提供程序都具有一个 <xref:System.Data.Common.DbDataReader> 和一个 <xref:System.Data.Common.DbDataAdapter> 对象：用于 OLE DB 的 .NET Framework 数据提供程序包括一个 <xref:System.Data.OleDb.OleDbDataReader> 和一个 <xref:System.Data.OleDb.OleDbDataAdapter> 对象，用于 SQL Server 的 .NET Framework 数据提供程序包括一个 <xref:System.Data.SqlClient.SqlDataReader> 和一个 <xref:System.Data.SqlClient.SqlDataAdapter> 对象，用于 ODBC 的 .NET Framework 数据提供程序包括一个 <xref:System.Data.Odbc.OdbcDataReader> 和一个 <xref:System.Data.Odbc.OdbcDataAdapter> 对象，用于 Oracle 的 .NET Framework 数据提供程序包括一个 <xref:System.Data.OracleClient.OracleDataReader> 和一个 <xref:System.Data.OracleClient.OracleDataAdapter> 对象。  
  
## <a name="in-this-section"></a>本节内容  

 [使用 DataReader 检索 ADO 数据](retrieving-data-using-a-datareader.md)  
 介绍 ADO.NET **DataReader** 对象，以及如何使用它从数据源返回结果流。  
  
 [从 DataAdapter 填充数据集](populating-a-dataset-from-a-dataadapter.md)  
 说明如何通过 `DataSet` 使用表、列和行填充 `DataAdapter`。  
  
 [DataAdapter 参数](dataadapter-parameters.md)  
 说明如何与 `DataAdapter` 的命令属性一起使用参数，包括如何将 `DataSet` 的列内容映射到命令参数。  
  
 [将现有约束添加到数据集](adding-existing-constraints-to-a-dataset.md)  
 说明如何将现有约束添加到 `DataSet`。  
  
 [DataAdapter 数据表和 DataColumn 映射](dataadapter-datatable-and-datacolumn-mappings.md)  
 说明如何为 `DataTableMappings` 设置 `ColumnMappings` 和 `DataAdapter`。  
  
 [通过查询结果分页](paging-through-a-query-result.md)  
 提供一个以数据页形式查看查询结果的示例。  
  
 [使用 DataAdapter 更新数据源](updating-data-sources-with-dataadapters.md)  
 说明如何使用 `DataAdapter` 将 `DataSet` 中的更改解析回数据库。  
  
 [处理 DataAdapter 事件](handling-dataadapter-events.md)  
 说明 `DataAdapter` 事件以及如何使用这些事件。  
  
 [使用 DataAdapter 执行批处理操作](performing-batch-operations-using-dataadapters.md)  
 说明在从 `DataSet` 应用更新时，如何通过减少与 SQL Server 之间的往返次数来提高应用程序的性能。  
  
## <a name="see-also"></a>请参阅

- [连接数据源](connecting-to-a-data-source.md)
- [命令和参数](commands-and-parameters.md)
- [事务和并发性](transactions-and-concurrency.md)
- [数据集、数据表和数据视图](./dataset-datatable-dataview/index.md)
- [ADO.NET 概述](ado-net-overview.md)
