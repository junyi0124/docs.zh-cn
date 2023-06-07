---
title: 系统函数
ms.date: 03/30/2017
ms.assetid: b7c71b58-09e6-44ce-a3e5-a0fdb892fb86
ms.openlocfilehash: 0d46429ac958e6f5db4d51947669784303af783b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91156493"
---
# <a name="system-functions"></a>系统函数

SQL Server .NET Framework 数据提供程序 (SqlClient) 提供以下系统函数：  
  
|函数|描述|  
|--------------|-----------------|  
|`CHECKSUM (` `value`, [`value`, [`value`]]`)`|返回校验和值。 `CHECKSUM` 用于生成哈希索引。<br /><br /> **参数**<br /><br /> `value`：、、、、、、、、、、 `Boolean` `Byte` `Int16` `Int32` `Int64` `Single` `Decimal` `Double` `DateTime` `String` `Binary` 或 `Guid` 。 可以指定一个、两个或三个值。<br /><br /> **返回值**<br /><br /> 指定表达式的绝对值。<br /><br /> **示例**<br /><br /> `SqlServer.CHECKSUM(10,100,1000.0)`|  
|`CURRENT_TIMESTAMP ()`|以 SQL Server 内部格式对 `DateTime` 值生成当前日期和时间（在 SQL Server 2008 中精度为 7，在 SQL Server 2005 中精度为 3）。<br /><br /> **返回值**<br /><br /> 作为 `DateTime` 的当前系统日期和时间。<br /><br /> **示例**<br /><br /> `SqlServer.CURRENT_TIMESTAMP()`|  
|`CURRENT_ USER` `()`|返回当前用户的名称。<br /><br /> **返回值**<br /><br /> ASCII `String`。<br /><br /> **示例**<br /><br /> `SqlServer.CURRENT_USER()`|  
|`DATALENGTH` `(` `expression` `)`|返回用于表示任何表达式的字节数。<br /><br /> **参数**<br /><br /> `expression`：、、、、、、、、、、、、 `Boolean` `Byte` `Int16` `Int32` `Int64` `Single` `Decimal` `Double` `DateTime` `Time` `DateTimeOffset` `String` `Binary` 或 `Guid` 。<br /><br /> **返回值**<br /><br /> 作为 `Int32` 的属性的大小。<br /><br /> **示例**<br /><br /> `SELECT VALUE SqlServer.DATALENGTH(P.Name)FROM`<br /><br /> `AdventureWorksEntities.Product AS P`|  
|`HOST_NAME()`|返回工作站名。<br /><br /> **返回值**<br /><br /> Unicode `String`。<br /><br /> **示例**<br /><br /> `SqlServer.HOST_NAME()`|  
|`ISDATE(` `expression` `)`|确定输入表达式是否为有效日期。<br /><br /> **参数**<br /><br /> `expression`：、、、、、、、、、、、、 `Boolean` `Byte` `Int16` `Int32` `Int64` `Single` `Decimal` `Double` `DateTime` `Time` `DateTimeOffset` `String` `Binary` 或 `Guid` 。<br /><br /> **返回值**<br /><br /> 一个 `Int32`。 如果输入表达式为有效日期，则为一 (1)。 否则为零 (0)。<br /><br /> **示例**<br /><br /> `SqlServer.ISDATE('1/1/2006')`|  
|`ISNUMERIC(` `expression` `)`|确定表达式是否为有效的数值类型。<br /><br /> **参数**<br /><br /> `expression`：、、、、、、、、、、、、 `Boolean` `Byte` `Int16` `Int32` `Int64` `Single` `Decimal` `Double` `DateTime` `Time` `DateTimeOffset` `String` `Binary` 或 `Guid` 。<br /><br /> **返回值**<br /><br /> 一个 `Int32`。 如果输入表达式为有效日期，则为一 (1)。 否则为零 (0)。<br /><br /> **示例**<br /><br /> `SqlServer.ISNUMERIC('21')`|  
|`NEWID()`|创建 Guid 类型的唯一值。<br /><br /> **返回值**<br /><br /> `Guid`。<br /><br /> **示例**<br /><br /> `SqlServer.NEWID()`|  
|`USER_NAME(` `id` `)`|根据指定的标识号返回数据库用户名。<br /><br /> **参数**<br /><br /> `expression`：与数据库用户关联的 `Int32` 标识号。<br /><br /> **返回值**<br /><br /> Unicode `String`。<br /><br /> **示例**<br /><br /> `SqlServer.USER_NAME(0)`|  
  
 有关 SqlClient 支持的函数的详细信息 `String` ，请参阅 [ (transact-sql) 的字符串函数 ](/sql/t-sql/functions/string-functions-transact-sql)。
  
## <a name="see-also"></a>请参阅

- [Entity SQL 语言](./language-reference/entity-sql-language.md)
- [用于实体框架函数的 SqlClient](sqlclient-for-ef-functions.md)
