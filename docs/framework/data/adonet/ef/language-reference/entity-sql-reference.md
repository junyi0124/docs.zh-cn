---
title: 实体 SQL 引用
ms.date: 03/30/2017
ms.assetid: 61ce7ee1-ffe2-477d-8a9f-835b0a11d900
ms.openlocfilehash: 987aa5c05b88d684e050721077d704b29e546aab
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90542119"
---
# <a name="entity-sql-reference"></a>实体 SQL 引用

本部分包含实体 SQL 参考文章。 本文按类别汇总并分组实体 SQL 运算符。

## <a name="arithmetic-operators"></a>算术运算符

算术运算符对两个表达式执行数学运算，这两个表达式可以是一个或多个数值数据类型。 下表列出了实体 SQL 算术运算符：

|运算符|使用|
|--------------|---------|
|[+（加）](add.md)|加。|
|[/（除）](divide-entity-sql.md)|除。|
|[%（取模）](modulo-entity-sql.md)|返回除法运算的余数。|
|[*（乘）](multiply-entity-sql.md)|乘。|
|[-（负）](negative-entity-sql.md)|求反。|
|[-（减）](subtract-entity-sql.md)|减。|

## <a name="canonical-functions"></a>规范函数

所有数据提供程序都支持规范函数，并且所有查询技术都可以使用规范函数。 下表列出了规范函数：

|函数|类型|
|--------------|----------|
|[聚合 Entity SQL 规范函数](aggregate-canonical-functions.md)|讨论聚合实体 SQL 规范函数。|
|[数学规范函数](math-canonical-functions.md)|讨论数学实体 SQL 规范函数。|
|[字符串规范函数](string-canonical-functions.md)|讨论字符串实体 SQL 规范函数。|
|[日期和时间规范函数](date-and-time-canonical-functions.md)|讨论实体 SQL 规范函数的日期和时间。|
|[按位规范函数](bitwise-canonical-functions.md)|讨论按位实体 SQL 规范函数。|
|[其他规范函数](other-canonical-functions.md)|讨论未分类为按位、日期/时间、字符串、数字或聚合函数的函数。|

## <a name="comparison-operators"></a>比较运算符

比较运算符适用于下列类型：`Byte`、`Int16`、`Int32`、`Int64`、`Double`、`Single`、`Decimal`、`String`、`DateTime`、`Date`、`Time`、`DateTimeOffset`。 应用比较运算符之前将对操作数进行隐性类型升级。 比较运算符总是生成布尔值。 如果操作数中至少有一个 `null`，则结果为 `null`。

相等和不相等适用于有标识的所有对象类型，如 `Boolean` 类型。 拥有相同标识的非基元对象被视为相等。 下表列出了实体 SQL 比较运算符：

|运算符|说明|
|--------------|-----------------|
|[=（等于）](equals-entity-sql.md)|比较两个表达式是否相等。|
|[>（大于）](greater-than-entity-sql.md)|比较两个表达式以确定左侧表达式的值是否大于右侧表达式的值。|
|[>=（大于或等于）](greater-than-or-equal-to-entity-sql.md)|比较两个表达式以确定左侧表达式的值是否大于或等于右侧表达式的值。|
|[不 \[ 为 \] NULL](isnull-entity-sql.md)|确定查询表达式是否为 null。|
|[<（小于）](less-than-entity-sql.md)|比较两个表达式以确定左侧表达式的值是否小于右侧表达式的值。|
|[<=（小于或等于）](less-than-or-equal-to-entity-sql.md)|比较两个表达式以确定左侧表达式的值是否小于或等于右侧表达式的值。|
|[\[不 \] 介于](between-entity-sql.md)|确定表达式的结果值是否在指定范围内。|
|[\!= (不等于) ](not-equal-to-entity-sql.md)|比较两个表达式以确定左侧表达式是否不等于右侧表达式。|
|[\[不 \] 类似于](like-entity-sql.md)|确定特定字符串是否与指定模式相匹配。|

## <a name="logical-and-case-expression-operators"></a>逻辑和 case 表达式运算符

逻辑运算符测试条件的真实性。 CASE 表达式计算一组布尔表达式的值以确定结果。 下表列出了逻辑 and CASE 表达式运算符：

