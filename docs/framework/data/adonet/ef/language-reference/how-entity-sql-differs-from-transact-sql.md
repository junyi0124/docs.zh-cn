---
title: Entity SQL 与 Transact-SQL 有何不同
ms.date: 03/30/2017
ms.assetid: 9c9ee36d-f294-4c8b-a196-f0114c94f559
ms.openlocfilehash: 9433e7a7ffdc3a7e32900981dca95eefde32f290
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204432"
---
# <a name="how-entity-sql-differs-from-transact-sql"></a>实体 SQL 与 Transact-sql 的区别

本文介绍实体 SQL 和 Transact-sql 之间的差异。  
  
## <a name="inheritance-and-relationships-support"></a>继承和关系支持  

 实体 SQL 可直接处理概念实体架构，并支持诸如继承和关系等概念模型功能。  
  
 使用继承时，从父类型实例集合中选择子类型的实例通常是有用的。 实体 SQL 中的 [oftype](oftype-entity-sql.md) 运算符与 c # 序列中 (类似， `oftype`) 提供此功能。  
  
## <a name="support-for-collections"></a>集合支持  

 实体 SQL 将集合视为第一类实体。 例如：  
  
- 集合表达式在 `from` 子句中有效。  
  
- `in` 和 `exists` 子查询已被一般化，以允许使用任何集合。  
  
     子查询是一种集合。 `e1 in e2` 和 `exists(e)` 是用于执行这些操作的实体 SQL 构造。  
  
- 集运算（例如 `union`、`intersect` 和 `except`）现在对集合执行运算。  
  
- 联接对集合执行运算。  
  
## <a name="support-for-expressions"></a>对表达式的支持  

 Transact-sql 具有 (表的子查询) 和 (行和列的表达式) 。  
  
 为了支持集合和嵌套集合，实体 SQL 使所有内容成为表达式。 实体 SQL 比 Transact-sql 更具可组合，则每个表达式都可在任何地方使用。 查询表达式总是产生投影类型的集合，并且可以在允许使用集合表达式的任何地方使用。 有关实体 SQL 中不支持的 Transact-sql 表达式的信息，请参阅不支持的 [表达式](unsupported-expressions-entity-sql.md)。  
  
 下面是所有有效的实体 SQL 查询：  
  
```sql  
1+2 *3  
"abc"  
row(1 as a, 2 as b)  
{ 1, 3, 5}
e1 union all e2  
set(e1)  
```  
  
## <a name="uniform-treatment-of-subqueries"></a>子查询统一处理  

 考虑到其对表的关注，Transact-sql 会对子查询执行上下文解释。 例如，`from` 子句中的子查询被视为多集（表）。 但是，`select` 子句中使用的同一子查询被视为标量子查询。 同样，运算符左侧使用的子查询被 `in` 视为标量子查询，而右侧应为多集子查询。  
  
 实体 SQL 消除了这些差异。 表达式具有不依赖于使用它的上下文的统一解释。 实体 SQL 将所有子查询视为多集子查询。 如果需要从子查询中使用标量值，实体 SQL 将提供对 `anyelement` 集合进行操作 (在此例中，子查询) ，并从集合中提取单一实例值。  
  
