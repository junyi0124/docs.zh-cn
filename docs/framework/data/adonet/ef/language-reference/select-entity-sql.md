---
title: SELECT (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 9a33bd0d-ded1-41e7-ba3c-305502755e3b
ms.openlocfilehash: 860e2a9a3e484e8d09cad282be8c0126c8235b46
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91202209"
---
# <a name="select-entity-sql"></a>SELECT (Entity SQL)

指定查询所返回的元素。  
  
## <a name="syntax"></a>语法  
  
```sql  
SELECT [ ALL | DISTINCT ] [ topSubclause ] aliasedExpr
      [{ , aliasedExpr }] FROM fromClause [ WHERE whereClause ] [ GROUP BY groupByClause [ HAVING havingClause ] ] [ ORDER BY orderByClause ]  
-- or  
SELECT VALUE [ ALL | DISTINCT ] [ topSubclause ] expr FROM fromClause [ WHERE whereClause ] [ GROUP BY groupByClause [ HAVING havingClause ] ] [ ORDER BY orderByClause  
```  
  
## <a name="arguments"></a>参数  

 ALL  
 指定结果集中可以出现重复项。 ALL 为默认值。  
  
 DISTINCT  
 指定结果集中只能出现唯一的结果。  
  
 值  
 仅允许指定一个项，且不添加到行包装上。  
  
 `topSubclause`  
 任何指示从查询返回的首批结果数的有效表达式（形式为 `top(expr)`）。  
  
 使用 [ORDER BY](order-by-entity-sql.md) 运算符的 LIMIT 参数，还可以选择结果集中的前 n 个项。  
  
 `aliasedExpr`  
 形式如下的表达式：  
  
 `expr` 作为 `identifier` &#124; `expr`  
  
 `expr`  
 文本或表达式。  
  
## <a name="remarks"></a>备注  

 在计算 [from](from-entity-sql.md)、 [GROUP BY](group-by-entity-sql.md)和 [HAVING](having-entity-sql.md) 子句之后计算 SELECT 子句。 SELECT 子句只能引用当前处于范围内的项（FROM 子句指定的范围或外部范围）。 如果指定了 GROUP BY 子句，则仅允许 SELECT 子句引用 GROUP BY 键的别名。 仅允许在聚合函数中引用 FROM 子句项。  
  
 跟在 SELECT 关键字之后的一个或多个查询表达式的列表称为“选择列表”，更加正式的名称为“投影”。 最普通的投影形式为单个查询表达式。 如果从集合 `member1` 选择一个成员 `collection1`，则会生成 `member1` 中每个对象的所有 `collection1`值的新集合（如下面的示例所示）。  
  
```sql  
SELECT collection1.member1 FROM collection1  
```  
  
 例如，如果 `customers` 是类型为 `Customer` 的集合，且该类型有类型为 `Name` 的属性 `string`，则从 `Name` 选择 `customers` 将生成一个字符串集合（如下面的示例所示）。  
  
```sql  
SELECT customers.Name FROM customers AS c  
```  
  
 也可以使用 JOIN 语法（FULL、INNER、LEFT、OUTER、ON 和 RIGHT）。 ON 是内部联接的必需语法，但不允许用于交叉联接。  
  
## <a name="row-and-value-select-clauses"></a>行和值选择子句  

 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 支持 SELECT 子句的两种变体。 第一种变体是行选择，由 SELECT 关键字标识，可用于指定一个或多个应被投影的值。由于在返回的值周围隐式添加了行包装，因此查询表达式的结果始终为行的多集。  
  
 行选择中的每个查询表达式都必须指定一个别名。 如果不指定别名，[!INCLUDE[esql](../../../../../../includes/esql-md.md)] 会尝试使用别名生成规则生成别名。  
  
 SELECT 子句的另一种变体为值选择，由 SELECT VALUE 关键字标识。 该变体仅允许指定一个值，且不添加行包装。  
  
 行选择总是可通过 VALUE SELECT 进行表示（如下面的示例所示）。  
  
```sql  
SELECT 1 AS a, "abc" AS b FROM C  
SELECT VALUE ROW(1 AS a, "abc" AS b) FROM C
```  
  
## <a name="all-and-distinct-modifiers"></a>All 和 Distinct 修饰符  

 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 中的两种 SELECT 变体都允许指定 ALL 或 DISTINCT 修饰符。 如果指定 DISTINCT 修饰符，则将从查询表达式（直到并包括 SELECT 子句）生成的集合中消除重复项。 如果指定 ALL 修饰符，则不消除重复项；ALL 为默认值。  
  
## <a name="differences-from-transact-sql"></a>与 Transact-SQL 的区别  

 与 Transact-SQL 不同， [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 不支持在 SELECT 子句中使用 * 参数。  [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 允许查询通过从 FROM 子句引用集合别名来提取出完整的记录（如下面的示例所示）。  
  
```sql  
SELECT * FROM T1, T2  
```  
  
 上一 Transact-sql 查询表达式 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 的表示方式如下所示。  
  
```sql  
SELECT a1, a2 FROM T1 AS a1, T2 AS a2  
```  
  
## <a name="example"></a>示例  

 下面的 Entity SQL 查询使用 SELECT 运算符指定查询要返回的元素。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 执行 [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md)中的过程。  
  
2. 将以下查询作为参数传递给 `ExecuteStructuralTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts#LESS](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#less)]  
  
## <a name="see-also"></a>请参阅

- [查询表达式](query-expressions-entity-sql.md)
- [实体 SQL 引用](entity-sql-reference.md)
- [TOP](top-entity-sql.md)
