---
description: from 子句 - C# 参考
title: from 子句 - C# 参考
ms.date: 07/20/2015
f1_keywords:
- from_CSharpKeyword
- from
helpviewer_keywords:
- from clause [C#]
- from keyword [C#]
ms.assetid: 1aefd18c-1314-47f8-99ec-9bcefb09e699
ms.openlocfilehash: 474b22f5a9d8f12c8a4365159817f878761b563c
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "89140784"
---
# <a name="from-clause-c-reference"></a>from 子句（C# 参考）

查询表达式必须以 `from` 子句开头。 此外，查询表达式可包含也以 `from` 子句开头的子查询。 `from` 子句指定下列各项：

- 将在其上运行查询或子查询的数据源。

- 表示源序列中每个元素的本地范围变量。

范围变量和数据源已强类型化。 `from` 子句中引用的数据源必须具有 <xref:System.Collections.IEnumerable>、<xref:System.Collections.Generic.IEnumerable%601> 类型之一，或 <xref:System.Linq.IQueryable%601> 等派生类型。

在下面的示例中，`numbers` 是数据源，`num` 是范围变量。 请注意，这两个变量都已强类型化，即使使用 [var](var.md) 关键字也是如此。

[!code-csharp[cscsrefQueryKeywords#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/From.cs#1)]

## <a name="the-range-variable"></a>范围变量

数据源实现 <xref:System.Collections.Generic.IEnumerable%601> 时，编译器推断范围变量的类型。 例如，如果源具有 `IEnumerable<Customer>` 类型，则范围变量会被推断为 `Customer`。 仅在以下情况下必须显式指定类型：源是 <xref:System.Collections.ArrayList> 等非泛型 `IEnumerable` 类型时。 有关详细信息，请参阅[如何使用 LINQ 查询 ArrayList](../../programming-guide/concepts/linq/how-to-query-an-arraylist-with-linq.md)。

在以上示例中，`num` 推断为 `int` 类型。 由于强类型化了范围变量，所以可以在其上调用方法，或将其用于其他操作中。 例如，不再编写 `select num`，而编写 `select num.ToString()`，使查询表达式返回字符串序列，而不是整数序列。 或者可以编写 `select num + 10`，使表达式返回序列 14、11、13、12、10。 有关详细信息，请参阅 [select 子句](select-clause.md)。

范围变量与 [foreach](foreach-in.md) 语句中的迭代变量类似，一项非常重要的区别除外：范围变量从不实际存储来自源的数据。 它只是一种语法上的便利，让查询能够描述执行查询时将发生的情况。 有关详细信息，请参阅 [LINQ 查询简介 (C#)](../../programming-guide/concepts/linq/introduction-to-linq-queries.md)。

## <a name="compound-from-clauses"></a>复合 from 子句

在某些情况下，源序列中的每个元素可能本身就是一个序列，或者包含一个序列。 例如，数据源可能是 `IEnumerable<Student>`，其中序列中的每个学生对象都包含测试分数的列表。 要访问每个 `Student` 元素的内部列表，可以使用复合 `from` 子句。 这种方法类似于使用嵌套的 [foreach](foreach-in.md) 语句。 可以向任一 `from`子句添加 [where](partial-method.md) 或 [orderby](orderby-clause.md) 子句筛选结果。 下面的示例演示 `Student` 对象的序列，其中每个对象都包含一个整数内部 `List`，表示测验分数。 访问内部列表可使用复合 `from` 子句。 如有必要，可以在这两个 `from` 子句间插入子句。

[!code-csharp[cscsrefQueryKeywords#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/From.cs#2)]

## <a name="using-multiple-from-clauses-to-perform-joins"></a>使用多个 from 子句执行联接

复合 `from` 子句用于访问单个数据源中的内部集合。 但是，查询也可以包含多个 `from` 子句，这些子句从独立的数据源生成补充查询。 通过此方法，可以执行使用 [join 子句](join-clause.md) 无法实现的某类联接操作。

下面的示例演示两个 `from` 子句如何用于形成两个数据源的完全交叉联接。

[!code-csharp[cscsrefQueryKeywords#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/From.cs#3)]

若要详细了解使用多个 `from` 子句的联接操作，请参阅[执行左外部联结](../../linq/perform-left-outer-joins.md)。

## <a name="see-also"></a>另请参阅

- [查询关键字 (LINQ)](query-keywords.md)
- [语言集成查询 (LINQ)](../../linq/index.md)
