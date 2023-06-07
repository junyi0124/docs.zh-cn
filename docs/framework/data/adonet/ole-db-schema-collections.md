---
title: OLE DB 架构集合
ms.date: 03/30/2017
ms.assetid: 6380c36b-658e-4d67-91e8-7131ef4a7c2c
ms.openlocfilehash: 90899a123b3dafcd47a50ef8f6eb003938b22a03
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91186934"
---
# <a name="ole-db-schema-collections"></a>OLE DB 架构集合

本节讨论对适用于 Microsoft SQL Server、Oracle 和 Microsoft Jet 的 OLE DB 提供程序的架构集合支持  
  
## <a name="microsoft-sql-server-ole-db-provider"></a>Microsoft SQL Server OLE DB 提供程序  

 除了通用架构集合之外，Microsoft SQL Server OLE DB 驱动程序还支持下列特定的架构集合：  
  
- 表  
  
- 列  
  
- 过程  
  
- ProcedureParameters  
  
- 目录  
  
- 索引  
  
### <a name="tables"></a>表  
  
|ColumnName|数据类型|  
|----------------|--------------|  
|TABLE_CATALOG|字符串|  
|TABLE_SCHEMA|字符串|  
|TABLE_NAME|字符串|  
|TABLE_TYPE|字符串|  
|TABLE_GUID|Guid|  
|DESCRIPTION|字符串|  
|TABLE_PROPID|Int64|  
|DATE_CREATED|DateTime|  
|DATE_MODIFIED|DateTime|  
  
### <a name="columns"></a>列  
  
|ColumnName|数据类型|  
|----------------|--------------|  
|TABLE_CATALOG|字符串|  
|TABLE_SCHEMA|字符串|  
|TABLE_NAME|字符串|  
|COLUMN_NAME|字符串|  
|COLUMN_GUID|Guid|  
|COLUMN_PROPID|Int64|  
|ORDINAL_POSITION|Int64|  
|COLUMN_HASDEFAULT|Boolean|  
|COLUMN_DEFAULT|字符串|  
|COLUMN_FLAGS|Int64|  
|IS_NULLABLE|Boolean|  
|DATA_TYPE|Int32|  
|TYPE_GUID|Guid|  
|CHARACTER_MAXIMUM_LENGTH|Int64|  
|CHARACTER_OCTET_LENGTH|Int64|  
|NUMERIC_PRECISION|Int32|  
|NUMERIC_SCALE|Int16|  
|DATETIME_PRECISION|Int64|  
|CHARACTER_SET_CATALOG|字符串|  
|CHARACTER_SET_SCHEMA|字符串|  
|CHARACTER_SET_NAME|字符串|  
|COLLATION_CATALOG|字符串|  
|COLLATION_SCHEMA|字符串|  
|COLLATION_NAME|字符串|  
|DOMAIN_CATALOG|字符串|  
|DOMAIN_SCHEMA|字符串|  
|DOMAIN_NAME|字符串|  
|DESCRIPTION|字符串|  
|COLUMN_LCID|Int32|  
|COLUMN_COMPFLAGS|Int32|  
|COLUMN_SORTID|Int32|  
|COLUMN_TDSCOLLATION|Byte[]|  
|IS_COMPUTED|Boolean|  
  
### <a name="procedures"></a>过程  
  
|ColumnName|数据类型|  
|----------------|--------------|  
|PROCEDURE_CATALOG|字符串|  
|PROCEDURE_SCHEMA|字符串|  
|PROCEDURE_NAME|字符串|  
|PROCEDURE_TYPE|Int16|  
|PROCEDURE_DEFINITION|字符串|  
|DESCRIPTION|字符串|  
|DATE_CREATED|DateTime|  
|DATE_MODIFIED|DateTime|  
  
### <a name="procedureparameters"></a>ProcedureParameters  
  
