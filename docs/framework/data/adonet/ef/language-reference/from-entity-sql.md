---
title: FROM (Entity SQL)
ms.date: 03/30/2017
ms.assetid: ff3e3048-0d5d-4502-ae5c-9187fcbd0514
ms.openlocfilehash: 8affac82fb1813aa0282540b5dc2f47d42234a1b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91148045"
---
# <a name="from-entity-sql"></a>FROM (Entity SQL)

指定在 [SELECT](select-entity-sql.md) 语句中使用的集合。

## <a name="syntax"></a>语法

```sql
FROM expression [ ,...n ] AS C
```

## <a name="arguments"></a>参数

`expression` \
任何可生成集合以用作 `SELECT` 语句中的源的有效查询表达式。

## <a name="remarks"></a>备注

`FROM` 子句是一个或多个 `FROM` 子句项的逗号分隔列表。 `FROM` 子句可用来为 `SELECT` 语句指定一个或多个源。 `FROM` 子句的最简单形式是单个查询表达式，标识用作 `SELECT` 语句中的源的集合和别名，如下例所示：

`FROM C as c`

## <a name="from-clause-items"></a>FROM 子句项

每个 `FROM` 子句项都引用 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 查询中的一个源集合。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 支持 `FROM` 子句项的以下类：简单的 `FROM` 子句项、`JOIN FROM` 子句项和 `APPLY FROM` 子句项。 下面几节将对这些 `FROM` 子句项逐一进行更详细地介绍。

### <a name="simple-from-clause-item"></a>简单 FROM 子句项

最简单的 `FROM` 子句项是标识一个集合和一个别名的单个表达式。 表达式可以只是实体集或子查询，或任何其他类型为集合的表达式。 以下是一个示例：

```sql
LOB.Customers as c
```

别名规范是可选的。 上面的 From 子句项的备选规范可以是：

```sql
LOB.Customers
```

如果未指定别名，[!INCLUDE[esql](../../../../../../includes/esql-md.md)] 会尝试根据集合表达式生成别名。

### <a name="join-from-clause-item"></a>JOIN FROM 子句项

`JOIN FROM` 子句项表示两个 `FROM` 子句项之间的联接。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 支持交叉联接、内部联接、左右外部联接和完全外部联接。 支持所有这些联接，这与在 Transact-sql 中受支持的方式相似。 与 Transact-sql 一样，中涉及的两个 `FROM` 子句项 `JOIN` 必须是独立的。 即，它们不能相关。 `CROSS APPLY` 或 `OUTER APPLY` 可用于这些情况。

#### <a name="cross-joins"></a>交叉联接

`CROSS JOIN` 查询表达式生成两个集合的笛卡儿积，如下例所示：

`FROM C AS c CROSS JOIN D as d`

#### <a name="inner-joins"></a>内部联接

`INNER JOIN` 生成两个集合的约束笛卡儿积，如下例所示：

`FROM C AS c [INNER] JOIN D AS d ON e`

前面的查询表达式将 `ON` 条件为 true 的每一个左侧集合元素与其右侧集合的配对元素组合起来。 如果未指定 `ON` 条件，则 `INNER JOIN` 退化为 `CROSS JOIN`。

#### <a name="left-outer-joins-and-right-outer-joins"></a>左外部联接和右外部联接

`OUTER JOIN` 查询表达式生成两个集合的约束笛卡儿积，如下例所示：

`FROM C AS c LEFT OUTER JOIN D AS d ON e`

前面的查询表达式将 `ON` 条件为 true 的每一个左侧集合元素与其右侧集合的配对元素组合起来。 如果 `ON` 条件为 false，表达式仍处理与右侧元素（值为 null）配对的单个左侧元素实例。

`RIGHT OUTER JOIN` 可以以类似方式表示。

#### <a name="full-outer-joins"></a>完全外部联接

显式 `FULL OUTER JOIN` 生成两个集合的约束笛卡儿积，如下例所示：

`FROM C AS c FULL OUTER JOIN D AS d ON e`

前面的查询表达式将 `ON` 条件为 true 的每一个左侧集合元素与其右侧集合的配对元素组合起来。 如果 `ON` 条件为 false，表达式仍处理与右侧元素（值为 null）配对的一个左侧元素实例。 此外，它还处理与左侧元素（值为 null）配对的一个左侧元素实例。

> [!NOTE]
> 为了保持与 SQL-92 的兼容性，在 Transact-sql 中，OUTER 关键字是可选的。 因此 `LEFT JOIN`、`RIGHT JOIN` 和 `FULL JOIN` 是 `LEFT OUTER JOIN`、`RIGHT OUTER JOIN` 和 `FULL OUTER JOIN` 的同义词。

