---
title: 查询表达式基础（C# 中的 LINQ）
description: 与查询表达式相关的概念介绍
ms.date: 11/30/2016
ms.assetid: 027db1f8-346f-44d2-a16e-043fcea3a4e0
ms.openlocfilehash: 83beaa82d4b4b42ff9da5230edddd391b33a0717
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/14/2020
ms.locfileid: "79173349"
---
# <a name="query-expression-basics"></a>查询表达式基础

本文介绍与 C# 中的查询表达式相关的基本概念。

## <a name="what-is-a-query-and-what-does-it-do"></a>查询是什么及其作用是什么？

查询  是一组指令，描述要从给定数据源（或源）检索的数据以及返回的数据应具有的形状和组织。 查询与它生成的结果不同。

通常情况下，源数据按逻辑方式组织为相同类型的元素的序列。 例如，SQL 数据库表包含行的序列。 在 XML 文件中，存在 XML 元素的“序列”（尽管这些元素在树结构按层次结构进行组织）。 内存中集合包含对象的序列。

从应用程序的角度来看，原始源数据的特定类型和结构并不重要。 应用程序始终将源数据视为 <xref:System.Collections.Generic.IEnumerable%601> 或 <xref:System.Linq.IQueryable%601> 集合。 例如在 LINQ to XML 中，源数据显示为 `IEnumerable`\<<xref:System.Xml.Linq.XElement>>。

对于此源序列，查询可能会执行三种操作之一：

