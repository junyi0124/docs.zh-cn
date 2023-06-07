---
title: 聚合函数（用于实体框架的 SqlClient）
ms.date: 03/30/2017
ms.assetid: 03303f01-b591-4efc-9875-f9c608edff0b
ms.openlocfilehash: 1c32ccfe18c67c9baeb7df0f981c9129b3bbc8bb
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204510"
---
# <a name="aggregate-functions-sqlclient-for-entity-framework"></a>聚合函数（用于实体框架的 SqlClient）

SQL Server .NET Framework 数据提供程序 (SqlClient) 提供聚合函数。 聚合函数对一组输入值执行计算并返回一个值。 这些函数位于 SqlServer 命名空间中，该命名空间在您使用 SqlClient 时可用。 提供程序的命名空间属性使实体框架可以确定此提供程序对特定构造（如类型和函数）使用哪个前缀。  
  
 下面是 SqlClient 聚合函数。  

## <a name="avgexpression"></a>AVG (表达式) 

返回集合中各值的平均值。 Null 值会被忽略。

**参数**

`Int32`、、 `Int64` `Double` 和 `Decimal` 。

**返回值**

`expression` 的类型。

**示例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_AVG](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_avg)]

## <a name="checksum_aggcollection"></a>CHECKSUM_AGG (集合) 

 返回集合中各值的校验和。 Null 值会被忽略。

 **参数**

 )  (集合 `Int32` 。

 **返回值**

 一个 `Int32`。

 **示例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_CHECKSUM](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_checksum)]

## <a name="countexpression"></a>计数 (表达式) 

以 `Int32` 形式返回集合中的项数。

**参数**

一个集合 \<T> ，其中 T 为以下类型之一：

|   |   |   |   |
|---|---|---|---|
|`Boolean`|`Double`|`DateTime`|`DateTimeOffset`|
|`Time`|`String`|`Binary`|`Guid` SQL Server 2000 中未返回 () |

**返回值**

一个 `Int32`。

**示例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_COUNT](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_count)]

## <a name="count_bigexpression"></a> (表达式的 COUNT_BIG) 

以 `bigint` 形式返回集合中的项数。

 **参数**

 集合 (T) ，其中 T 为以下类型之一：

 |   |   |   |   |
|---|---|---|---|
|`Boolean`|`Double`|`DateTime`|`DateTimeOffset`|
|`Time`|`String`|`Binary`|`Guid` SQL Server 2000 中未返回 () |

**返回值**

一个 `Int64`。

**示例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_COUNTBIG](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_countbig)]

## <a name="maxexpression"></a>MAX (表达式) 

返回集合中的最大值。

**参数**

集合 (T) ，其中 T 为以下类型之一：

|   |   |   |   |
|---|---|---|---|
|`Boolean`|`Double`|`DateTime`|`DateTimeOffset`|
|`Time`|`String`|`Binary`||

**返回值**

`expression` 的类型。

**示例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_MAX](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_max)]

## <a name="minexpression"></a>MIN (表达式) 

返回集合中的最小值。

**参数**

集合 (T) ，其中 T 为以下类型之一：

|   |   |   |   |
|---|---|---|---|
|`Boolean`|`Double`|`DateTime`|`DateTimeOffset`|
|`Time`|`String`|`Binary`||

**返回值**

`expression` 的类型。

**示例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_MIN](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_min)]

## <a name="stdevexpression"></a>STDEV (表达式) 

返回指定表达式中所有值的标准偏差。

**参数**

)  (集合 `Double` 。

**返回值**

`Double`。

**示例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_STDEV](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_stdev)]

## <a name="stdevpexpression"></a>STDEVP (表达式) 

返回指定表达式中所有值的总体标准偏差。

**参数**

)  (集合 `Double` 。

**返回值**

`Double`。

**示例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_STDEVP](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_stdevp)]

## <a name="sumexpression"></a>SUM (表达式) 

返回集合中所有值的总和。

**参数**

集合 (T) 其中 T 为以下类型之一： `Int32` 、 `Int64` 、 `Double` 和 `Decimal` 。

**返回值**

`expression` 的类型。

**示例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_SUM](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_sum)]

## <a name="varexpression"></a>VAR (表达式) 

返回指定表达式中所有值的方差。

**参数**

)  (集合 `Double` 。

**返回值**

`Double`。

**示例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_VAR](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_var)]

## <a name="varpexpression"></a>VARP (表达式) 

返回指定表达式中所有值的总体统计方差。

**参数**

)  (集合 `Double` 。

**返回值**

`Double`。

**示例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_VARP](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_varp)]
  
## <a name="see-also"></a>请参阅

- [聚合函数 (Transact-SQL)](/sql/t-sql/functions/aggregate-functions-transact-sql)
- [Entity SQL 语言](./language-reference/entity-sql-language.md)
- [聚合规范函数](./language-reference/aggregate-canonical-functions.md)