|运算符|说明|
|--------------|-----------------|
|[ (逻辑与)&&  ](and-entity-sql.md)|逻辑 AND。|
|[\! (逻辑非) ](not-entity-sql.md)|逻辑非。|
|[&#124;&#124;  (逻辑或) ](or-entity-sql.md)|逻辑 OR。|
|[CASE](case-entity-sql.md)|求出一组布尔表达式的值以确定结果。|
|[THEN](then-entity-sql.md)|当 [when](/previous-versions/dotnet/netframework-4.0/bb387119(v=vs.100)) 子句的计算结果为 true 时的结果。|

## <a name="query-operators"></a>查询运算符

查询运算符用于定义返回实体数据的查询表达式。 下表列出了查询运算符：

|运算符|使用|
|--------------|---------|
|[FROM](from-entity-sql.md)|指定在 [SELECT](select-entity-sql.md) 语句中使用的集合。|
|[GROUP BY](group-by-entity-sql.md)|指定由查询返回的对象 ([选择](select-entity-sql.md) 要放置) 表达式的组。|
|[GroupPartition](grouppartition-entity-sql.md)|返回从聚合与之相关的组分区提取的参数值集合。|
|[HAVING](having-entity-sql.md)|指定组或聚合的搜索条件。|
|[上限](limit-entity-sql.md)|与 [ORDER BY](order-by-entity-sql.md) 子句一起使用，以执行物理分页。|
|[ORDER BY](order-by-entity-sql.md)|指定在 [SELECT](select-entity-sql.md) 语句中返回的对象上使用的排序顺序。|
|[SELECT](select-entity-sql.md)|指定投影中由查询返回的元素。|
|[略](skip-entity-sql.md)|与 [ORDER BY](order-by-entity-sql.md) 子句一起使用，以执行物理分页。|
|[TOP](top-entity-sql.md)|指定查询结果中将只返回第一组行。|
|[WHERE](where-entity-sql.md)|按条件筛选由查询返回的数据。|

## <a name="reference-operators"></a>Reference 运算符

引用是指向特定实体集中的特定实体的逻辑指针（外键）。 实体 SQL 支持以下运算符来构造、析构和导航引用：

|运算符|使用|
|--------------|---------|
|[CREATEREF](createref-entity-sql.md)|创建对实体集中的实体的引用。|
|[DEREF](deref-entity-sql.md)|取消引用一个引用值，并生成该取消引用的结果。|
|[KEY](key-entity-sql.md)|提取引用或实体表达式的键。|
|[引导](navigate-entity-sql.md)|使您可以从一个实体类型到另一个实体类型对关系进行导航|
|[REF](ref-entity-sql.md)|返回对实体实例的引用。|

## <a name="set-operators"></a>集合运算符

实体 SQL 提供各种功能强大的设置操作。 这包括类似于 Transact-sql 运算符（如 UNION、INTERSECT、EXCEPT 和 EXISTS）的集运算符。 实体 SQL 还支持重复消除 (集) 、) 中的成员资格测试 (以及联接 (联接) 的运算符。 下表列出了实体 SQL 集运算符：

|运算符|使用|
|--------------|---------|
|[ANYELEMENT](anyelement-entity-sql.md)|从多值集合中提取元素。|
|[EXCEPT](except-entity-sql.md)|返回一个集合，该集合中的所有非重复值都是从除除运算符右侧的查询表达式返回的查询表达式中的任何非重复值。|
|[\[不 \] 存在](exists-entity-sql.md)|确定集合是否为空。|
|[展](flatten-entity-sql.md)|将一个由多个集合组成的集合转换为一个平展集合。|
|[\[不 \] 在](in-entity-sql.md)|确定某个值是否与某个集合中的任何值匹配。|
|[INTERSECT](intersect-entity-sql.md)|返回 INTERSECT 操作数左右两边的两个查询表达式均返回的所有非重复值的集合。|
|[OVERLAPS](overlaps-entity-sql.md)|确定两个集合是否具有公共元素。|
|[SET](set-entity-sql.md)|用于通过生成一个新集合（其中移除了所有重复元素）将对象集合转换为一个集。|
|[UNION](union-entity-sql.md)|将两个或更多查询的结果组合成单个集合。|

## <a name="type-operators"></a>类型运算符

实体 SQL 提供了允许构造、查询和操作表达式 (值) 类型的操作。 下表列出了用于处理类型的运算符：

|运算符|使用|
|--------------|---------|
|[CAST](cast-entity-sql.md)|将一种数据类型的表达式转换为另一种数据类型的表达式。|
|[集合](collection-entity-sql.md)|在 [函数](function-entity-sql.md) 操作中用于声明实体类型或复杂类型的集合。|
|[\[不是 \]](isof-entity-sql.md)|确定表达式的类型是否为指定类型或指定类型的某个子类型。|
|[OFTYPE](oftype-entity-sql.md)|从查询表达式返回特定类型的对象集合。|
|[命名类型构造函数](named-type-constructor-entity-sql.md)|用于创建实体类型或复杂类型的实例。|
|[MULTISET](multiset-entity-sql.md)|根据值列表创建多集的实例。|
|[ROW](row-entity-sql.md)|从一个或多个值构造结构上类型化的匿名记录。|
|[TREAT](treat-entity-sql.md)|将特定基类型的对象视为指定派生类型的对象。|

## <a name="other-operators"></a>其他运算符

下表列出了其他实体 SQL 运算符：

|运算符|使用|
|--------------|---------|
|[+（字符串串联）](string-concatenation-entity-sql.md)|用于在实体 SQL 中串联字符串。|
|[. (成员访问) ](member-access-entity-sql.md)|用于访问结构化概念模型类型实例的属性或字段的值。|
|[--（注释）](comment-entity-sql.md)|包括实体 SQL 注释。|
|[FUNCTION](function-entity-sql.md)|定义可在 Entity SQL 查询中执行的内联函数。|

## <a name="see-also"></a>请参阅

- [Entity SQL 语言](entity-sql-language.md)
