---
title: 字符串函数
ms.date: 03/30/2017
ms.assetid: 338f0c26-8aee-43eb-bd1a-ec0849a376b9
ms.openlocfilehash: f5c9696da51591b7a44de14838313112bbc46c06
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91156519"
---
# <a name="string-functions"></a>字符串函数

SQL Server .NET Framework 数据提供程序 (SqlClient) 提供了各种 `String` 函数，这些函数针对输入 `String` 执行操作并返回 `String` 或数值结果。 这些函数位于 SqlServer 命名空间中，该命名空间在您使用 SqlClient 时可用。 提供程序的命名空间属性使实体框架可以确定此提供程序对特定构造（如类型和函数）使用哪个前缀。  
  
 下表显示 SqlClient `String` 函数。  
  
|函数|描述|  
|--------------|-----------------|  
|`ASCII(expression)`|返回字符串表达式中最左侧字符的 ASCII 代码值。<br /><br /> **参数**<br /><br /> `expression`：ASCII `String` 类型的任何有效表达式。<br /><br /> **返回值**<br /><br /> 一个 `Int32`。<br /><br /> **示例**<br /><br /> `SqlServer.ASCII('A')`|  
|`CHAR(expression)`|将 `Int32` 代码转换为 ASCII 字符串。<br /><br /> **参数**<br /><br /> `expression`：`Int32`。<br /><br /> **返回值**<br /><br /> ASCII `String`。<br /><br /> **示例**<br /><br /> `SqlServer.char(97)`|  
|`CHARINDEX(expression1, expression2 [, start_location])`|返回字符串中指定表达式的起始位置。<br /><br /> **参数**<br /><br /> `expression1`：包含要查找的字符序列的表达式。 表达式可以为 String（ASCII 或 Unicode）类型或 Binary 类型。<br /><br /> `expression2`：用于搜索指定序列的表达式（通常是列）。 表达式可以为 String（ASCII 或 Unicode）类型或 Binary 类型。<br /><br /> `start_location`：（可选）Int64（在 SQL Server 2000 中不返回）或 Int32，表示在 expression2 中开始搜索 expression1 的字符位置。 如果未指定 start_location、或者指定值为负数或零，那么搜索将会开始于 expression2 的起始位置。<br /><br /> **返回值**<br /><br /> 一个 `Int32`。<br /><br /> **示例**<br /><br /> `SqlServer.CHARINDEX('h', 'habcdefgh', 2)`|  
|`DIFFERENCE(expression, expression)`|比较两个字符串的 `SOUNDEX` 值，并评估它们之间的相似性。<br /><br /> **参数**<br /><br /> ASCII 或 Unicode `String` 类型。 `expression` 可以是常量、变量或列。<br /><br /> **返回值**<br /><br /> 返回 `Int32`，表示两个字符表达式的 SOUNDEX 值之间的差异。 范围为从 0 到 4。 0 表示几乎不同或完全不同，4 表示几乎相同或完全相同。<br /><br /> **示例**<br /><br /> `// The following example returns a DIFFERENCE value of 4,`<br /><br /> `//the least possible difference or the best match.`<br /><br /> `SqlServer.DIFFERENCE('Green','Greene');`|  
|`LEFT(expression, count)`|返回字符串中从左边开始指定个数的字符。<br /><br /> **参数**<br /><br /> `expression`：Unicode 或 ASCII String 类型。 使用 CAST 函数可以显式转换 character_expression。<br /><br /> `count`：`Int64`（在 SQL Server 2000 中不返回）或 `Int32` 类型，指定将返回 character_expression 中的多少个字符。<br /><br /> **返回值**<br /><br /> Unicode 或 ASCII `String`。<br /><br /> **示例**<br /><br /> `SqlServer.LEFT('SQL Server', 4)`|  
|`LEN(expression)`|返回指定 String 表达式中的字符数，其中不包含尾随空格。<br /><br /> **参数**<br /><br /> `expression`：`String`（Unicode 或 ASCII）类型或 `Binary` 类型的表达式<br /><br /> **返回值**<br /><br /> 一个 `Int32`。<br /><br /> **示例**<br /><br /> `SqlServer.LEN('abcd')`|  
|`LOWER(expression)`|将大写字符数据转换成小写后返回 `String` 表达式。<br /><br /> **参数**<br /><br /> `expression`：`String` 类型的任何有效表达式。<br /><br /> **返回值**<br /><br /> `String`。<br /><br /> **示例**<br /><br /> `SqlServer.LOWER('AbB')`|  
|`LTRIM(expression)`|返回删除了前导空格的 `String` 表达式。<br /><br /> **参数**<br /><br /> `expression`：`String` 类型的任何有效表达式。<br /><br /> **返回值**<br /><br /> `String`。<br /><br /> **示例**<br /><br /> `SqlServer.LTRIM('   d')`|  
|`NCHAR(expression)`|根据 Unicode 标准的定义，返回具有指定的整数代码的 Unicode `String`。<br /><br /> **参数**<br /><br /> `expression`：`Int32`。<br /><br /> **返回值**<br /><br /> Unicode `String`。<br /><br /> **示例**<br /><br /> `SqlServer.NCHAR(65)`|  
|`PATINDEX('%pattern%', expression)`|返回某一模式在指定的 `String` 表达式中首次出现的开始位置。<br /><br /> **参数**<br /><br /> `'%pattern%'`：ASCII 或 Unicode `String` 类型。 可以使用通配符；但是模式的前后必须使用字符 %（搜索最前面的或最后面的字符时除外）。<br /><br /> `expression`：用于搜索指定模式的 ASCII 或 Unicode `String`。<br /><br /> **返回值**<br /><br /> 一个 `Int32`。<br /><br /> **示例**<br /><br /> `SqlServer.PATINDEX('abc', 'ab')`|  
|`QUOTENAME('char_string' [, 'quote_char'])`|返回带有分隔符的 Unicode `String`，分隔符的加入可使输入字符串成为有效的 SQL Server 2005 分隔标识符。<br /><br /> **参数**<br /><br /> `char_string`：Unicode `String`。<br /><br /> `quote_char`：用作分隔符的单字符字符串。 可以是单引号 (')、左方括号或右方括号 ([ ]) 或者英文双引号 (")。 如果未指定 `quote_char`，则使用方括号。<br /><br /> **返回值**<br /><br /> Unicode `String`。<br /><br /> **示例**<br /><br /> `SqlServer.QUOTENAME('abc[]def')`|  
|`REPLACE(expression1, expression2, expression3)`|将字符表达式替换为其他字符表达式。<br /><br /> **参数**<br /><br /> `expression1`：要在其中搜索的字符串表达式。 `expression1` 可以是 Unicode 或 ASCII 字符串类型。<br /><br /> `expression2`：要查找的子字符串。 `expression2` 可以是 Unicode 或 ASCII 字符串类型。<br /><br /> `expression3`：替换字符串。 `expression3` 可以是 Unicode 或 ASCII 字符串类型。<br /><br /> **示例**<br /><br /> `SqlServer.REPLACE('aabbcc', 'bc', 'zz')`|  
|`REPLICATE(char_expression, int_expression)`|按指定次数重复字符表达式。<br /><br /> **参数**<br /><br /> `char_expression`：Unicode 或 ASCII `String` 类型。<br /><br /> `int_expression`：`Int64`（在 SQL Server 2000 中不支持）或 `Int32`。<br /><br /> **返回值**<br /><br /> Unicode 或 ASCII `String` 类型。<br /><br /> **示例**<br /><br /> `SqlServer.REPLICATE('aa',2)`|  
|`REVERSE(expression)`|返回将输入字符串的字符顺序反转后的 Unicode 或 ASCII String。<br /><br /> **参数**<br /><br /> `expression`：Unicode 或 ASCII `String` 类型。<br /><br /> **返回值**<br /><br /> Unicode 或 ASCII `String` 类型。<br /><br /> **示例**<br /><br /> `SqlServer.REVERSE('abcd')`|  
|`RIGHT(char_expression, count)`|返回字符串中从右边开始指定个数的字符。<br /><br /> **参数**<br /><br /> `char_expression`： Unicode 或 ASCII 字符串类型。 使用 CAST 函数可以显式转换 character_expression。<br /><br /> `count`：`Int64`（在 SQL Server 2000 中不返回）或 `Int32` 类型，指定将返回 character_expression 中的多少个字符。<br /><br /> **返回值**<br /><br /> ASCII `String` 类型。<br /><br /> **示例**<br /><br /> `SqlServer.RIGHT('SQL Server', 6)`|  
|`RTRIM(expression)`|返回删除了尾随空格的 Unicode 或 ASCII String。<br /><br /> **参数**<br /><br /> `expression`：Unicode 或 ASCII `String` 类型。<br /><br /> **返回值**<br /><br /> Unicode 或 ASCII `String` 类型。<br /><br /> **示例**<br /><br /> `SqlServer.RTRIM('   d   e      ')`|  
|`SOUNDEX(expression)`|返回一个由四个字符组成的 (SOUNDEX) 代码，以评估两个字符串的相似性。**参数**<br /><br /> `expression`：Unicode 或 ASCII String 类型。<br /><br /> **返回值**<br /><br /> ASCII `String`。 一个由四个字符组成的代码 (SOUNDEX)，用于评估两个字符串的相似性。<br /><br /> **示例**<br /><br /> `Select SqlServer.SOUNDEX('Smith'), SqlServer.SOUNDEX('Smythe') FROM {1}`<br /><br /> **返回**<br /><br /> `----- -----  S530  S530`|  
|`SPACE(int_expression)`|返回由重复的空格组成的 ASCII `String`。<br /><br /> **参数**<br /><br /> `int_expression`：指示空格数的 `Int64`（在 SQL Server 2000 中不返回）或 `Int32`。<br /><br /> **返回值**<br /><br /> ASCII `String`。<br /><br /> **示例**<br /><br /> `SqlServer.SPACE(2)`|  
|`STR(float_expression [, length [, decimal]])`|返回从数值数据转换而成的 ASCII `String`。<br /><br /> **参数**<br /><br /> `float _expression`：带小数点的近似数值 (`Double`) 数据类型的表达式。<br /><br /> `length`：（可选）表示总长度的 `Int32`。 它包括小数点、符号、数字以及空格。 默认值为 10。<br /><br /> `decimal`： (可选) `Int32` ，它表示小数点右侧的位数。 小数必须小于或等于 16 位。 如果小数超过 16 位，则会将结果截断至小数点右侧 16 位。<br /><br /> **返回值**<br /><br /> ASCII `String`。<br /><br /> **示例**<br /><br /> `SqlServer.STR(212.0)`|  
|`STUFF(str_expression, start, length, str_expression_to_insert)`|在字符串表达式中，在指定的起始点删除指定长度的字符并插入另一组字符。<br /><br /> **参数**<br /><br /> `str_expression`：Unicode 或 ASCII `String`。<br /><br /> `start:``Int64`SQL Server 2000) 或值中未返回 (`Int32` ，它指定开始删除和插入的位置。<br /><br /> `length`：`Int64`（在 SQL Server 2000 中不返回）或 `Int32` 值，指定要删除的字符数。<br /><br /> `str_expression_to_insert`：Unicode 或 ASCII `String`。<br /><br /> **返回值**<br /><br /> Unicode 或 ASCII `String`。<br /><br /> **示例**<br /><br /> `SqlServer.STUFF('abcd', 2, 2, 'zz')`|  
|`SUBSTRING(str_expression, start, length)`|返回 `String` 表达式的一部分。<br /><br /> **参数**<br /><br /> `str_expression`：`String`（ASCII 或 Unicode）类型或 `Binary` 类型的表达式。<br /><br /> `start`：`Int64`（在 SQL Server 2000 中不返回）或 `Int32`，指定子字符串的开始位置。 1 指此字符串中的第一个字符。<br /><br /> `length`：`Int64`（在 SQL Server 2000 中不返回）或 `Int32`，指定将返回表达式中的多少个字符。<br /><br /> **返回值**<br /><br /> `String`（ASCII 或 Unicode）类型或 `Binary` 类型。<br /><br /> **示例**<br /><br /> `SqlServer.SUBSTRING('abcd', 2, 2)`|  
|`UNICODE(expression)`|按照 Unicode 标准的定义，返回输入表达式的第一个字符的整数值。<br /><br /> **参数**<br /><br /> `expression`：Unicode `String`。<br /><br /> **返回值**<br /><br /> 一个 `Int32`。<br /><br /> **示例**<br /><br /> `SqlServer.UNICODE('a')`|  
|`UPPER(expression)`|将小写字符数据转换成大写后返回 `String` 表达式。<br /><br /> **参数**<br /><br /> `expression`：ASCII 或 Unicode String 类型的表达式。<br /><br /> **返回值**<br /><br /> ASCII 或 Unicode `String` 类型。<br /><br /> **示例**<br /><br /> `SqlServer.UPPER('AbB')`|  
  
 有关 SqlClient 支持的函数的详细信息 `String` ，请参阅 [ (transact-sql) 的字符串函数 ](/sql/t-sql/functions/string-functions-transact-sql)。
  
## <a name="see-also"></a>请参阅

- [用于实体框架函数的 SqlClient](sqlclient-for-ef-functions.md)
- [SqlClient 中的已知问题（实体框架）](known-issues-in-sqlclient-for-entity-framework.md)