- 检索元素的子集以生成新序列，而不修改各个元素。 查询然后可能以各种方式对返回的序列进行排序或分组，如下面的示例所示（假定 `scores` 是 `int[]`）：

    [!code-csharp[csrefQueryExpBasics#45](~/samples/snippets/csharp/concepts/linq/query-expression-basics_1.cs)]

- 如前面的示例所示检索元素的序列，但是将它们转换为新类型的对象。 例如，查询可以只从数据源中的某些客户记录检索姓氏。 或者可以检索完整记录，然后用于构造其他内存中对象类型甚至是 XML 数据，再生成最终的结果序列。 下面的示例演示从 `int` 到 `string` 的投影。 请注意 `highScoresQuery` 的新类型。

    [!code-csharp[csrefQueryExpBasics#46](~/samples/snippets/csharp/concepts/linq/query-expression-basics_2.cs)]

- 检索有关源数据的单独值，如：

  - 与特定条件匹配的元素数。

  - 具有最大或最小值的元素。

  - 与某个条件匹配的第一个元素，或指定元素集中特定值的总和。 例如，下面的查询从 `scores` 整数数组返回大于 80 的分数的数量：

    [!code-csharp[csrefQueryExpBasics#47](~/samples/snippets/csharp/concepts/linq/query-expression-basics_3.cs)]

    在前面的示例中，请注意在调用 `Count` 方法之前，在查询表达式两边使用了括号。 也可以通过使用新变量存储具体结果，来表示此行为。 这种方法更具可读性，因为它使存储查询的变量与存储结果的查询分开。

    [!code-csharp[csrefQueryExpBasics#48](~/samples/snippets/csharp/concepts/linq/query-expression-basics_4.cs)]

在上面的示例中，查询在 `Count` 调用中执行，因为 `Count` 必须循环访问结果才能确定 `highScoresQuery` 返回的元素数。

## <a name="what-is-a-query-expression"></a>查询表达式是什么？

查询表达式  是以查询语法表示的查询。 查询表达式是一流的语言构造。 它如同任何其他表达式一样，可以在 C# 表达式有效的任何上下文中使用。 查询表达式由一组用类似于 SQL 或 XQuery 的声明性语法所编写的子句组成。 每个子句进而包含一个或多个 C# 表达式，而这些表达式可能本身是查询表达式或包含查询表达式。

查询表达式必须以 [from](../language-reference/keywords/from-clause.md) 子句开头，且必须以 [select](../language-reference/keywords/select-clause.md) 或 [group](../language-reference/keywords/group-clause.md) 子句结尾。 在第一个 `from` 子句与最后一个 `select` 或 `group` 子句之间，可以包含以下这些可选子句中的一个或多个：[where](../language-reference/keywords/where-clause.md)、[orderby](../language-reference/keywords/orderby-clause.md)、[join](../language-reference/keywords/join-clause.md)、[let](../language-reference/keywords/let-clause.md)，甚至是其他 [from](../language-reference/keywords/from-clause.md) 子句。 还可以使用 [into](../language-reference/keywords/into.md) 关键字，使 `join` 或 `group` 子句的结果可以充当相同查询表达式中的其他查询子句的源。

### <a name="query-variable"></a>查询变量

在 LINQ 中，查询变量是存储查询  而不是查询结果  的任何变量。 更具体地说，查询变量始终是可枚举类型，在 `foreach` 语句或对其 `IEnumerator.MoveNext` 方法的直接调用中循环访问时会生成元素序列。

下面的代码示例演示一个简单查询表达式，它具有一个数据源、一个筛选子句、一个排序子句并且不转换源元素。 该查询以 `select` 子句结尾。

[!code-csharp[csrefQueryExpBasics#49](~/samples/snippets/csharp/concepts/linq/query-expression-basics_5.cs)]

在上面的示例中，`scoreQuery` 是查询变量  ，它有时仅仅称为查询  。 查询变量不存储在 `foreach` 循环生成中的任何实际结果数据。 并且当 `foreach` 语句执行时，查询结果不会通过查询变量 `scoreQuery` 返回。 而是通过迭代变量 `testScore` 返回。 `scoreQuery` 变量可以在另一个 `foreach` 循环中进行循环访问。 只要既没有修改它，也没有修改数据源，便会生成相同结果。

查询变量可以存储采用查询语法、方法语法或是两者的组合进行表示的查询。 在以下示例中，`queryMajorCities` 和 `queryMajorCities2` 都是查询变量：

[!code-csharp[csrefQueryExpBasics#50](~/samples/snippets/csharp/concepts/linq/query-expression-basics_6.cs)]

另一方面，以下两个示例演示不是查询变量的变量（即使各自使用查询进行初始化）。 它们不是查询变量，因为它们存储结果：

[!code-csharp[csrefQueryExpBasics#51](~/samples/snippets/csharp/concepts/linq/query-expression-basics_7.cs)]

有关表示查询的不同方式的详细信息，请参阅 [LINQ 中的查询语法和方法语法](../programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq.md)。

#### <a name="explicit-and-implicit-typing-of-query-variables"></a>查询变量的显式和隐式类型化

本文档通常提供查询变量的显式类型以便显示查询变量与 [select 子句](../language-reference/keywords/select-clause.md)之间的类型关系。 但是，还可以使用 [var](../language-reference/keywords/var.md) 关键字指示编译器在编译时推断查询变量（或任何其他局部变量）的类型。 例如，本主题中前面演示的查询示例也可以使用隐式类型化进行表示：

[!code-csharp[csrefQueryExpBasics#52](~/samples/snippets/csharp/concepts/linq/query-expression-basics_8.cs)]

有关详细信息，请参阅[隐式类型化局部变量](../programming-guide/classes-and-structs/implicitly-typed-local-variables.md)和 [LINQ 查询操作中的类型关系](../programming-guide/concepts/linq/type-relationships-in-linq-query-operations.md)。

### <a name="starting-a-query-expression"></a>开始查询表达式

查询表达式必须以 `from` 子句开头。 它指定数据源以及范围变量。 范围变量表示遍历源序列时，源序列中的每个连续元素。 范围变量基于数据源中元素的类型进行强类型化。 在下面的示例中，因为 `countries` 是 `Country` 对象的数组，所以范围变量也类型化为 `Country`。 因为范围变量是强类型，所以可以使用点运算符访问该类型的任何可用成员。

[!code-csharp[csrefQueryExpBasics#53](~/samples/snippets/csharp/concepts/linq/query-expression-basics_9.cs)]

范围变量一直处于范围中，直到查询使用分号或 continuation  子句退出。

查询表达式可能会包含多个 `from` 子句。 在源序列中的每个元素本身是集合或包含集合时，可使用其他 `from` 子句。 例如，假设具有 `Country` 对象的集合，其中每个对象都包含名为 `Cities` 的 `City` 对象集合。 若要查询每个 `Country` 中的 `City` 对象，请使用两个 `from` 子句，如下所示：

[!code-csharp[csrefQueryExpBasics#54](~/samples/snippets/csharp/concepts/linq/query-expression-basics_10.cs)]

有关详细信息，请参阅 [from 子句](../language-reference/keywords/from-clause.md)。

### <a name="ending-a-query-expression"></a>结束查询表达式

查询表达式必须以 `group` 子句或 `select` 子句结尾。

#### <a name="group-clause"></a>group 子句

使用 `group` 子句可生成按指定键组织的组的序列。 键可以是任何数据类型。 例如，下面的查询会创建包含一个或多个 `Country` 对象并且其键是 `char` 值的组的序列。

[!code-csharp[csrefQueryExpBasics#55](~/samples/snippets/csharp/concepts/linq/query-expression-basics_11.cs)]

有关分组的详细信息，请参阅 [group 子句](../language-reference/keywords/group-clause.md)。

#### <a name="select-clause"></a>select 子句

使用 `select` 子句可生成所有其他类型的序列。 简单 `select` 子句只生成类型与数据源中包含的对象相同的对象的序列。 在此示例中，数据源包含 `Country` 对象。 `orderby` 子句只按新顺序对元素进行排序，而 `select` 子句生成重新排序的 `Country` 对象的序列。

[!code-csharp[csrefQueryExpBasics#56](~/samples/snippets/csharp/concepts/linq/query-expression-basics_12.cs)]

`select` 子句可以用于将源数据转换为新类型的序列。 此转换也称为投影  。 在下面的示例中，`select` 子句对只包含原始元素中的字段子集的匿名类型序列进行投影  。 请注意，新对象使用对象初始值设定项进行初始化。

[!code-csharp[csrefQueryExpBasics#57](~/samples/snippets/csharp/concepts/linq/query-expression-basics_13.cs)]

有关可以使用 `select` 子句转换源数据的所有方法的详细信息，请参阅 [select 子句](../language-reference/keywords/select-clause.md)。

#### <a name="continuations-with-into"></a>使用“into”进行延续

可以在 `select` 或 `group` 子句中使用 `into` 关键字创建存储查询的临时标识符。 如果在分组或选择操作之后必须对查询执行其他查询操作，则可以这样做。 在下面的示例中，`countries` 按 1000 万范围，根据人口进行分组。 创建这些组之后，附加子句会筛选出一些组，然后按升序对组进行排序。 若要执行这些附加操作，需要由 `countryGroup` 表示的延续。

[!code-csharp[csrefQueryExpBasics#58](~/samples/snippets/csharp/concepts/linq/query-expression-basics_14.cs)]

有关详细信息，请参阅 [into](../language-reference/keywords/into.md)。

### <a name="filtering-ordering-and-joining"></a>筛选、排序和联接

在开头 `from` 子句与结尾 `select` 或 `group` 子句之间，所有其他子句（`where`、`join`、`orderby`、`from`、`let`）都是可选的。 任何可选子句都可以在查询正文中使用零次或多次。

#### <a name="where-clause"></a>where 子句

使用 `where` 子句可基于一个或多个谓词表达式，从源数据中筛选出元素。 以下示例中的 `where` 子句具有一个谓词及两个条件。

[!code-csharp[csrefQueryExpBasics#59](~/samples/snippets/csharp/concepts/linq/query-expression-basics_15.cs)]

有关详细信息，请参阅 [where 子句](../language-reference/keywords/where-clause.md)。

#### <a name="orderby-clause"></a>orderby 子句

使用 `orderby` 子句可按升序或降序对结果进行排序。 还可以指定次要排序顺序。 下面的示例使用 `Area` 属性对 `country` 对象执行主要排序。 然后使用 `Population` 属性执行次要排序。

[!code-csharp[csrefQueryExpBasics#60](~/samples/snippets/csharp/concepts/linq/query-expression-basics_16.cs)]

`ascending` 关键字是可选的；如果未指定任何顺序，则它是默认排序顺序。 有关详细信息，请参阅 [orderby 子句](../language-reference/keywords/orderby-clause.md)。

#### <a name="join-clause"></a>join 子句

使用 `join` 子句可基于每个元素中指定的键之间的相等比较，将一个数据源中的元素与另一个数据源中的元素进行关联和/或合并。 在 LINQ 中，联接操作是对元素属于不同类型的对象序列执行。 联接了两个序列之后，必须使用 `select` 或 `group` 语句指定要存储在输出序列中的元素。 还可以使用匿名类型将每组关联元素中的属性合并到输出序列的新类型中。 下面的示例关联其 `Category` 属性与 `categories` 字符串数组中一个类别匹配的 `prod` 对象。 筛选出其 `Category` 不与 `categories` 中的任何字符串匹配的产品。`select` 语句会投影其属性取自 `cat` 和 `prod` 的新类型。

[!code-csharp[csrefQueryExpBasics#61](~/samples/snippets/csharp/concepts/linq/query-expression-basics_17.cs)]

还可以通过使用 [into](../language-reference/keywords/into.md) 关键字将 `join` 操作的结果存储到临时变量中来执行分组联接。 有关详细信息，请参阅 [join 子句](../language-reference/keywords/join-clause.md)。

#### <a name="let-clause"></a>let 子句

使用 `let` 子句可将表达式（如方法调用）的结果存储在新范围变量中。 在下面的示例中，范围变量 `firstName` 存储 `Split` 返回的字符串数组的第一个元素。

[!code-csharp[csrefQueryExpBasics#62](~/samples/snippets/csharp/concepts/linq/query-expression-basics_18.cs)]

有关详细信息，请参阅 [let 子句](../language-reference/keywords/let-clause.md)。

### <a name="subqueries-in-a-query-expression"></a>查询表达式中的子查询

查询子句本身可能包含查询表达式，这有时称为子查询  。 每个子查询都以自己的 `from` 子句开头，该子句不一定指向第一个 `from` 子句中的相同数据源。 例如，下面的查询演示在 select 语句用于检索分组操作结果的查询表达式。

[!code-csharp[csrefQueryExpBasics#63](~/samples/snippets/csharp/concepts/linq/query-expression-basics_19.cs)]

有关详细信息，请参阅[对分组操作执行子查询](perform-a-subquery-on-a-grouping-operation.md)。

## <a name="see-also"></a>请参阅

- [C# 编程指南](../programming-guide/index.md)
- [语言集成查询 (LINQ)](index.md)
- [查询关键字 (LINQ)](../language-reference/keywords/query-keywords.md)
- [标准查询运算符概述](../programming-guide/concepts/linq/standard-query-operators-overview.md)
