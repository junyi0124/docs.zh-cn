---
title: SQL Server 函数映射的规范化概念模型
ms.date: 03/30/2017
ms.assetid: 1a2631bc-a426-4c0a-ba8d-26d9c80d39e2
ms.openlocfilehash: 495a662cbab84c2686e4c31945c30d6f82d117cb
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91153113"
---
# <a name="conceptual-model-canonical-to-sql-server-functions-mapping"></a>SQL Server 函数映射的规范化概念模型

本主题介绍概念模型规范函数如何映射到相应的 SQL Server 函数。  
  
## <a name="date-and-time-functions"></a>日期和时间函数  

 下表介绍了日期和时间函数映射：  
  
|规范函数|SQL Server 函数|  
|-------------------------|--------------------------|  
|[AddDays(expression)](./language-reference/date-and-time-canonical-functions.md)|`DATEADD(day, number, date)`|  
|[AddHours(expression)](./language-reference/date-and-time-canonical-functions.md)|`DATEADD(hour, number, date)`|  
|[AddMicroseconds(expression)](./language-reference/date-and-time-canonical-functions.md)|`DATEADD(microsecond, number, date)`|  
|[AddMilliseconds(expression)](./language-reference/date-and-time-canonical-functions.md)|`DATEADD(millisecond, number, date)`|  
|[AddMinutes(expression)](./language-reference/date-and-time-canonical-functions.md)|`DATEADD(minute, number, date)`|  
|[AddMonths(expression)](./language-reference/date-and-time-canonical-functions.md)|`DATEADD(month, number, date)`|  
|[AddNanoseconds(expression)](./language-reference/date-and-time-canonical-functions.md)|`DATEADD(nanosecond, number, date)`|  
|[AddSeconds(expression)](./language-reference/date-and-time-canonical-functions.md)|`DATEADD(second, number, date)`|  
|[AddYears(expression)](./language-reference/date-and-time-canonical-functions.md)|`DATEADD(year, number, date)`|  
|[CreateDateTime(year, month, day, hour, minute, second)](./language-reference/date-and-time-canonical-functions.md)|对于 SQL Server 2000 和 SQL Server 2005，在服务器上创建 `datetime` 带格式的值。 对于 SQL Server 2008 及更高版本，在服务器上创建 `datetime2` 值。|  
|[CreateDateTimeOffset(year, month, day, hour, minute, second, tzoffset)](./language-reference/date-and-time-canonical-functions.md)|在服务器上创建 `datetimeoffset` 带格式的值。<br /><br /> 在 SQL Server 2000 或 SQL Server 2005 中不受支持。|  
|[CreateTime(hour, minute, second)](./language-reference/date-and-time-canonical-functions.md)|在服务器上创建 `time` 带格式的值。<br /><br /> 在 SQL Server 2000 或 SQL Server 2005 中不受支持。|  
|[CurrentDateTime ( # B1 ](./language-reference/date-and-time-canonical-functions.md)|`SysDateTime()`（在 SQLServer 2008 中）。<br /><br /> `GetDate()`（在 SQLServer 2000 和 SQLServer 2005 中）。|  
|[CurrentDateTimeOffset()](./language-reference/date-and-time-canonical-functions.md)|`SysDateTimeOffset()`（在 SQL Server 2008 中）。<br /><br /> 在 SQL Server 2000 或 SQL Server 2005 中不受支持。|  
|[CurrentUtcDateTime()](./language-reference/date-and-time-canonical-functions.md)|`SysUtcDateTime()`（在 SQLServer 2008 中）。 `GetUtcDate()`（在 SQL Server 2000 和 SQL Server 2005 中）。|  
|[DayOfYear(expression)](./language-reference/date-and-time-canonical-functions.md)|`DatePart(dayofyear, expression)`|  
|[Day(expression)](./language-reference/date-and-time-canonical-functions.md)|`DatePart(day, expression)`|  
|[DiffDays(startExpression, endExpression)](./language-reference/date-and-time-canonical-functions.md)|`DATEDIFF(day, startdate, enddate)`|  
|[DiffHours(startExpression, endExpression)](./language-reference/date-and-time-canonical-functions.md)|`DATEDIFF(hour, startdate, enddate)`|  
|[DiffMicroseconds(startExpression, endExpression)](./language-reference/date-and-time-canonical-functions.md)|`DATEDIFF(microsecond, startdate, enddate)`|  
|[DiffMilliseconds(startExpression, endExpression)](./language-reference/date-and-time-canonical-functions.md)|`DATEDIFF(millisecond, startdate, enddate)`|  
|[DiffMinutes(startExpression, endExpression)](./language-reference/date-and-time-canonical-functions.md)|`DATEDIFF(minute, startdate, enddate)`|  
|[DiffNanoseconds(startExpression, endExpression)](./language-reference/date-and-time-canonical-functions.md)|`DATEDIFF(nanosecond, startdate, enddate)`|  
|[DiffSeconds(startExpression, endExpression)](./language-reference/date-and-time-canonical-functions.md)|`DATEDIFF(second, startdate, enddate)`|  
|[DiffYears(startExpression, endExpression)](./language-reference/date-and-time-canonical-functions.md)|`DATEDIFF(year, startdate, enddate)`|  
|[GetTotalOffsetMinutes(DateTimeOffset)](./language-reference/date-and-time-canonical-functions.md)|`DatePart(tzoffset, expression)`|  
|[Hour(expression)](./language-reference/date-and-time-canonical-functions.md)|`DatePart(hour, expression)`|  
|[Millisecond(expression)](./language-reference/date-and-time-canonical-functions.md)|`DatePart(millisecond, expression)`|  
|[Minute(expression)](./language-reference/date-and-time-canonical-functions.md)|`DatePart(minute, expression)`|  
|[Month(expression)](./language-reference/date-and-time-canonical-functions.md)|`DatePart(month, expression)`|  
|[Second(expression)](./language-reference/date-and-time-canonical-functions.md)|`DatePart(second, expression)`|  
|[Truncate(expression)](./language-reference/date-and-time-canonical-functions.md)|对于 SQL Server 2000 和 SQL Server 2005，将 `datetime` 在服务器上创建截断格式的值。 对于 SQL Server 2008 及更高版本，将 `datetime2` `datetimeoffset` 在服务器上创建一个截断的或值。|  
|[Year(expression)](./language-reference/date-and-time-canonical-functions.md)|`DatePart(YEAR, expression)`|  
  
## <a name="aggregate-functions"></a>聚合函数  

 下表介绍了聚合函数映射：  
  
|规范函数|SQL Server 函数|  
|-------------------------|--------------------------|  
|[Avg(expression)](./language-reference/aggregate-canonical-functions.md)|`AVG(expression)`|  
|[BigCount(expression)](./language-reference/aggregate-canonical-functions.md)|`BIGCOUNT(expression)`|  
|[Count(expression)](./language-reference/aggregate-canonical-functions.md)|`COUNT(expression)`|  
|[Min(expression)](./language-reference/aggregate-canonical-functions.md)|`MIN(expression)`|  
|[Max(expression)](./language-reference/aggregate-canonical-functions.md)|`MAX(expression)`|  
|[StDev(expression)](./language-reference/aggregate-canonical-functions.md)|`STDEV(expression)`|  
|[StDevP(expression)](./language-reference/aggregate-canonical-functions.md)|`STDEVP(expression)`|  
|[Sum(expression)](./language-reference/aggregate-canonical-functions.md)|`SUM(expression)`|  
|[Var(expression)](./language-reference/aggregate-canonical-functions.md)|`VAR(expression)`|  
|[VarP(expression)](./language-reference/aggregate-canonical-functions.md)|`VARP(expression)`|  
  
## <a name="math-functions"></a>数学函数  

 下表介绍了数学函数映射：  
  
|规范函数|SQL Server 函数|  
|-------------------------|--------------------------|  
|[Abs(value)](./language-reference/math-canonical-functions.md)|`ABS(value)`|  
|[Ceiling(value)](./language-reference/math-canonical-functions.md)|`CEILING(value)`|  
|[Floor(value)](./language-reference/math-canonical-functions.md)|`FLOOR(value)`|  
|[Power(value)](./language-reference/math-canonical-functions.md)|`POWER(value, exponent)`|  
|[Round(value)](./language-reference/math-canonical-functions.md)|`ROUND(value, digits, 0)`|  
|[去](./language-reference/math-canonical-functions.md)|`ROUND(value , digits, 1)`|  
  
## <a name="string-functions"></a>字符串函数  

 下表介绍了字符串函数映射：  
  
|规范函数|SQL Server 函数|  
|-------------------------|--------------------------|  
|[Contains(string, target)](./language-reference/string-canonical-functions.md)|`CHARINDEX(target, string)`|  
|[Concat(string1, string2)](./language-reference/string-canonical-functions.md)|string1 + string2|  
|[EndsWith(string, target)](./language-reference/string-canonical-functions.md)|`CHARINDEX(REVERSE(target), REVERSE(string)) = 1`<br /><br /> **注意**`CHARINDEX` `false` 如果 `string` 存储在固定长度字符串列中且为常量，则该函数返回 `target` 。 在这种情况下，将搜索整个字符串，包括任何填充尾随空格。 一种可行的解决方法是在将字符串传递给 `EndsWith` 函数前，将数据裁剪为固定长度字符串，如下面的示例所示：`EndsWith(TRIM(string), target)`|  
|[IndexOf(target, string2)](./language-reference/string-canonical-functions.md)|`CHARINDEX(target, string2)`|  
|[Left (string1, length)](./language-reference/string-canonical-functions.md)|`LEFT(string1, length)`|  
|[Length (string)](./language-reference/string-canonical-functions.md)|`LEN(string)`|  
|[LTrim(string)](./language-reference/string-canonical-functions.md)|`LTRIM(string)`|  
|[Right (string1, length)](./language-reference/string-canonical-functions.md)|`RIGHT (string1, length)`|  
|[Trim(string)](./language-reference/string-canonical-functions.md)|`LTRIM(RTRIM(string))`|  
|[Replace (string1, string2, string3)](./language-reference/string-canonical-functions.md)|`REPLACE(string1, string2, string3)`|  
|[反转 (字符串) ](./language-reference/string-canonical-functions.md)|`REVERSE (string)`|  
|[RTrim(string)](./language-reference/string-canonical-functions.md)|`RTRIM(string)`|  
|[StartsWith(string, target)](./language-reference/string-canonical-functions.md)|`CHARINDEX(target, string)`|  
|[Substring(string, start, length)](./language-reference/string-canonical-functions.md)|`SUBSTRING(string, start, length)`|  
|[ToLower(string)](./language-reference/string-canonical-functions.md)|`LOWER(string)`|  
|[ToUpper(string)](./language-reference/string-canonical-functions.md)|`UPPER(string)`|  
  
## <a name="bitwise-functions"></a>位函数  

 下表介绍了位函数映射：  
  
|规范函数|SQL Server 函数|  
|-------------------------|--------------------------|  
|[BitWiseAnd (value1, value2)](./language-reference/bitwise-canonical-functions.md)|value1 & value2|  
|[BitWiseNot (value)](./language-reference/bitwise-canonical-functions.md)|~value|  
|[BitWiseOr (value1, value2)](./language-reference/bitwise-canonical-functions.md)|value1 &#124; value2|  
|[BitWiseXor (value1, value2)](./language-reference/bitwise-canonical-functions.md)|value1 ^ value2|
