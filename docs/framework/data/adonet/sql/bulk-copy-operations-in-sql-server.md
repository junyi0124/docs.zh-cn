---
title: SQL Server 中的批量复制操作
description: 了解如何使用 SqlBulkCopy 类编写将大型文件大容量复制到 SQL Server 数据库中的表或视图中的托管代码解决方案。
ms.date: 03/30/2017
ms.assetid: 83a7a0d2-8018-4354-97b9-0b1d99f8342b
ms.openlocfilehash: 43d63f3671ea8ff05da3e10580c2784fa3aae581
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91197425"
---
# <a name="bulk-copy-operations-in-sql-server"></a>SQL Server 中的批量复制操作

Microsoft SQL Server 包含一个名为 bcp 的受欢迎的命令行实用工具，以便将较大文件快速大容量复制到 SQL Server 数据库的表或视图中  。 <xref:System.Data.SqlClient.SqlBulkCopy> 类允许你编写可提供类似功能的托管代码解决方案。 还可通过其他方法将数据加载到 SQL Server 表（例如 INSERT 语句），但 <xref:System.Data.SqlClient.SqlBulkCopy> 可提供显著的性能优势。  
  
 <xref:System.Data.SqlClient.SqlBulkCopy> 类可用于只将数据写入 SQL Server 表。 但是，数据源不限于 SQL Server；可以使用任何数据源，只要数据可以加载到 <xref:System.Data.DataTable> 实例或使用 <xref:System.Data.IDataReader> 实例读取即可。  
  
 使用 <xref:System.Data.SqlClient.SqlBulkCopy> 类，你可以执行以下操作：  
  
- 单次大容量复制操作  
  
- 多次大容量复制操作  
  
- 事务中的大容量复制操作  
  
> [!NOTE]
> 在使用 .NET Framework 1.1 版或更低版本时（不支持 <xref:System.Data.SqlClient.SqlBulkCopy> 类），可以使用  **对象执行 SQL Server Transact-SQL BULK INSERT 语句**<xref:System.Data.SqlClient.SqlCommand>。  
  
## <a name="in-this-section"></a>本节内容  

 [大容量复制示例设置](bulk-copy-example-setup.md)  
 介绍用于大容量复制示例的表，并提供用于在 AdventureWorks 数据库中创建表的 SQL 脚本。  
  
 [单次大容量复制操作](single-bulk-copy-operations.md)  
 介绍如何使用 <xref:System.Data.SqlClient.SqlBulkCopy> 类将单个大容量数据复制到 SQL Server 实例，以及如何使用 Transact-SQL 语句和 <xref:System.Data.SqlClient.SqlCommand> 类执行大容量复制操作。  
  
 [多个大容量复制操作](multiple-bulk-copy-operations.md)  
 介绍如何使用 <xref:System.Data.SqlClient.SqlBulkCopy> 类在 SQL Server 的实例中执行多次大容量复制操作。  
  
 [事务和大容量复制操作](transaction-and-bulk-copy-operations.md)  
 介绍了如何在事务中执行大容量复制操作，包括如何提交或回滚事务。  
  
## <a name="see-also"></a>请参阅

- [SQL Server 和 ADO.NET](index.md)
- [ADO.NET 概述](../ado-net-overview.md)
