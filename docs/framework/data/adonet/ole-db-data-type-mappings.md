---
title: OLE DB 数据类型映射
ms.date: 03/30/2017
ms.assetid: 04bcb259-59d3-4fd7-894d-4f0dd0c68069
ms.openlocfilehash: 7f3b498e39feac4a6fe98e739793d20e0268b8f4
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91150695"
---
# <a name="ole-db-data-type-mappings"></a>OLE DB 数据类型映射

下表显示了 ADO 的 .NET Framework 数据提供程序中的数据类型的推断 .NET Framework 类型，并 OLE DB (<xref:System.Data.OleDb>) 。 另外，还列出了 <xref:System.Data.OleDb.OleDbDataReader> 的类型化访问器方法。  
  
|ADO 类型|OLE DB 类型|.NET Framework 类型|.NET Framework 类型化访问器|  
|--------------|-----------------|----------------------------------------------------------------------|--------------------------------------------------------------------------------|  
|adBigInt|DBTYPE_I8|Int64|GetInt64()|  
|adBinary|DBTYPE_BYTES|Byte[]|GetBytes()|  
|adBoolean|DBTYPE_BOOL|Boolean|GetBoolean()|  
|adBSTR|DBTYPE_BSTR|字符串|GetString()|  
|adChapter|DBTYPE_HCHAPTER|通过 `DataReader` 支持。 请参阅 [使用 DataReader 检索数据](retrieving-data-using-a-datareader.md)。|GetValue()|  
|adChar|DBTYPE_STR|字符串|GetString()|  
|adCurrency|DBTYPE_CY|小数|GetDecimal()|  
|adDate|DBTYPE_DATE|DateTime|GetDateTime()|  
|adDBDate|DBTYPE_DBDATE|DateTime|GetDateTime()|  
|adDBTime|DBTYPE_DBTIME|DateTime|GetDateTime()|  
|adDBTimeStamp|DBTYPE_DBTIMESTAMP|DateTime|GetDateTime()|  
|adDecimal|DBTYPE_DECIMAL|小数|GetDecimal()|  
|adDouble|DBTYPE_R8|Double|GetDouble()|  
|adError|DBTYPE_ERROR|ExternalException|GetValue()|  
|adFileTime|DBTYPE_FILETIME|DateTime|GetDateTime()|  
|adGUID|DBTYPE_GUID|Guid|GetGuid()|  
|adIDispatch|DBTYPE_IDISPATCH *|对象|GetValue()|  
|adInteger|DBTYPE_I4|Int32|GetInt32()|  
|adIUnknown|DBTYPE_IUNKNOWN *|对象|GetValue()|  
|adNumeric|DBTYPE_NUMERIC|小数|GetDecimal()|  
|adPropVariant|DBTYPE_PROPVARIANT|对象|GetValue()|  
|adSingle|DBTYPE_R4|Single|GetFloat()|  
|adSmallInt|DBTYPE_I2|Int16|GetInt16()|  
|adTinyInt|DBTYPE_I1|Byte|GetByte()|  
|adUnsignedBigInt|DBTYPE_UI8|UInt64|GetValue()|  
|adUnsignedInt|DBTYPE_UI4|UInt32|GetValue()|  
|adUnsignedSmallInt|DBTYPE_UI2|UInt16|GetValue()|  
|adUnsignedTinyInt|DBTYPE_UI1|Byte|GetByte()|  
|adVariant|DBTYPE_VARIANT|对象|GetValue()|  
|adWChar|DBTYPE_WSTR|字符串|GetString()|  
|adUserDefined|DBTYPE_UDT|不支持||  
|adVarNumeric|DBTYPE_VARNUMERIC|不支持||  
  
 \* 对于 OLE DB 类型 `DBTYPE_IUNKNOWN` 和 `DBTYPE_IDISPATCH` ，对象引用是指针的封送表示形式。  
  
## <a name="see-also"></a>请参阅

- [在 ADO.NET 中检索和修改数据](retrieving-and-modifying-data.md)
- [ADO.NET 概述](ado-net-overview.md)