|ColumnName|数据类型|  
|----------------|--------------|  
|PROCEDURE_CATALOG|字符串|  
|PROCEDURE_SCHEMA|字符串|  
|PROCEDURE_NAME|字符串|  
|PARAMETER_NAME|字符串|  
|ORDINAL_POSITION|Int32|  
|PARAMETER_TYPE|Int32|  
|PARAMETER_HASDEFAULT|Boolean|  
|PARAMETER_DEFAULT|字符串|  
|IS_NULLABLE|Boolean|  
|DATA_TYPE|Int32|  
|CHARACTER_MAXIMUM_LENGTH|Int64|  
|CHARACTER_OCTET_LENGTH|Int64|  
|NUMERIC_PRECISION|Int32|  
|NUMERIC_SCALE|Int16|  
|DESCRIPTION|字符串|  
|TYPE_NAME|字符串|  
|LOCAL_TYPE_NAME|字符串|  
  
### <a name="catalog"></a>目录  
  
|ColumnName|数据类型|  
|----------------|--------------|  
|CATALOG_NAME|字符串|  
|DESCRIPTION|字符串|  
  
### <a name="indexes"></a>索引  
  
|ColumnName|数据类型|  
|----------------|--------------|  
|TABLE_CATALOG|字符串|  
|TABLE_SCHEMA|字符串|  
|TABLE_NAME|字符串|  
|INDEX_CATALOG|字符串|  
|INDEX_SCHEMA|字符串|  
|INDEX_NAME|字符串|  
|PRIMARY_KEY|Boolean|  
|UNIQUE|Boolean|  
|CLUSTERED|Boolean|  
|TYPE|Int32|  
|FILL_FACTOR|Int32|  
|INITIAL_SIZE|Int32|  
|NULLS|Int32|  
|SORT_BOOKMARKS|Boolean|  
|AUTO_UPDATE|Boolean|  
|NULL_COLLATION|Int32|  
|ORDINAL_POSITION|Int64|  
|COLUMN_NAME|字符串|  
|COLUMN_GUID|Guid|  
|COLUMN_PROPID|Int64|  
|COLLATION|Int16|  
|CARDINALITY|小数|  
|PAGES|Int32|  
|FILTER_CONDITION|字符串|  
|INTEGRATED|Boolean|  
  
## <a name="microsoft-oracle-ole-db-provider"></a>Microsoft Oracle OLE DB 提供程序  

 除了通用架构集合之外，Microsoft Oracle OLE DB 驱动程序还支持下列特定的架构集合：  
  
- 表  
  
- 列  
  
- 过程  
  
- ProcedureColumns  
  
- ProcedureParameters  
  
- 视图  
  
- 索引  
  
### <a name="tables"></a>表  
  
|ColumnName|数据类型|  
|----------------|--------------|  
|TABLE_CATALOG|字符串|  
|TABLE_SCHEMA|字符串|  
|TABLE_NAME|字符串|  
|TABLE_TYPE|字符串|  
|TABLE_GUID|Guid|  
|DESCRIPTION|字符串|  
|TABLE_PROPID|Int64|  
|DATE_CREATED|DateTime|  
|DATE_MODIFIED|DateTime|  
  
### <a name="columns"></a>列  
  
|ColumnName|数据类型|  
|----------------|--------------|  
|TABLE_CATALOG|字符串|  
|TABLE_SCHEMA|字符串|  
|TABLE_NAME|字符串|  
|COLUMN_NAME|字符串|  
|COLUMN_GUID|Guid|  
|COLUMN_PROPID|Int64|  
|ORDINAL_POSITION|Int64|  
|COLUMN_HASDEFAULT|Boolean|  
|COLUMN_DEFAULT|字符串|  
|COLUMN_FLAGS|Int64|  
|IS_NULLABLE|Boolean|  
|DATA_TYPE|Int32|  
|TYPE_GUID|Guid|  
|CHARACTER_MAXIMUM_LENGTH|Int64|  
|CHARACTER_OCTET_LENGTH|Int64|  
|NUMERIC_PRECISION|Int32|  
|NUMERIC_SCALE|Int16|  
|DATETIME_PRECISION|Int64|  
|CHARACTER_SET_CATALOG|字符串|  
|CHARACTER_SET_SCHEMA|字符串|  
|CHARACTER_SET_NAME|字符串|  
|COLLATION_CATALOG|字符串|  
|COLLATION_SCHEMA|字符串|  
|COLLATION_NAME|字符串|  
|DOMAIN_CATALOG|字符串|  
|DOMAIN_SCHEMA|字符串|  
|DOMAIN_NAME|字符串|  
|DESCRIPTION|字符串|  
  
