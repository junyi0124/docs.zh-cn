---
title: 日期和时间规范函数
ms.date: 03/30/2017
ms.assetid: 9628b74f-1585-436a-b385-8b02ed0cdd63
ms.openlocfilehash: 9b7650990232face3a7c3673a6fb789912acf15c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91148277"
---
# <a name="date-and-time-canonical-functions"></a>日期和时间规范函数

[!INCLUDE[esql](../../../../../../includes/esql-md.md)] 提供日期和时间规范函数。  
  
## <a name="remarks"></a>备注  

 下表显示了日期和时间 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 规范函数。 `datetime` 是一个 <xref:System.DateTime> 值。  
  
|函数|描述|  
|--------------|-----------------|  
|`AddNanoseconds(expression,number)`|将指定的毫微秒 `number` 添加到 `expression`。<br /><br /> **参数**<br /><br /> `expression`：`DateTime`、`DateTimeOffset` 或 `Time`。<br /><br /> `number`: `Int32`.<br /><br /> **返回值**<br /><br /> `expression` 的类型。|  
|`AddMicroseconds(expression,number)`|将指定的微秒 `number` 添加到 `expression`。<br /><br /> **参数**<br /><br /> `expression`：`DateTime`、`DateTimeOffset` 或 `Time`。<br /><br /> `number`: `Int32`.<br /><br /> **返回值**<br /><br /> `expression` 的类型。|  
|`AddMilliseconds(expression,number)`|将指定的毫秒 `number` 添加到 `expression`。<br /><br /> **参数**<br /><br /> `expression`：`DateTime`、`DateTimeOffset` 或 `Time`。<br /><br /> `number`: `Int32`.<br /><br /> **返回值**<br /><br /> `expression` 的类型。|  
|`AddSeconds(expression,number)`|将指定的秒 `number` 添加到 `expression`。<br /><br /> **参数**<br /><br /> `expression`：`DateTime`、`DateTimeOffset` 或 `Time`。<br /><br /> `number`: `Int32`.<br /><br /> **返回值**<br /><br /> `expression` 的类型。|  
|`AddMinutes(expression,number)`|将指定的分钟 `number` 添加到 `expression`。<br /><br /> **参数**<br /><br /> `expression`：`DateTime`、`DateTimeOffset` 或 `Time`。<br /><br /> `number`: `Int32`.<br /><br /> **返回值**<br /><br /> `expression` 的类型。|  
|`AddHours(expression,number)`|将指定的小时 `number` 添加到 `expression`。<br /><br /> **参数**<br /><br /> `expression`：`DateTime`、`DateTimeOffset` 或 `Time`。<br /><br /> `number`: `Int32`.<br /><br /> **返回值**<br /><br /> `expression` 的类型。|  
|`AddDays(expression,number)`|将指定的天 `number` 添加到 `expression`。<br /><br /> **参数**<br /><br /> `expression`：`DateTime` 或 `DateTimeOffset`。<br /><br /> `number`: `Int32`.<br /><br /> **返回值**<br /><br /> `expression` 的类型。|  
|`AddMonths(expression,number)`|将指定的月份 `number` 添加到 `expression`。<br /><br /> **参数**<br /><br /> `expression`：`DateTime` 或 `DateTimeOffset`。<br /><br /> `number`: `Int32`.<br /><br /> **返回值**<br /><br /> `expression` 的类型。|  
|`AddYears(expression,number)`|将指定的年份 `number` 添加到 `expression`。<br /><br /> **参数**<br /><br /> `expression`：`DateTime` 或 `DateTimeOffset`。<br /><br /> `number`: `Int32`.<br /><br /> **返回值**<br /><br /> `expression` 的类型。|  
|`CreateDateTime(year,month,day,hour,minute,second)`|返回一个新的 `DateTime` 值作为服务器在自己的时区中的当前日期和时间。<br /><br /> **参数**<br /><br /> `year`、`month`、`day`、`hour`、`minute`：`Int16` 和 `Int32`。<br /><br /> `second`: `Double`.<br /><br /> **返回值**<br /><br /> `DateTime`。|  
|`CreateDateTimeOffset(year,month,day,hour,minute,second,tzoffset)`|返回一个新的 `DateTimeOffset` 值作为服务器相对于协调世界时 (UTC) 的当前日期和时间。<br /><br /> **参数**<br /><br /> `year`, `month`, `day`, `hour`, `minute`, `tzoffset`: `Int32`.<br /><br /> `second`: `Double`.<br /><br /> **返回值**<br /><br /> `DateTimeOffset`。|  
|`CreateTime(hour,minute,second)`|返回一个新的 `Time` 值作为当前时间。<br /><br /> **参数**<br /><br /> `hour` 和 `minute`：`Int32`。<br /><br /> `second`: `Double`.<br /><br /> **返回值**<br /><br /> `Time`。|  
|`CurrentDateTime()`|返回一个 `DateTime` 值作为服务器所在时区中的当前日期和时间。<br /><br /> **返回值**<br /><br /> `DateTime`。|  
|`CurrentDateTimeOffset()`|将当前日期、时间和偏移量作为 `DateTimeOffset` 返回。<br /><br /> **返回值**<br /><br /> `DateTimeOffset`。|  
|`CurrentUtcDateTime()`|返回一个 <xref:System.DateTime> 值，该值作为服务器在 UTS 时区中的当前日期和时间。<br /><br /> **返回值**<br /><br /> `DateTime`。|  
|`Day(expression)`|将 `expression` 的日部分作为一个介于 1 到 31 之间的 `Int32` 返回。<br /><br /> **参数**<br /><br /> `DateTime` 和 `DateTimeOffset`。<br /><br /> **返回值**<br /><br /> 一个 `Int32`。<br /><br /> **示例**<br /><br /> `-- The following example returns 12.`<br /><br /> `Day(cast('03/12/1998' as DateTime))`|  
|`DayOfYear(expression)`|将 `expression` 的日部分作为一个介于 1 到 366 之间的 `Int32` 返回，对于闰年的最后一天将返回 366。<br /><br /> **参数**<br /><br /> `DateTime` 或 `DateTimeOffset`。<br /><br /> **返回值**<br /><br /> 一个 `Int32`。|  
|`DiffNanoseconds(startExpression,endExpression)`|返回 `startExpression` 和 `endExpression` 之间的差（毫微秒）。<br /><br /> **参数**<br /><br /> `startExpression`、`endExpression`：`DateTime`、`DateTimeOffset` 或 `Time`。 **注意：** `startExpression`和 `endExpression` 必须属于同一类型。   <br /><br /> **返回值**<br /><br /> 一个 `Int32`。|  
|`DiffMilliseconds(startExpression,endExpression)`|返回 `startExpression` 和 `endExpression` 之间的差（毫秒）。<br /><br /> **参数**<br /><br /> `startExpression`、`endExpression`：`DateTime`、`DateTimeOffset` 或 `Time`。 **注意：** `startExpression`和 `endExpression` 必须属于同一类型。   <br /><br /> **返回值**<br /><br /> 一个 `Int32`。|  
|`DiffMicroseconds(startExpression,endExpression)`|返回 `startExpression` 和 `endExpression` 之间的差（微秒）。<br /><br /> **参数**<br /><br /> `startExpression`、`endExpression`：`DateTime`、`DateTimeOffset` 或 `Time`。 **注意：** `startExpression`和 `endExpression` 必须属于同一类型。   <br /><br /> **返回值**<br /><br /> 一个 `Int32`。|  
|`DiffSeconds(startExpression,endExpression)`|返回 `startExpression` 和 `endExpression` 之间的差（秒）。<br /><br /> **参数**<br /><br /> `startExpression`、`endExpression`：`DateTime`、`DateTimeOffset` 或 `Time`。 **注意：** `startExpression`和 `endExpression` 必须属于同一类型。   <br /><br /> **返回值**<br /><br /> 一个 `Int32`。|  
|`DiffMinutes(startExpression,endExpression)`|返回 `startExpression` 和 `endExpression` 之间的差（分钟）。<br /><br /> **参数**<br /><br /> `startExpression`、`endExpression`：`DateTime`、`DateTimeOffset` 或 `Time`。 **注意：** `startExpression`和 `endExpression` 必须属于同一类型。   <br /><br /> **返回值**<br /><br /> 一个 `Int32`。|  
|`DiffHours(startExpression,endExpression)`|返回 `startExpression` 和 `endExpression` 之间的差（小时）。<br /><br /> **参数**<br /><br /> `startExpression`、`endExpression`：`DateTime`、`DateTimeOffset` 或 `Time`。 **注意：** `startExpression`和 `endExpression` 必须属于同一类型。   <br /><br /> **返回值**<br /><br /> 一个 `Int32`。|  
|`DiffDays(startExpression,endExpression)`|返回 `startExpression` 和 `endExpression` 之间的差（天）。<br /><br /> **参数**<br /><br /> `startExpression`、`endExpression`：`DateTime` 或 `DateTimeOffset`。 **注意：** `startExpression`和 `endExpression` 必须属于同一类型。   <br /><br /> **返回值**<br /><br /> 一个 `Int32`。|  
|`DiffMonths(startExpression,endExpression)`|返回 `startExpression` 和 `endExpression` 之间的差（月）。<br /><br /> **参数**<br /><br /> `startExpression`、`endExpression`：`DateTime` 或 `DateTimeOffset`。 **注意：** `startExpression`和 `endExpression` 必须属于同一类型。   <br /><br /> **返回值**<br /><br /> 一个 `Int32`。|  
|`DiffYears(startExpression,endExpression)`|返回 `startExpression` 和 `endExpression` 之间的差（年）。<br /><br /> **参数**<br /><br /> `startExpression`、`endExpression`：`DateTime` 或 `DateTimeOffset`。 **注意：** `startExpression`和 `endExpression` 必须属于同一类型。   <br /><br /> **返回值**<br /><br /> 一个 `Int32`。|  
|`GetTotalOffsetMinutes(datetimeoffset)`|返回 `datetimeoffset` 相对于 GMT 偏移的分钟数。 此值通常介于 +780 到 -780 之间（+ 或 - 13 小时）。 **注意：**  仅 SQL Server 2008 支持此函数。 <br /><br /> **参数**<br /><br /> `DateTimeOffset`。<br /><br /> **返回值**<br /><br /> 一个 `Int32`。|  
|`Hour(expression)`|将 `expression` 的小时部分作为一个介于 0 到 23 之间的 `Int32` 返回。<br /><br /> **参数**<br /><br /> `DateTime, Time` 和 `DateTimeOffset`。<br /><br /> **示例**<br /><br /> `-- The following example returns 22.`<br /><br /> `Hour(cast('22:35:5' as DateTime))`|  
|`Millisecond(expression)`|将 `expression` 的毫秒部分作为一个介于 0 到 999 之间的 `Int32` 返回。<br /><br /> **参数**<br /><br /> `DateTime, Time` 和 `DateTimeOffset`。<br /><br /> **返回值**<br /><br /> 一个 `Int32`。|  
|`Minute(expression)`|将 `expression` 的分钟部分作为一个介于 0 到 59 之间的 `Int32` 返回。<br /><br /> **参数**<br /><br /> `DateTime, Time` 或 `DateTimeOffset`。<br /><br /> **返回值**<br /><br /> 一个 `Int32`。<br /><br /> **示例**<br /><br /> `-- The following example returns 35`<br /><br /> `Minute(cast('22:35:5' as DateTime))`|  
|`Month(expression)`|将 `expression` 的月份部分作为一个介于 1 到 12 之间的 `Int32` 返回。<br /><br /> **参数**<br /><br /> `DateTime` 或 `DateTimeOffset`。<br /><br /> **返回值**<br /><br /> 一个 `Int32`。<br /><br /> **示例**<br /><br /> `-- The following example returns 3.`<br /><br /> `Month(cast('03/12/1998' as DateTime))`|  
|`Second(expression)`|将 `expression` 的秒部分作为一个介于 0 到 59 之间的 `Int32` 返回。<br /><br /> **参数**<br /><br /> `DateTime, Time` 和 `DateTimeOffset`。<br /><br /> **返回值**<br /><br /> 一个 `Int32`。<br /><br /> **示例**<br /><br /> `-- The following example returns 5`<br /><br /> `Second(cast('22:35:5' as DateTime))`|  
|`TruncateTime(expression)`|返回截断了时间值的 `expression`。<br /><br /> **参数**<br /><br /> `DateTime` 或 `DateTimeOffset`。<br /><br /> **返回值**<br /><br /> `expression` 的类型。|  
|`Year(expression)`|返回的年份部分 `expression` `Int32` `YYYY` 。<br /><br /> **参数**<br /><br /> `DateTime` 和 `DateTimeOffset`。<br /><br /> **返回值**<br /><br /> 一个 `Int32`。<br /><br /> **示例**<br /><br /> `-- The following example returns 1998.`<br /><br /> `Year(cast('03/12/1998' as DateTime))`|  
  
 如果提供 `null` 输入，则这些函数返回 `null`。  
  
 Microsoft SQL 客户端托管提供程序中提供了等效功能。 有关详细信息，请参阅 [SqlClient for 实体框架函数](../sqlclient-for-ef-functions.md)。  
  
## <a name="see-also"></a>请参阅

- [规范函数](canonical-functions.md)
