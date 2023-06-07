---
title: 用于实体框架的 SqlClient 类型
ms.date: 03/30/2017
ms.assetid: f2a95ead-c845-4e97-9fb3-04b444f7ed81
ms.openlocfilehash: bca2cc0e0d9f43c51c66080f3bd38c245ce94381
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91156584"
---
# <a name="sqlclient-for-entity-frameworktypes"></a>用于实体框架的 SqlClient 类型

SQL Server .NET Framework        (SqlClient)                                                                                       
  
 下表描述了 SQL Server 2008、SQL Server 2005 和 SQL Server 2000 数据库的类型以及这些类型如何映射到概念模型类型。 在 SQL Server 的较新版本中引入的某些新类型在 SQL Server 早期版本中不受支持。 下表中注明了这些类型。  
  
|提供程序类型<br /><br /> name|提供程序类型<br /><br /> attributes|`EDMSimpleType`<br /><br /> name|Facet|  
|----------------------------|----------------------------------|------------------------------|------------|  
|`bit`|不适用|`Edm.Boolean`|不适用|  
|`tinyint`|不适用|`Edm.Byte`|不适用|  
|`smallint`|不适用|`Edm.Int16`|不适用|  
|`int`|不适用|`Edm.Int32`|不适用|  
|`bigint`|不适用|`Edm.Int64`|不适用|  
|`float`|不适用|`Edm.Double`|不适用|  
|`real`|不适用|`Edm.Double`|不适用|  
|`decimal`|不适用|`Edm.Decimal`|Precision<br /><br /> -最小值：1<br /><br /> -最大值：38<br /><br /> -默认值：18<br /><br /> -常量： False<br /><br /> 规模：<br /><br /> -最小值：0<br /><br /> -最大值：38<br /><br /> -默认值：0<br /><br /> -常量： False|  
|`numeric`|不适用|`Edm.Decimal`|Precision<br /><br /> -最小值：1<br /><br /> -最大值：38<br /><br /> -默认值：18<br /><br /> -常量： False<br /><br /> 规模：<br /><br /> -最小值：0<br /><br /> -最大值：38<br /><br /> -默认值：0<br /><br /> -常量： False|  
|`smallmoney`|不适用|`Edm.Decimal`|Precision<br /><br /> -默认值：10<br /><br /> -常量： True<br /><br /> 规模：<br /><br /> -默认值：4<br /><br /> -常量： True|  
|`money`|不适用|`Edm.Decimal`|Precision<br /><br /> -默认值：19<br /><br /> -常量： True<br /><br /> 规模：<br /><br /> -默认值：4<br /><br /> -常量： True|  
|`binary`|不适用|`Edm.Binary`|长度<br /><br /> -最小值：1<br /><br /> -最大值：8000<br /><br /> -默认值：8000<br /><br /> -常量： False<br /><br /> FixedLength<br /><br /> -默认值： True<br /><br /> -常量： True|  
|`varbinary`|不适用|`Edm.Binary`|长度<br /><br /> -最小值：1<br /><br /> -最大值：8000<br /><br /> -默认值：8000<br /><br /> -常量： False<br /><br /> FixedLength<br /><br /> -默认值： False<br /><br /> -常量： True|  
|`varbinary(max)`<br /><br /> 注意： SQL Server 2000 不支持此类型。|不适用|`Edm.Binary`|长度<br /><br /> -默认值：214748364780<br /><br /> -常量： True<br /><br /> FixedLength<br /><br /> -默认值： False<br /><br /> -常量： True|  
|`image`|不适用|`Edm.Binary`|长度<br /><br /> -默认值：2147483647<br /><br /> -常量： True<br /><br /> FixedLength<br /><br /> -默认值： False<br /><br /> -常量： True|  
|`timestamp`|不适用|`Edm.Binary`|长度<br /><br /> -默认值：8<br /><br /> -常量： True<br /><br /> FixedLength<br /><br /> -默认值： True<br /><br /> -常量： True|  
|`rowversion`|不适用|`Edm.Binary`|长度<br /><br /> -默认值：8<br /><br /> -常量： True<br /><br /> FixedLength<br /><br /> -默认值： True<br /><br /> -常量： True|  
|`smalldatetime`|不适用|`Edm.DateTime`|Precision<br /><br /> -默认值：0<br /><br /> -常量： True|  
|`datetime`|不适用|`Edm.DateTime`|Precision<br /><br /> -默认值：3<br /><br /> -常量： True|  
|`date`<br /><br /> 注意： SQL Server 2005 和 SQL Server 2000 不支持此类型。|不适用|`Edm.DateTime`|Precision<br /><br /> -默认值：0<br /><br /> -常量： False|  
|`time`<br /><br /> 注意： SQL Server 2005 和 SQL Server 2000 不支持此类型。|不适用|`Edm.Time`|Precision<br /><br /> -默认值：7<br /><br /> -常量： False|  
|`datetime2`<br /><br /> 注意： SQL Server 2005 和 SQL Server 2000 不支持此类型。|不适用|`Edm.DateTime`|Precision<br /><br /> -默认值：7<br /><br /> -常量： False|  
|`datetimeoffset`<br /><br /> 注意： SQL Server 2005 和 SQL Server 2000 不支持此类型。|不适用|`Edm.DateTimeOffset`|Precision<br /><br /> -默认值：7<br /><br /> -常量： False|  
|`nvarchar`<br /><br /> 注意： SQL Server 2000 不支持此类型。|不适用|`Edm.String`|长度<br /><br /> -最小值：1<br /><br /> -最大值：4000<br /><br /> -默认值：4000<br /><br /> -常量： False<br /><br /> Unicode：<br /><br /> -默认值： True<br /><br /> -常量： True<br /><br /> FixedLength<br /><br /> -默认值： False<br /><br /> -常量： True|  
|`varchar`<br /><br /> 注意： SQL Server 2000 不支持此类型。|不适用|`Edm.String`|长度<br /><br /> -最小值：1<br /><br /> -最大值：8000<br /><br /> -默认值：8000<br /><br /> -常量： False<br /><br /> Unicode：<br /><br /> -默认值： False<br /><br /> -常量： True<br /><br /> FixedLength<br /><br /> -默认值： False<br /><br /> -常量： True|  
|`char`|不适用|`Edm.String`|长度<br /><br /> -最小值：1<br /><br /> -最大值：8000<br /><br /> -默认值：8000<br /><br /> -常量： False<br /><br /> Unicode：<br /><br /> -默认值： False<br /><br /> -常量： True<br /><br /> FixedLength<br /><br /> -默认值： True<br /><br /> -常量： True|  
|`nchar`|不适用|`Edm.String`|长度<br /><br /> -最小值：1<br /><br /> -最大值：4000<br /><br /> -默认值：4000<br /><br /> -常量： False<br /><br /> Unicode：<br /><br /> -默认值： True<br /><br /> -常量： True<br /><br /> FixedLength<br /><br /> -默认值： True<br /><br /> -常量： True|  
|`varchar`(`max`)|不适用|`Edm.String`|长度<br /><br /> -默认值：2147483647<br /><br /> -常量： True<br /><br /> Unicode：<br /><br /> -默认值： False<br /><br /> -常量： True<br /><br /> FixedLength<br /><br /> -默认值： False<br /><br /> -常量： True|  
|`nvarchar`(`max`)|不适用|`Edm.String`|长度<br /><br /> -默认值：1073741823<br /><br /> -常量： True<br /><br /> Unicode：<br /><br /> -默认值： True<br /><br /> -常量： True<br /><br /> FixedLength<br /><br /> -默认值： False<br /><br /> -常量： True|  
|`ntext`|相等可比较： False<br /><br /> 可比较顺序： False|`Edm.String`|长度<br /><br /> -默认值：1073741823<br /><br /> -常量： True<br /><br /> Unicode：<br /><br /> -默认值： False<br /><br /> -常量： True<br /><br /> FixedLength<br /><br /> -默认值： False<br /><br /> -常量： True|  
|`text`|相等可比较： False<br /><br /> 可比较顺序： False|`Edm.String`|长度<br /><br /> -默认值：2147483647<br /><br /> -常量： True<br /><br /> Unicode：<br /><br /> -默认值： False<br /><br /> -常量： True<br /><br /> FixedLength<br /><br /> -默认值： False<br /><br /> -常量： True|  
|`Unique`<br /><br /> `identifier`|相等比较： True<br /><br /> 可比较顺序： True|`Edm.Guid`|不适用|  
|`xml`|相等可比较： False<br /><br /> 可比较顺序： False|`Edm.String`|长度<br /><br /> -默认值：1073741823<br /><br /> -常量： True<br /><br /> Unicode：<br /><br /> -默认值： True<br /><br /> -常量： True<br /><br /> FixedLength<br /><br /> -默认值： False<br /><br /> -常量： True|  
  
## <a name="see-also"></a>请参阅

- [CSDL、SSDL 和 MSL 规范](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)