### <a name="apply-clause-item"></a>APPLY 子句项

[!INCLUDE[esql](../../../../../../includes/esql-md.md)] 支持两种 `APPLY`：`CROSS APPLY` 和 `OUTER APPLY`。

`CROSS APPLY` 生成左侧集合的每个元素与通过计算右侧表达式得出的集合的元素的唯一配对。 利用 `CROSS APPLY`，右侧表达式在功能上依赖于左侧元素，如下面的关联集合示例所示：

`SELECT c, f FROM C AS c CROSS APPLY c.Assoc AS f`

`CROSS APPLY` 的行为与联接列表类似。 如果右侧表达式的计算结果为空集合，则 `CROSS APPLY` 不生成左侧元素实例的配对。

`OUTER APPLY` 与 `CROSS APPLY` 类似，区别在于当右侧表达式计算结果为空集合时，仍然生成配对。 下面是一个 `OUTER APPLY` 的示例：

`SELECT c, f FROM C AS c OUTER APPLY c.Assoc AS f`

> [!NOTE]
> 与 Transact-sql 不同，中不需要显式 unnest 步骤 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 。

> [!NOTE]
> `CROSS` 和 `OUTER APPLY` 运算符是在 SQL Server 2005 中引入的。 在某些情况下，查询管道可能生成包含 `CROSS APPLY` 和/或 `OUTER APPLY` 运算符的 Transact-SQL。 因为某些后端提供程序（包括早于 SQL Server 2005 的 SQL Server 版本）不支持这些运算符，所以不能对这些后端提供程序执行此类查询。
>
> 下面是一些可能导致输出查询中出现 `CROSS APPLY` 和/或 `OUTER APPLY`运算符的典型情况：分页相关子查询；相关子查询或导航所生成的集合上的 AnyElement；使用接受元素选择器的分组方法的 LINQ 查询；显式指定 `CROSS APPLY` 或 `OUTER APPLY` 的查询；在 `DEREF` 构造上具有 `REF` 构造的查询。

## <a name="multiple-collections-in-the-from-clause"></a>FROM 子句中的多个集合

`FROM` 子句不能包含逗号分隔的多个集合。 在这些情况下，假定集合将联接在一起。 将它们视为一个 n 向 CROSS JOIN。

在下面的示例中， `C` 和 `D` 是独立集合，但 `c.Names` 依赖于 `C` 。

```sql
FROM C AS c, D AS d, c.Names AS e
```

下面的示例与前面的示例在逻辑上是等效的：

`FROM (C AS c JOIN D AS d) CROSS APPLY c.Names AS e`

## <a name="left-correlation"></a>左相关

 `FROM` 子句中的项可以引用前面子句中指定的项。 在下面的示例中，`C` 和 `D` 是独立集合，但 `c.Names` 依赖于 `C`：

```sql
from C as c, D as d, c.Names as e
```

这在逻辑上等效于：

```sql
from (C as c join D as d) cross apply c.Names as e
```

## <a name="semantics"></a>语义

在逻辑上，`FROM` 子句中的集合假定为 `n` 向交叉联接的一部分（1 向交叉联接除外）。 `FROM` 子句中的别名是从左到右处理的，它们被添加到当前范围供以后引用。 `FROM` 子句假定生成行的多重集。 `FROM` 子句中的每一项都有一个字段，表示该集合项中的单个元素。

在逻辑上，`FROM` 子句生成类型为 Row(c, d, e) 的行多重集，其中字段 c、d 和 e 假定为元素类型 `C`、`D` 和 `c.Names`。

[!INCLUDE[esql](../../../../../../includes/esql-md.md)] 为范围中的每个简单 `FROM` 子句项都引入一个别名。 例如，在下面的 FROM 子句代码段中，引入范围的名称为 c、d 和 e。

```sql
from (C as c join D as d) cross apply c.Names as e
```

在 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] (与 transact-sql) 不同， `FROM` 子句仅将别名引入作用域。 任何对这些集合的列（属性）的引用都必须以别名进行限定。

## <a name="pulling-up-keys-from-nested-queries"></a>从嵌套查询中拉取键

某些需要从嵌套查询提取键的查询类型不受支持。 例如，下面的查询是有效的：

```sql
select c.Orders from Customers as c
```

但是，下面的查询无效，因为该嵌套查询没有任何键：

```sql
select {1} from {2, 3}
```

## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
- [查询表达式](query-expressions-entity-sql.md)
- [可以为 NULL 的结构化类型](nullable-structured-types-entity-sql.md)
