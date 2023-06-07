---
title: 语言集成查询 (LINQ) (C#)
description: 了解语言集成查询 (LINQ)，查看完整查询操作的示例。
ms.date: 02/02/2017
ms.assetid: 19dd1782-905b-4a9d-a3e9-618453037fa2
ms.openlocfilehash: 967f69e94b0424e208fb675a312ab306d5e11842
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90556728"
---
# <a name="language-integrated-query-linq"></a>语言集成查询 (LINQ)

语言集成查询 (LINQ) 是一系列直接将查询功能集成到 C# 语言的技术统称。 数据查询历来都表示为简单的字符串，没有编译时类型检查或 IntelliSense 支持。 此外，需要针对每种类型的数据源了解不同的查询语言：SQL 数据库、XML 文档、各种 Web 服务等。 借助 LINQ，查询成为了最高级的语言构造，就像类、方法和事件一样。 可以使用语言关键字和熟悉的运算符针对强类型化对象集合编写查询。 LINQ 系列技术提供了针对对象 (LINQ to Objects)、关系数据库 (LINQ to SQL) 和 XML (LINQ to XML) 的一致查询体验。

对于编写查询的开发者来说，LINQ 最明显的“语言集成”部分就是查询表达式。 查询表达式采用声明性*查询语法*编写而成。 使用查询语法，可以用最少的代码对数据源执行筛选、排序和分组操作。 可使用相同的基本查询表达式模式来查询和转换 SQL 数据库、ADO .NET 数据集、XML 文档和流以及 .NET 集合中的数据。

在 C# 中可为以下对象编写 LINQ 查询：SQL Server 数据库、XML 文档、ADO.NET 数据集以及支持 <xref:System.Collections.IEnumerable> 或泛型 <xref:System.Collections.Generic.IEnumerable%601> 接口的任何对象集合。 此外，第三方也为许多 Web 服务和其他数据库实现提供了 LINQ 支持。

下面的示例展示了完整的查询操作。 完整的操作包括创建数据源、定义查询表达式和在 `foreach` 语句中执行查询。

[!code-csharp[csProgGuideLINQ#11](~/samples/snippets/csharp/concepts/linq/index_1.cs)]

下图为 Visual Studio 中的图例，显示了使用 C# 和 Visual Basic 针对 SQL Server 数据库编写的不完整 LINQ 查询，并具有完全类型检查和 IntelliSense 支持：

![显示具有 Intellisense 的 LINQ 查询的图表。](./media/introduction-to-linq/linq-query-intellisense.png)

## <a name="query-expression-overview"></a>查询表达式概述

- 查询表达式可用于查询并转换所有启用了 LINQ 的数据源中的数据。 例如，通过一个查询即可检索 SQL 数据库中的数据，并生成 XML 流作为输出。
- 查询表达式易于掌握，因为使用了许多熟悉的 C# 语言构造。
- 查询表达式中的变量全都是强类型，尽管在许多情况下，无需显式提供类型，因为编译器可以推断出。 有关详细信息，请参阅 [LINQ 查询操作中的类型关系](type-relationships-in-linq-query-operations.md)。
- 只有在循环访问查询变量后，才会执行查询（例如，在 `foreach` 语句中）。 有关详细信息，请参阅 [LINQ 查询简介](introduction-to-linq-queries.md)。
- 在编译时，查询表达式根据 C# 规范规则转换成标准查询运算符方法调用。 可使用查询语法表示的任何查询都可以使用方法语法进行表示。 不过，在大多数情况下，查询语法的可读性更高，也更为简洁。 有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/expressions.md#query-expressions)和[标准查询运算符概述](standard-query-operators-overview.md)。
- 通常，我们建议在编写 LINQ 查询时尽量使用查询语法，并在必要时尽可能使用方法语法。 这两种不同的形式在语义或性能上毫无差异。 查询表达式通常比使用方法语法编写的等同表达式更具可读性。
- 一些查询操作（如 <xref:System.Linq.Enumerable.Count%2A> 或 <xref:System.Linq.Enumerable.Max%2A>）没有等效的查询表达式子句，因此必须表示为方法调用。 可以各种方式结合使用方法语法和查询语法。 有关详细信息，请参阅 [LINQ 中的查询语法和方法语法](query-syntax-and-method-syntax-in-linq.md)。
- 查询表达式可被编译成表达式树或委托，具体视应用查询的类型而定。 <xref:System.Collections.Generic.IEnumerable%601> 查询编译为委托。 <xref:System.Linq.IQueryable> 和 <xref:System.Linq.IQueryable%601> 查询编译为表达式树。 有关详细信息，请参阅[表达式树](../../../expression-trees.md)。

## <a name="next-steps"></a>后续步骤

若要详细了解 LINQ，请先自行熟悉[查询表达式基础知识](../../../linq/query-expression-basics.md)中的一些基本概念，然后再阅读感兴趣的 LINQ 技术的相关文档：

- XML 文档：[LINQ to XML](../../../../standard/linq/linq-xml-overview.md)  
- ADO.NET 实体框架：[LINQ to Entities](../../../../framework/data/adonet/ef/language-reference/linq-to-entities.md)
- .NET 集合、文件、字符串等：[LINQ to objects](linq-to-objects.md)

若要更深入地全面了解 LINQ，请参阅 [C# 中的 LINQ](../../../linq/linq-in-csharp.md)。

若要开始在 C# 中使用 LINQ，请参阅教程[使用 LINQ](../../../tutorials/working-with-linq.md)。
