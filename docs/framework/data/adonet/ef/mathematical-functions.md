---
title: 数学函数
ms.date: 03/30/2017
ms.assetid: b040c7cb-156d-40f2-9152-61065b18148c
ms.openlocfilehash: 4512aaa2eeb3e715a1d6f7a8655912eb15162124
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2020
ms.locfileid: "79149760"
---
# <a name="mathematical-functions"></a>数学函数

SQL Server .NET Framework 数据提供程序 (SqlClient) 提供了各种数学函数，这些函数针对作为自变量提供的输入值执行计算并返回数值结果。 这些函数位于 SqlServer 命名空间中，该命名空间在您使用 SqlClient 时可用。 提供程序的命名空间属性使实体框架可以确定此提供程序对特定构造（如类型和函数）使用哪个前缀。 下表描述了 SqlClient 数学函数。  
  
## <a name="absexpression"></a>ABS（表达式）

执行绝对值函数。

**参数**

`expression`：`Int32`、`Int64`、`Double` 或 `Decimal`。

**返回值**

指定表达式的绝对值。

**示例**

`SqlServer.ABS(-2)`

## <a name="acosexpression"></a>ACOS（表达式）

返回指定表达式的反余弦值。

**参数**

`expression`：`Double`。

**返回值**

一个 `Double`。

**示例**

`SqlServer.ACOS(.9)`

## <a name="asinexpression"></a>ASIN（表达式）

返回指定表达式的反正弦值。

**参数**

`expression`：`Double`。

**返回值**

一个 `Double`。

**示例**

`SqlServer.ASIN(.9)`

## <a name="atanexpression"></a>ATAN（表达式）

返回指定数值表达式的反正切值。

**参数**

`expression`：`Double`。

**返回值**

一个 `Double`。

**示例**

`SqlServer.ATAN(9)`

## <a name="atn2expression-expression"></a>ATN2（表达式、表达式）

返回以弧度表示的角度，其正切介于两个指定的数值表达式之间。

**参数**

`expression`：`Double`。

**返回值**

一个 `Double`。

**示例**

`SqlServer.ATN2(9, 8)`

## <a name="ceilingexpression"></a>天花板（表达式）

将指定表达式转换为大于或等于该表达式的最小整数。

**参数**

`expression`：`Int32`、`Int64`、`Double` 或 `Decimal`。

**返回值**

`Int32`、、`Int64`或`Double``Decimal`。

**示例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_CEILING](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_ceiling)]

## <a name="cosexpression"></a>COS（表达式）

计算以弧度表示的指定角度的三角余弦。

**参数**

`expression`：`Double`。

**返回值**

一个 `Double`。

**示例**

`SqlServer.COS(45)`

## <a name="cotexpression"></a>COT（表达式）

计算以弧度表示的指定角度的三角余切。

**参数**

`expression`：`Double`。

**返回值**

一个 `Double`。

**示例**

`SqlServer.COT(60)`
  
## <a name="degreesradians"></a>度（辐射）

返回以度为单位的对应角度。

**参数**

`expression`：`Int32`、`Int64`、`Double` 或 `Decimal`。

**返回值**

`Int32`、、`Int64`或`Double``Decimal`。

**示例**

`SqlServer.DEGREES(3.1)`

## <a name="expexpression"></a>EXP（表达式）

计算指定数值表达式的指数值。

**参数**

`expression`：`Double`。

**返回值**

一个 `Double`。

**示例**`SqlServer.EXP(1)`

## <a name="floorexpression"></a>地板（表达式）

将指定表达式转换为小于或等于该表达式的最大整数。

**参数**

`expression`：`Double`。

**返回值**

一个 `Double`。

**示例**

