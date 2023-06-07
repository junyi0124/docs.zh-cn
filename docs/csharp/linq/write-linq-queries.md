---
title: 在 C# 中编写 LINQ 查询
description: 了解如何在 C# 中编写 LINQ 查询。
ms.date: 12/01/2016
ms.assetid: 30703f79-cf3a-4d02-b892-c95d58a1d9ed
ms.openlocfilehash: bd7da81f2873c6a25570cab32fafecc66fd98be4
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063440"
---
# <a name="write-linq-queries-in-c"></a>在 C\# 中编写 LINQ 查询

本文介绍可以用于在 C# 中编写 LINQ 查询的三种方法：

1. 使用查询语法。

2. 使用方法语法。

3. 结合使用查询语法和方法语法。

下面的示例演示使用前面列出的每种方法的一些简单 LINQ 查询。 一般情况下，规则是尽可能使用 (1)，每当需要时使用 (2) 和 (3)。

> [!NOTE]
> 这些查询对简单的内存中集合进行操作；但是，基本语法等同于在 LINQ to Entities 和 LINQ to XML 中使用的语法。

## <a name="example---query-syntax"></a>示例 - 查询语法

编写大多数查询的推荐方式是使用查询语法  创建查询表达式  。 下面的示例演示三个查询表达式。 第一个查询表达式演示如何通过应用包含 `where` 子句的条件来筛选或限制结果。 它返回源序列中值大于 7 或小于 3 的所有元素。 第二个表达式演示如何对返回的结果进行排序。 第三个表达式演示如何根据某个键对结果进行分组。 此查询基于单词的第一个字母返回两个组。

[!code-csharp[csProgGuideLINQ#5](~/samples/snippets/csharp/concepts/linq/how-to-write-linq-queries_1.cs)]

注意，查询类型为 <xref:System.Collections.Generic.IEnumerable%601>。 可以使用 `var` 编写所有这些查询，如下面的示例所示：

`var query = from num in numbers...`

在前面的每个示例中，在 `foreach` 语句或其他语句中循环访问查询变量之前，查询不会实际执行。 有关详细信息，请参阅 [LINQ 查询介绍](../programming-guide/concepts/linq/introduction-to-linq-queries.md)。

## <a name="example---method-syntax"></a>示例 - 方法语法

某些查询操作必须表示为方法调用。 最常见的此类方法是可返回单一数值的方法，例如 <xref:System.Linq.Enumerable.Sum%2A>、<xref:System.Linq.Enumerable.Max%2A>、<xref:System.Linq.Enumerable.Min%2A>、<xref:System.Linq.Enumerable.Average%2A> 等。 这些方法在任何查询中都必须始终最后一个调用，因为它们只表示单个值，不能用作其他查询操作的源。 下面的示例演示查询表达式中的方法调用：

[!code-csharp[csProgGuideLINQ#6](~/samples/snippets/csharp/concepts/linq/how-to-write-linq-queries_2.cs)]

如果方法具有 Action 或 Func 参数，则这些参数以 [lambda](../language-reference/operators/lambda-expressions.md) 表达式的形式提供，如下面的示例所示：

[!code-csharp[csProgGuideLINQ#7](~/samples/snippets/csharp/concepts/linq/how-to-write-linq-queries_3.cs)]

在上面的查询中，只有查询 #4 立即执行。 这是因为它将返回单个值，而不是泛型 <xref:System.Collections.Generic.IEnumerable%601> 集合。 该方法本身必须使用 `foreach` 才能计算其值。

上面的每个查询可以通过 [var](../language-reference/keywords/var.md) 使用隐式类型化进行编写，如下面的示例所示：

[!code-csharp[csProgGuideLINQ#8](~/samples/snippets/csharp/concepts/linq/how-to-write-linq-queries_4.cs)]

## <a name="example---mixed-query-and-method-syntax"></a>示例 - 混合查询和方法语法

此示例演示如何对查询子句的结果使用方法语法。 只需将查询表达式括在括号中，然后应用点运算符并调用方法。 在下面的示例中，查询 #7 返回对值介于 3 与 7 之间的数字进行的计数。 但是通常情况下，最好使用另一个变量存储方法调用的结果。 采用此方法时，查询不太可能与查询的结果相混淆。

[!code-csharp[csProgGuideLINQ#9](~/samples/snippets/csharp/concepts/linq/how-to-write-linq-queries_5.cs)]

由于查询 #7 返回单个值而不是集合，因此查询立即执行。

前面的查询可以通过 `var` 使用隐式类型化进行编写，如下所示：

```csharp
var numCount = (from num in numbers...
```

它可以采用方法语法进行编写，如下所示：

```csharp
var numCount = numbers.Where(n => n < 3 || n > 7).Count();
```

它可以使用显式类型化进行编写，如下所示：

```csharp
int numCount = numbers.Where(n => n < 3 || n > 7).Count();
```

## <a name="see-also"></a>另请参阅

- [演练：用 C# 编写查询](../programming-guide/concepts/linq/walkthrough-writing-queries-linq.md)
- [语言集成查询 (LINQ)](index.md)
- [where 子句](../language-reference/keywords/where-clause.md)