### <a name="avoiding-implicit-coercions-for-subqueries"></a>避免对子查询执行隐式强制转换  

 与统一处理子查询相关的一个副作用是将子查询隐式转换为标量值。 具体而言，在 Transact-sql 中，与单个字段)  (的行多重集将隐式转换为其数据类型为该字段的数据类型的标量值。  
  
 实体 SQL 不支持此隐式强制。 实体 SQL 提供 `ANYELEMENT` 运算符来从集合中提取单一实例值，并提供 `select value` 子句以避免在查询表达式中创建行包装器。  
  
## <a name="select-value-avoiding-the-implicit-row-wrapper"></a>Select Value：避免隐式行包装  

 Transact-sql 子查询中的 select 子句会在子句中的项的周围隐式创建行包装。 这意味着我们无法创建标量或对象的集合。 Transact-sql 允许在 `rowtype` 具有一个字段和相同数据类型的单一实例值之间进行隐式强制转换。  
  
 实体 SQL 提供了 `select value` 子句来跳过隐式行构造。 在 `select value` 子句中只能指定一个项。 使用这样的子句时，不会围绕子句中的项构造行包装器 `select` ，并且可能生成所需形状的集合，例如 `select value a` 。  
  
 实体 SQL 还提供了用于构造任意行的行构造函数。 `select` 采用投影中的一个或多个元素，并生成包含字段的数据记录：  
  
 `select a, b, c`  
  
## <a name="left-correlation-and-aliasing"></a>左相关与别名化  

 在 Transact-sql 中，给定作用域中的表达式 (或) 的单个 `select` 子句 `from` 不能引用前面在同一范围内定义的表达式。 SQL (的一些方言（包括 Transact-sql) ）支持子句中的有限形式 `from` 。  
  
 实体 SQL 通用化中的左关联 `from` ，并统一对待它们。 `from` 子句中的表达式无需使用额外的语法，即可引用同一子句中先前的定义（位于左侧的定义）。  
  
 实体 SQL 还对涉及子句的查询施加了额外限制 `group by` 。 此类查询的 `select` 子句和 `having` 子句中的表达式只能通过其别名引用 `group by` 关键字。 以下构造在 Transact-sql 中有效，但不在实体 SQL 中：  
  
```sql  
SELECT t.x + t.y FROM T AS t group BY t.x + t.y
```  
  
 若要在实体 SQL 执行此操作：  
  
```sql  
SELET k FROM T AS t GROUP BY (t.x + t.y) AS k
```  
  
## <a name="referencing-columns-properties-of-tables-collections"></a>引用表（集合）的列（属性）  

 实体 SQL 中的所有列引用都必须用表别名限定。 以下构造 (假设 `a` 是表) 的有效列 `T` 在 transact-sql 中有效，但在实体 SQL 中不有效。  
  
```sql  
SELECT a FROM T
```  
  
 实体 SQL 窗体是  
  
```sql  
SELECT t.a AS A FROM T AS t
```  
  
 表别名在 `from` 子句中是可选的。 表名称被用作隐式别名。 实体 SQL 还允许以下形式：  
  
```sql  
SELET Tab.a FROM Tab
```  
  
## <a name="navigation-through-objects"></a>通过对象导航  

 Transact-sql 使用 "." 表示法来引用表)  (的列。 实体 SQL 将此表示法扩展 (从编程语言中借用) ，以支持通过对象的属性进行导航。  
  
 例如，如果 `p` 是 "Person" 类型的表达式，则下面是用于引用此人地址所在城市的实体 SQL 语法。  
  
```sql  
p.Address.City
```  
  
## <a name="no-support-for-"></a>不支持 \*  

 Transact-sql 支持将非限定 \* 语法作为整行的别名，而限定 \* 语法 (t. \*) 作为该表字段的快捷方式。 此外，Transact-sql 允许) 聚合 (包含 null 的特殊计数 \* 。  
  
 实体 SQL 不支持 * 构造。 格式为的 transact-sql 查询 `select * from T` ， `select T1.* from T1, T2...` 可以分别以和实体 SQL 表示 `select value t from T as t` `select value t1 from T1 as t1, T2 as t2...` 。 此外，这些构造还处理继承（值可替换性），同时将 `select *` 变体限制到所声明类型的顶级属性。  
  
 实体 SQL 不支持 `count(*)` 聚合。 请改用 `count(0)`。  
  
## <a name="changes-to-group-by"></a>对 Group By 的更改  

 实体 SQL 支持密钥的别名 `group by` 。 `select` 子句和 `having` 子句中的表达式必须通过这些别名引用 `group by` 关键字。 例如，以下实体 SQL 语法：  
  
```sql  
SELECT k1, count(t.a), sum(t.a)
FROM T AS t
GROUP BY t.b + t.c AS k1
```  
  
 ...等效于以下 Transact-sql：  
  
```sql  
SELECT b + c, count(*), sum(a)
FROM T
GROUP BY b + c
```  
  
## <a name="collection-based-aggregates"></a>基于集合的聚合  

 实体 SQL 支持两种类型的聚合。  
  
 基于集合的合计对集合进行运算，并且产生合计结果。 它们可以出现在查询中的任何位置，并且不需要 `group by` 子句。 例如：  
  
```sql  
SELECT t.a AS a, count({1,2,3}) AS b FROM T AS t
```  
  
 实体 SQL 还支持 SQL 样式的聚合。 例如：  
  
```sql  
SELECT a, sum(t.b) FROM T AS t GROUP BY t.a AS a
```  
  
## <a name="order-by-clause-usage"></a>ORDER BY 子句用法  

Transact-sql `ORDER BY` 仅允许在最顶部的块中指定子句 `SELECT .. FROM .. WHERE` 。 在实体 SQL 中，您可以使用嵌套 `ORDER BY` 表达式，并将其放在查询中的任何位置，但不会保留嵌套查询中的排序。  
  
```sql  
-- The following query will order the results by the last name  
SELECT C1.FirstName, C1.LastName  
        FROM AdventureWorks.Contact AS C1
        ORDER BY C1.LastName  
```  
  
```sql  
-- In the following query ordering of the nested query is ignored.  
SELECT C2.FirstName, C2.LastName  
    FROM (SELECT C1.FirstName, C1.LastName  
        FROM AdventureWorks.Contact as C1  
        ORDER BY C1.LastName) as C2  
```  
  
## <a name="identifiers"></a>标识符  

 在 Transact-sql 中，标识符比较基于当前数据库的排序规则。 在实体 SQL 中，标识符始终不区分大小写且区分重音 (也就是说，实体 SQL 区分重音字符和非重音字符;例如，"a" 不等于 "ấ" ) 。 实体 SQL 将看起来相同但来自不同代码页的字母版本视为不同的字符。 有关详细信息，请参阅 [输入字符集](input-character-set-entity-sql.md)。  
  
## <a name="transact-sql-functionality-not-available-in-entity-sql"></a>Transact-SQL 功能在 Entity SQL 中不可用  

 以下 Transact-sql 功能在实体 SQL 中不可用。  
  
 DML  
 实体 SQL 当前不提供对 DML 语句 (insert、update、delete) 的支持。  
  
 DDL  
 实体 SQL 在当前版本中不支持 DDL。  
  
 命令式编程  
 与 Transact-sql 不同，实体 SQL 不提供对命令性编程的支持。 请改而使用编程语言。  
  
 分组函数  
 实体 SQL 尚不支持分组函数 (例如，多维数据集、汇总和 GROUPING_SET) 。  
  
 分析函数  
 实体 SQL 尚不 () 提供对分析功能的支持。  
  
 内置函数、运算符  
 实体 SQL 支持 Transact-sql 的内置函数和运算符的子集。 这些运算符和函数可能受到主要存储提供程序的支持。 实体 SQL 使用在提供程序清单中声明的特定于存储的函数。 此外，实体框架允许声明内置的和用户定义的现有存储函数，以供实体 SQL 使用。  
  
 提示  
 实体 SQL 不提供查询提示的机制。  
  
 查询结果批处理  
 实体 SQL 不支持批处理查询结果。 例如，以下是以批处理) 形式发送的有效 Transact-sql (：  
  
```sql  
SELECT * FROM products;
SELECT * FROM catagories;
```  
  
 但是，不支持等效的实体 SQL：  
  
```sql  
SELECT value p FROM Products AS p;
SELECT value c FROM Categories AS c;
```  
  
 实体 SQL 仅支持一个结果-每个命令生成查询语句。  
  
## <a name="see-also"></a>请参阅

- [Entity SQL 概述](entity-sql-overview.md)
- [不支持的表达式](unsupported-expressions-entity-sql.md)