[!code-sql[DP EntityServices Concepts#SQLSERVER_FLOOR](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#sqlserver_floor)]

## <a name="logexpression"></a>日志（表达式）

计算指定 `float` 表达式的自然对数。

**参数**

`expression`：`Double`。

**返回值**

一个 `Double`。

**示例**

`SqlServer.LOG(100)`

## <a name="log10expression"></a>LOG10（表达式）

返回指定 `Double` 表达式的以 10 为底的对数。

**参数**

`expression`：`Double`。

**返回值**

一个 `Double`。

**示例**

`SqlServer.LOG10(100)`

## <a name="pi"></a>PI（）

以 `Double` 格式返回 pi 的常量值。

**返回值**

一个 `Double`。

**示例**

`SqlServer.PI()`

## <a name="powernumeric_expression-power_expression"></a>电源（numeric_expression，power_expression）

计算指定表达式的指定幂的值。

**参数**

|  |  |
|--|--|
|`numeric_expression`| `Int32`、、`Int64`或`Double``Decimal`。|
|`power_expression`| `Double`，表示对 `numeric_expression` 进行幂运算的幂值。|

**返回值**

指定 `numeric_expression` 的指定 `power_expression` 次幂的值。

**示例**

`SqlServer.POWER(2,7)`

## <a name="radiansexpression"></a>拉迪安斯（表达式）

将度转换为弧度。

**参数**

`expression`：`Int32`、`Int64`、`Double` 或 `Decimal`。

**返回值**

`Int32`、、`Int64`或`Double``Decimal`。

**示例**

`SqlServer.RADIANS(360.0)`

## <a name="randseed"></a>兰德（[种子]）

返回介于 0 和 1 之间的随机值。

**参数**

种子值为`Int32`。 如果未指定种子，则 SQL Server 数据库引擎将随机分配种子值。 对于指定的种子值，返回的结果始终相同。

**返回值**

介于 0 和 1 之间的随机 `Double` 值。

**示例**

`SqlServer.RAND()`
  
## <a name="roundnumeric_expression-lengthfunction"></a>ROUND（numeric_expression、长度[、功能]）

返回一个舍入到指定长度或精度的数值表达式。

**参数**

|  |  |
|--|--|
|`numeric_expression`| `Int32`、、`Int64`或`Double``Decimal`。
|`length`| 表示 `Int32` 要舍入到的精度的 `numeric_expression`。 如果 `length` 为正数，则将 `numeric_expression` 舍入到 `length` 指定的小数位数。 如果 `length` 为负数，则将 `numeric_expression` 向小数点左边舍入 `length` 指定的长度。|
|`function` | 可选。 表示`Int32`要执行的操作类型的 的 。 当省略函数或值为 0（默认值）时，`numeric_expression`将四舍五入。 如果指定了 0 以外的值，则将截断 `numeric_expression`。 |

**返回值**

指定 `numeric_expression` 的指定 `power_expression` 次幂的值。

**示例**

`SqlServer.ROUND(748.58, -3)`

## <a name="signexpression"></a>符号（表达式）

返回指定表达式的正号 (+1)、零 (0) 或负号 (-1)。

**参数**

`expression`：`Int32`、`Int64`、`Double` 或 `Decimal`

**返回值**

`Int32`、、`Int64`或`Double``Decimal`。

**示例**

`SqlServer.SIGN(-10)`

## <a name="sinexpression"></a>SIN（表达式）

计算以弧度表示的指定角度的三角正弦并返回 `Double` 表达式。

**参数**

`expression`：`Double`。

**返回值**

一个 `Double`。

**示例**`SqlServer.SIN(20)`

## <a name="sqrtexpression"></a>SQRT（表达式）

返回指定表达式的平方根。

**参数**

`expression`：`Double`。

**返回值**

一个 `Double`。

**示例**`SqlServer.SQRT(3600)`

## <a name="squareexpression"></a>格（表达式）

返回指定表达式的平方。

**参数**

`expression`：`Double`。

**返回值**

一个 `Double`。

**示例**

`SqlServer.SQUARE(25)`

## <a name="tanexpression"></a>TAN（表达式）

计算指定表达式的正切。

**参数**

`expression`: `Double`

**返回值**

`Double`

**示例**

`SqlServer.TAN(45.0)`
  
## <a name="see-also"></a>另请参阅

- [数学函数 (Transact-SQL)](/sql/t-sql/functions/mathematical-functions-transact-sql)
- [用于实体框架函数的 SqlClient](sqlclient-for-ef-functions.md)
