---
title: SQL Server 数据类型和 ADO.NET
titleSuffix: ''
ms.date: 03/30/2017
ms.assetid: 81b43550-23e8-43bb-b460-7eb8ac825c33
ms.openlocfilehash: db4618ac624ea8401cab682a8c21d8f23c253d05
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91155453"
---
# <a name="sql-server-data-types-and-adonet"></a>SQL Server 数据类型和 ADO.NET

SQL Server 和 .NET Framework 基于不同的类型系统，这可导致潜在的数据丢失。 为了保持数据的完整性，适用于 SQL Server 的 .NET Framework 数据提供程序 (<xref:System.Data.SqlClient>) 提供了用于处理 SQL Server 数据的类型化访问器方法。 可以使用 <xref:System.Data.SqlDbType> 类中的枚举来指定 <xref:System.Data.SqlClient.SqlParameter> 数据类型。  
  
 有关 SQL Server 和 .NET Framework 数据类型之间的数据类型映射的详细信息和表，请参阅 [SQL Server 数据类型映射](../sql-server-data-type-mappings.md)。  
  
 SQL Server 2008 引入了新的数据类型，这些数据类型旨在满足企业使用日期和时间数据、结构化数据、半结构化数据和非结构化数据的需求。 相关文档位于 SQL Server 2008 联机丛书中。  
  
 可在应用程序中使用的 SQL Server 数据类型取决于使用的 SQL Server 版本。 有关更多信息，请参见下表中的相关版本的 SQL Server 联机丛书。  
  
 **SQL Server 文档**  
  
1. [数据类型 (Transact-SQL)](/sql/t-sql/data-types/data-types-transact-sql)  
  
## <a name="in-this-section"></a>本节内容  

 [SqlTypes 和数据集](sqltypes-and-the-dataset.md)  
 介绍对 `DataSet` 中 `SqlTypes` 的类型支持。  
  
 [处理 Null 值](handling-null-values.md)  
 演示如何处理 null 值和三值逻辑。  
  
 [比较 GUID 和 uniqueidentifier 值](comparing-guid-and-uniqueidentifier-values.md)  
 演示如何在 SQL Server 和 .NET Framework 中使用 GUID 和 uniqueidentifier 值。  
  
 [日期和时间数据](date-and-time-data.md)  
 介绍如何使用在 SQL Server 2008 中引入的新的日期和时间数据类型。  
  
 [大型 UDT](large-udts.md)  
 演示如何通过 SQL Server 2008 中引入的大型值 UDT 检索数据。  
  
 [SQL Server 中的 XML 数据](xml-data-in-sql-server.md)  
 介绍如何使用从 SQL Server 检索的 XML 数据。  
  
## <a name="reference"></a>参考  

 <xref:System.Data.DataSet>  
 介绍 `DataSet` 类及其所有成员。  
  
 <xref:System.Data.SqlTypes>  
 介绍 `SqlTypes` 命名空间及其所有成员。  
  
 <xref:System.Data.SqlDbType>  
 介绍 `SqlDbType` 枚举及其所有成员。  
  
 <xref:System.Data.DbType>  
 介绍 `DbType` 枚举及其所有成员。  
  
## <a name="see-also"></a>请参阅

- [SQL Server 数据类型映射](../sql-server-data-type-mappings.md)
- [配置参数和参数数据类型](../configuring-parameters-and-parameter-data-types.md)
- [表值参数](table-valued-parameters.md)
- [SQL Server 二进制和大值数据](sql-server-binary-and-large-value-data.md)
- [ADO.NET 概述](../ado-net-overview.md)