### <a name="procedures"></a>过程  
  
|ColumnName|数据类型|  
|----------------|--------------|  
|PROCEDURE_CATALOG|字符串|  
|PROCEDURE_SCHEMA|字符串|  
|PROCEDURE_NAME|字符串|  
|PROCEDURE_TYPE|Int16|  
|PROCEDURE_DEFINITION|字符串|  
|DESCRIPTION|字符串|  
|DATE_CREATED|DateTime|  
|DATE_MODIFIED|DateTime|  
  
### <a name="procedurecolumns"></a>ProcedureColumns  
  
|ColumnName|数据类型|  
|----------------|--------------|  
|PROCEDURE_CATALOG|字符串|  
|PROCEDURE_SCHEMA|字符串|  
|PROCEDURE_NAME|字符串|  
|COLUMN_NAME|字符串|  
|COLUMN_GUID|Guid|  
|COLUMN_PROPID|Int64|  
|ROWSET_NUMBER|Int64|  
|ORDINAL_POSITION|Int64|  
|IS_NULLABLE|Boolean|  
|DATA_TYPE|Int32|  
|TYPE_GUID|Guid|  
|CHARACTER_MAXIMUM_LENGTH|Int64|  
|CHARACTER_OCTET_LENGTH|Int64|  
|NUMERIC_PRECISION|Int32|  
|NUMERIC_SCALE|Int16|  
|DESCRIPTION|字符串|  
|OVERLOAD|Int16|  
  
### <a name="views"></a>视图  
  
|ColumnName|数据类型|  
|----------------|--------------|  
|TABLE_CATALOG|字符串|  
|TABLE_SCHEMA|字符串|  
|TABLE_NAME|字符串|  
|VIEW_DEFINITION|字符串|  
|CHECK_OPTION|Boolean|  
|IS_UPDATABLE|Boolean|  
|DESCRIPTION|字符串|  
|DATE_CREATED|DateTime|  
|DATE_MODIFIED|DateTime|  
  
### <a name="indexes"></a>索引  
  
|ColumnName|数据类型|  
|----------------|--------------|  
|TABLE_CATALOG|字符串|  
|TABLE_SCHEMA|字符串|  
|TABLE_NAME|字符串|  
|INDEX_CATALOG|字符串|  
|INDEX_SCHEMA|字符串|  
|INDEX_NAME|字符串|  
|PRIMARY_KEY|Boolean|  
|UNIQUE|Boolean|  
|CLUSTERED|Boolean|  
|TYPE|Int32|  
|FILL_FACTOR|Int32|  
|INITIAL_SIZE|Int32|  
|NULLS|Int32|  
|SORT_BOOKMARKS|Boolean|  
|AUTO_UPDATE|Boolean|  
|NULL_COLLATION|Int32|  
|ORDINAL_POSITION|Int64|  
|COLUMN_NAME|字符串|  
|COLUMN_GUID|Guid|  
|COLUMN_PROPID|Int64|  
|COLLATION|Int16|  
|CARDINALITY|小数|  
|PAGES|Int32|  
|FILTER_CONDITION|字符串|  
|INTEGRATED|Boolean|  
  
## <a name="microsoft-jet-ole-db-provider"></a>Microsoft Jet OLE DB       

 除了通用架构集合之外，Microsoft Jet OLE DB 驱动程序还支持下列特定的架构集合：  
  
- 表  
  
- 列  
  
- 过程  
  
- 视图  
  
- 索引  
  
### <a name="tables"></a>表  
  
