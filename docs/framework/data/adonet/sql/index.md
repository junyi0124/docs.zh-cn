---
title: SQL Server 和 ADO.NET
description: 了解用于 SQL Server 的 .NET Framework 数据提供程序的功能和行为，用于封装特定于数据库的协议。
titleSuffix: ''
ms.date: 03/30/2017
ms.assetid: c18b1fb1-2af1-4de7-80a4-95e56fd976cb
ms.openlocfilehash: a517bccd9b60d00f6c6c636c9164d63fb5966de3
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91197386"
---
# <a name="sql-server-and-adonet"></a>SQL Server 和 ADO.NET

本节描述适用于 SQL Server 的 .NET Framework 数据提供程序 (<xref:System.Data.SqlClient>) 特定的功能和行为。  
  
 <xref:System.Data.SqlClient> 提供对 SQL Server 各版本的访问权限，这些版本封装有数据库特定的协议。 该数据提供程序设计的功能与 OLE DB、ODBC 和 Oracle 的 .NET Framework 数据提供程序的功能类似。 <xref:System.Data.SqlClient> 包含一个表格格式数据流 (TDS) 分析程序，它用于直接与 SQL Server 通信。  
  
> [!NOTE]
> 要使用 SQL Server .NET Framework 数据提供程序，应用程序必须引用 <xref:System.Data.SqlClient> 命名空间。  
  
## <a name="in-this-section"></a>本节内容  

 [SQL Server 安全性](sql-server-security.md)  
 简要介绍 SQL Server 安全功能，以及创建面向 SQL Server 的安全 ADO.NET 应用程序的应用程序方案。  
  
 [SQL Server 数据类型和 ADO.NET](sql-server-data-types.md)  
 描述如何使用 SQL Server 数据类型以及这些数据类型如何与 .NET Framework 数据类型进行交互。  
  
 [SQL Server 二进制和大值数据](sql-server-binary-and-large-value-data.md)  
 介绍如何在 SQL Server 中使用大值数据。  
  
 [ADO.NET 中的 SQL Server 数据操作](sql-server-data-operations.md)  
 介绍如何使用 SQL Server 中的数据。 包含有关大容量复制操作、MARS、异步操作和表值参数的部分。  
  
 [SQL Server 功能和 ADO.NET](sql-server-features-and-adonet.md)  
 介绍对 ADO.NET 应用程序开发人员而言有用的 SQL Server 功能。  
  
 [LINQ to SQL](./linq/index.md)  
 描述创建 LINQ to SQL 应用程序所需的基本构造块、进程和技术。  
  
 有关 SQL Server 数据库引擎的完整文档，请参见你正在使用的 SQL Server 版本的 SQL Server 联机丛书。  
  
 [QL Server 联机丛书](/sql/sql-server/sql-server-technical-documentation)  
  
## <a name="see-also"></a>请参阅

- [保证 ADO.NET 应用程序的安全](../securing-ado-net-applications.md)
- [ADO.NET 中的数据类型映射](../data-type-mappings-in-ado-net.md)
- [数据集、数据表和数据视图](../dataset-datatable-dataview/index.md)
- [在 ADO.NET 中检索和修改数据](../retrieving-and-modifying-data.md)
- [ADO.NET 概述](../ado-net-overview.md)
