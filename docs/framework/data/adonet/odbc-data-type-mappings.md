---
title: ODBC 数据类型映射
ms.date: 03/30/2017
ms.assetid: 43c35d32-831d-480f-a150-78f7e869d17f
ms.openlocfilehash: b08c649c148aacf4050c1f7ebcc17f79d1305e0c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91150708"
---
# <a name="odbc-data-type-mappings"></a>ODBC 数据类型映射

下表显示了针对 ODBC () 的 .NET Framework 数据提供程序中的数据类型的推断 .NET Framework 类型 <xref:System.Data.Odbc> 。 另外，还列出了 <xref:System.Data.Odbc.OdbcDataReader> 的类型化访问器方法。  
  
|ODBC 类型|.NET Framework 类型|.NET Framework 类型化访问器|  
|---------------|----------------------------------------------------------------------|--------------------------------------------------------------------------------|  
|SQL_BIGINT|Int64|GetInt64()|  
|SQL_BINARY|Byte[]|GetBytes()|  
|SQL_BIT|Boolean|GetBoolean()|  
|SQL_CHAR|字符串<br /><br /> Char[]|GetString()<br /><br /> GetChars()|  
|SQL_DECIMAL|小数|GetDecimal()|  
|SQL_DOUBLE|双精度|GetDouble()|  
|SQL_GUID|Guid|GetGuid()|  
|SQL_INTEGER|Int32|GetInt32()|  
|SQL_LONG_VARCHAR|字符串<br /><br /> Char[]|GetString()<br /><br /> GetChars()|  
|SQL_LONGVARBINARY|Byte[]|GetBytes()|  
|SQL_NUMERIC|小数|GetDecimal()|  
|SQL_REAL|Single|GetFloat()|  
|SQL_SMALLINT|Int16|GetInt16()|  
|SQL_TINYINT|Byte|GetByte()|  
|SQL_TYPE_TIMES|DateTime|GetDateTime()|  
|SQL_TYPE_TIMESTAMP|DateTime|GetDateTime()|  
|SQL_VARBINARY|Byte[]|GetBytes()|  
|SQL_WCHAR|字符串<br /><br /> Char[]|GetString()<br /><br /> GetChars()|  
|SQL_WLONGVARCHAR|字符串<br /><br /> Char[]|GetString()<br /><br /> GetChars()|  
|SQL_WVARCHAR|字符串<br /><br /> Char[]|GetString()<br /><br /> GetChars()|  
  
## <a name="see-also"></a>请参阅

- [在 ADO.NET 中检索和修改数据](retrieving-and-modifying-data.md)
- [ADO.NET 概述](ado-net-overview.md)
