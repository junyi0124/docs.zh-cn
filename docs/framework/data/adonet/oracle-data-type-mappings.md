---
title: Oracle 数据类型映射
ms.date: 03/30/2017
ms.assetid: ec34ae21-bbbb-4adb-b672-83865e2a8451
ms.openlocfilehash: 71a85f82ea3535cf7aec8dd92fbda6726a36fb81
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91166568"
---
# <a name="oracle-data-type-mappings"></a>Oracle 数据类型映射

下表列出 Oracle 数据类型及其与 <xref:System.Data.OracleClient.OracleDataReader> 的映射。  
  
|Oracle 数据类型|由 OracleDataReader.GetValue 返回的 .NET Framework 数据类型|由 OracleDataReader.GetOracleValue 返回的 OracleClient 数据类型|备注|  
|----------------------|--------------------------------------------------------------------|------------------------------------------------------------------------|-------------|  
|**BFILE**|**Byte []**|<xref:System.Data.OracleClient.OracleBFile>||  
|**BLOB**|**Byte []**|<xref:System.Data.OracleClient.OracleLob>||  
|**CHAR**|**字符串**|<xref:System.Data.OracleClient.OracleString>||  
|**CLOB**|**字符串**|<xref:System.Data.OracleClient.OracleLob>||  
|DATE|**DateTime**|<xref:System.Data.OracleClient.OracleDateTime>||  
|**FLOAT**|**小数**|<xref:System.Data.OracleClient.OracleNumber>|此数据类型是 **数字** 数据类型的别名，旨在使返回的是 <xref:System.Data.OracleClient.OracleDataReader> **Decimal** 或 <xref:System.Data.OracleClient.OracleNumber> 而不是浮点值。 使用该 .NET Framework 数据类型可能导致溢出。|  
|**INTEGER**|**小数**|<xref:System.Data.OracleClient.OracleNumber>|此数据类型是 **数字 (38) ** 数据类型的别名，旨在使返回的是 <xref:System.Data.OracleClient.OracleDataReader> **Decimal** 或 <xref:System.Data.OracleClient.OracleNumber> 而不是整数值。 使用该 .NET Framework 数据类型可能导致溢出。|  
|**INTERVAL YEAR TO MONTH**|**Int32**|<xref:System.Data.OracleClient.OracleMonthSpan>||  
|**INTERVAL DAY TO SECOND**|**TimeSpan**|<xref:System.Data.OracleClient.OracleTimeSpan>||  
|**LONG**|**字符串**|<xref:System.Data.OracleClient.OracleString>||  
|**LONG RAW**|**Byte []**|<xref:System.Data.OracleClient.OracleBinary>||  
|**NCHAR**|**字符串**|<xref:System.Data.OracleClient.OracleString>||  
|**NCLOB**|**字符串**|<xref:System.Data.OracleClient.OracleLob>||  
|**多种**|**小数**|<xref:System.Data.OracleClient.OracleNumber>|使用该 .NET Framework 数据类型可能导致溢出。|  
|**NVARCHAR2**|**字符串**|<xref:System.Data.OracleClient.OracleString>||  
|**RAW**|**Byte []**|<xref:System.Data.OracleClient.OracleBinary>||  
|**REF CURSOR**|||对象不支持 Oracle **REF CURSOR** 数据类型 <xref:System.Data.OracleClient.OracleDataReader> 。|  
|**ROWID**|**字符串**|<xref:System.Data.OracleClient.OracleString>||  
|**TIMESTAMP**|**DateTime**|<xref:System.Data.OracleClient.OracleDateTime>||  
|**TIMESTAMP WITH LOCAL TIME ZONE**|**DateTime**|<xref:System.Data.OracleClient.OracleDateTime>||  
|**TIMESTAMP WITH TIME ZONE**|**DateTime**|<xref:System.Data.OracleClient.OracleDateTime>||  
|**无符号整数**|数字 |<xref:System.Data.OracleClient.OracleNumber>|此数据类型是 **数字 (38) ** 数据类型的别名，旨在使返回的是 <xref:System.Data.OracleClient.OracleDataReader> **系统 Decimal** ， <xref:System.Data.OracleClient.OracleNumber> 而不是无符号整数值。 使用该 .NET Framework 数据类型可能导致溢出。|  
|**VARCHAR2**|**字符串**|<xref:System.Data.OracleClient.OracleString>||  
  
 下表列出了 Oracle 数据类型以及在将它们作为参数绑定时要使用的 .NET Framework 数据类型)  (的数据类型。 **System.Data.DbType** <xref:System.Data.OracleClient.OracleType>  
  
|Oracle 数据类型|要绑定为参数的 DbType 枚举|要绑定为参数的 OracleType 枚举|备注|  
|----------------------|-----------------------------------------------|---------------------------------------------------|-------------|  
|**BFILE**||**BFile**|Oracle 只允许将 **bfile** 绑定为 **bfile** 参数。 如果尝试绑定非**BFILE** 值（如 **byte []** 或），则适用于 Oracle 的 .net 数据提供程序不会自动构造一个 <xref:System.Data.OracleClient.OracleBinary> 。|  
|**BLOB**||**Blob**|Oracle 只允许将 **blob** 作为 **blob** 参数进行绑定。 如果尝试绑定非**BLOB** 值（如 **byte []** 或），则适用于 Oracle 的 .net 数据提供程序不会自动构造一个 <xref:System.Data.OracleClient.OracleBinary> 。|  
|**CHAR**|**AnsiStringFixedLength**|**Char**||  
|**CLOB**||**Clob**|Oracle 只允许将 **CLOB** 绑定为 **CLOB** 参数。 如果您尝试绑定一个非**CLOB** 值（如 **system.string** 或），则用于 Oracle 的 .net 数据提供程序不会自动构造一个 <xref:System.Data.OracleClient.OracleString> 。|  
|DATE|**DateTime**|**DateTime**||  
|**FLOAT**|**Single、Double、Decimal**|**Float、Double、Number**|<xref:System.Data.OracleClient.OracleParameter.Size%2A> 确定 **system.object** 和 <xref:System.Data.OracleClient.OracleType> 。|  
|**INTEGER**|**SByte、Int16、Int32、Int64、Decimal**|**SByte、Int16、Int32、Number**|<xref:System.Data.OracleClient.OracleParameter.Size%2A> 确定 **system.object** 和 <xref:System.Data.OracleClient.OracleType> 。|  
|**INTERVAL YEAR TO MONTH**|**Int32**|**IntervalYearToMonth**|只有在同时使用 Oracle 9i 客户端和服务器软件时，<xref:System.Data.OracleClient.OracleType> 才可用。|  
|**INTERVAL DAY TO SECOND**|**Object**|**IntervalDayToSecond**|只有在同时使用 Oracle 9i 客户端和服务器软件时，<xref:System.Data.OracleClient.OracleType> 才可用。|  
|**LONG**|**AnsiString**|**LongVarChar**||  
|**LONG RAW**|**二进制**|**LongRaw**||  
|**NCHAR**|**StringFixedLength**|**NChar**||  
|**NCLOB**||**NClob**|Oracle 只允许将 **NCLOB** 绑定为 **NCLOB** 参数。 如果您尝试绑定一个非**NCLOB** 值（如 **system.string** 或），则用于 Oracle 的 .net 数据提供程序不会自动构造一个 <xref:System.Data.OracleClient.OracleString> 。|  
|**多种**|**VarNumeric**|数字 ||  
|**NVARCHAR2**|**字符串**|**NVarChar**||  
|**RAW**|**二进制**|**原始**||  
|**REF CURSOR**||**游标**|有关详细信息，请参阅 [ORACLE REF](oracle-ref-cursors.md)cursor。|  
|**ROWID**|**AnsiString**|**Rowid**||  
|**TIMESTAMP**|**DateTime**|**Timestamp**|只有在同时使用 Oracle 9i 客户端和服务器软件时，<xref:System.Data.OracleClient.OracleType> 才可用。|  
|**TIMESTAMP WITH LOCAL TIME ZONE**|**DateTime**|**TimestampLocal**|只有在同时使用 Oracle 9i 客户端和服务器软件时，<xref:System.Data.OracleClient.OracleType> 才可用。|  
|**TIMESTAMP WITH TIME ZONE**|**DateTime**|**TimestampWithTz**|只有在同时使用 Oracle 9i 客户端和服务器软件时，<xref:System.Data.OracleClient.OracleType> 才可用。|  
|**无符号整数**|**Byte、UInt16、UInt32、UInt64、Decimal**|**Byte、UInt16、Uint32、Number**|<xref:System.Data.OracleClient.OracleParameter.Size%2A> 确定 **system.object** 和 <xref:System.Data.OracleClient.OracleType> 。|  
|**VARCHAR2**|**AnsiString**|**VarChar**||  
  
 对象的属性使用的 **InputOutput**、 **Output**和 **ReturnValue** **ParameterDirection** 值 <xref:System.Data.OracleClient.OracleParameter.Value%2A> <xref:System.Data.OracleClient.OracleParameter> 是 .NET Framework 数据类型，除非输入值是 Oracle 数据类型 (例如 <xref:System.Data.OracleClient.OracleNumber> 或 <xref:System.Data.OracleClient.OracleString>) 。 这不适用于 **REF CURSOR**、 **BFILE**或 **LOB** 数据类型。  
  
## <a name="see-also"></a>请参阅

- [Oracle 和 ADO.NET](oracle-and-adonet.md)
- [ADO.NET 概述](ado-net-overview.md)