|ColumnName|数据类型|  
|----------------|--------------|  
|TABLE_CATALOG|字符串|  
|TABLE_SCHEMA|字符串|  
|TABLE_NAME|字符串|  
|TABLE_TYPE|字符串|  
|TABLE_GUID|Guid|  
|DESCRIPTION|字符串|  
|TABLE_PROPID|Int64|  
|DATE_CREATED|DateTime|  
|DATE_MODIFIED|DateTime|  
  
### <a name="columns"></a>列  
  
|ColumnName|数据类型|  
|----------------|--------------|  
|TABLE_CATALOG|字符串|  
|TABLE_SCHEMA|字符串|  
|TABLE_NAME|字符串|  
|COLUMN_NAME|字符串|  
|COLUMN_GUID|Guid|  
|COLUMN_PROPID|Int64|  
|ORDINAL_POSITION|Int64|  
|COLUMN_HASDEFAULT|Boolean|  
|COLUMN_DEFAULT|字符串|  
|COLUMN_FLAGS|Int64|  
|IS_NULLABLE|Boolean|  
|DATA_TYPE|Int32|  
|TYPE_GUID|Guid|  
|CHARACTER_MAXIMUM_LENGTH|Int64|  
|CHARACTER_OCTET_LENGTH|Int64|  
|NUMERIC_PRECISION|Int32|  
|NUMERIC_SCALE|Int16|  
|DATETIME_PRECISION|Int64|  
|CHARACTER_SET_CATALOG|字符串|  
|CHARACTER_SET_SCHEMA|字符串|  
|CHARACTER_SET_NAME|字符串|  
|COLLATION_CATALOG|字符串|  
|COLLATION_SCHEMA|字符串|  
|COLLATION_NAME|字符串|  
|DOMAIN_CATALOG|字符串|  
|DOMAIN_SCHEMA|字符串|  
|DOMAIN_NAME|字符串|  
|DESCRIPTION|字符串|  
  
### <a name="procedures"></a>过程  
  
|ColumnName|数据类型|  
|----------------|--------------|  
|PROCEDURE_CATALOG|字符串|  
|PROCEDURE_SCHEMA|字符串|  
|PROCEDURE_NAME|字符串|  
|PROCEDURE_TYPE|Int16|  
|PROCEDURE_DEFINITION|字符串|  
|DESCRIPTION|字符串|  
|DATE_CREATED|DateTime|  
|DATE_MODIFIED|DateTime|  
  
### <a name="views"></a>视图  
  
|ColumnName|数据类型|  
|----------------|--------------|  
|TABLE_CATALOG|字符串|  
|TABLE_SCHEMA|字符串|  
|TABLE_NAME|字符串|  
|VIEW_DEFINITION|字符串|  
|CHECK_OPTION|Boolean|  
|IS_UPDATABLE|Boolean|  
|DESCRIPTION|字符串|  
|DATE_CREATED|DateTime|  
|DATE_MODIFIED|DateTime|  
  
### <a name="indexes"></a>索引  
  
|ColumnName|数据类型|  
|----------------|--------------|  
|TABLE_CATALOG|字符串|  
|TABLE_SCHEMA|字符串|  
|TABLE_NAME|字符串|  
|INDEX_CATALOG|字符串|  
|INDEX_SCHEMA|字符串|  
|INDEX_NAME|字符串|  
|PRIMARY_KEY|Boolean|  
|UNIQUE|Boolean|  
|CLUSTERED|Boolean|  
|TYPE|Int32|  
|FILL_FACTOR|Int32|  
|INITIAL_SIZE|Int32|  
|NULLS|Int32|  
|SORT_BOOKMARKS|Boolean|  
|AUTO_UPDATE|Boolean|  
|NULL_COLLATION|Int32|  
|ORDINAL_POSITION|Int64|  
|COLUMN_NAME|字符串|  
|COLUMN_GUID|Guid|  
|COLUMN_PROPID|Int64|  
|COLLATION|Int16|  
|CARDINALITY|小数|  
|PAGES|Int32|  
|FILTER_CONDITION|字符串|  
|INTEGRATED|Boolean|  
  
## <a name="see-also"></a>请参阅

- [ADO.NET 概述](ado-net-overview.md)
