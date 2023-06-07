---
title: LINQ 中的查询语法和方法语法 (C#)
description: 了解 LINQ 中的查询语法和方法语法。 包括标准查询运算符扩展方法和 Lambda 表达式。
ms.date: 07/20/2015
helpviewer_keywords:
- LINQ [C#], query syntax vs. method syntax
- queries [LINQ in C#], syntax comparisons
ms.assetid: eedd6dd9-fec2-428c-9581-5b8783810ded
ms.openlocfilehash: 14319c7ec17c3186bfbd11875ee0d4480de7e3ff
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91157559"
---
# <a name="query-syntax-and-method-syntax-in-linq-c"></a>LINQ 中的查询语法和方法语法 (C#)

介绍性的语言集成查询 (LINQ) 文档中的大多数查询是使用 LINQ 声明性查询语法编写的。 但是在编译代码时，查询语法必须转换为针对 .NET 公共语言运行时 (CLR) 的方法调用。 这些方法调用会调用标准查询运算符（名称为 `Where`、`Select`、`GroupBy`、`Join`、`Max` 和 `Average` 等）。 可以使用方法语法（而不查询语法）来直接调用它们。  
  
 查询语法和方法语法在语义上是相同的，但是许多人发现查询语法更简单且更易于阅读。 某些查询必须表示为方法调用。 例如，必须使用方法调用表示检索与指定条件匹配的元素数的查询。 还必须对检索源序列中具有最大值的元素的查询使用方法调用。 <xref:System.Linq> 命名空间中的标准查询运算符的参考文档通常使用方法语法。 因此，即使在开始编写 LINQ 查询时，熟悉如何在查询和查询表达式本身中使用方法语法也十分有用。  
  
## <a name="standard-query-operator-extension-methods"></a>标准查询运算符扩展方法  

 下面的示例演示一个简单查询表达式以及编写为基于方法的查询的语义上等效的查询。  
  
 [!code-csharp[csLINQGettingStarted#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#22)]  
  
 这两个示例的输出是相同的。 可以看到查询变量的类型在两种形式中是相同的：<xref:System.Collections.Generic.IEnumerable%601>。  
  
 为了了解基于方法的查询，我们来仔细讨论它。 在表达式右侧，请注意，`where` 子句现在表示为 `numbers` 对象上的实例方法，它具有类型 `IEnumerable<int>`（如同你会回忆起的那样）。 如果熟悉泛型 <xref:System.Collections.Generic.IEnumerable%601> 接口，则会知道它没有 `Where` 方法。 但是，如果在 Visual Studio IDE 中调用 IntelliSense 完成列表，则不仅会看到 `Where` 方法，还会看到许多其他方法（如 `Select`、`SelectMany`、`Join` 和 `Orderby`）。 这些都是标准查询运算符。  
  
 ![显示 Intellisense 中的所有标准查询运算符的屏幕截图。](./media/query-syntax-and-method-syntax-in-linq/standard-query-operators.png)  
  
 虽然看起来似乎 <xref:System.Collections.Generic.IEnumerable%601> 进行了重新定义以包括这些其他方法，不过实际上情况并非如此。 标准查询运算符作为一种新类型的方法（称为扩展方法）来实现。 扩展方法可“扩展”现有类型；它们可以如同类型上的实例方法一样进行调用。 标准查询运算符扩展了 <xref:System.Collections.Generic.IEnumerable%601>，因此可以写入 `numbers.Where(...)`。  
  
 若要开始使用 LINQ，你在扩展方法方面实际需要了解的所有内容是如何使用正确的 `using` 指令将它们引入应用程序的范围。 从应用程序的角度来看，扩展方法与常规实例方法是相同的。  
  
 有关扩展方法的详细信息，请参阅[扩展方法](../../classes-and-structs/extension-methods.md)。 有关标准查询运算符的详细信息，请参阅[标准查询运算符概述 (C#)](./standard-query-operators-overview.md)。 某些 LINQ 提供程序（如 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] 和 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)]），会实现自己的标准查询运算符，并为 <xref:System.Collections.Generic.IEnumerable%601> 之外的其他类型实现额外的扩展方法。  
  
## <a name="lambda-expressions"></a>Lambda 表达式  

 在上面的示例中，请注意，条件表达式 (`num % 2 == 0`) 作为内联参数传递给 `Where` 方法：`Where(num => num % 2 == 0).` 此内联表达式称为 lambda 表达式。 可采用匿名方法、泛型委托或表达式树的形式编写原本必须以更繁琐的形式编写的代码，这是一种便利的方式。 在 C# 中，`=>` 是 lambda 运算符（读为“转到”）。 运算符左侧的 `num` 是输入变量，它与查询表达式中的 `num` 对应。 编译器可以推断出 `num` 的类型，因为它知道 `numbers` 是泛型 <xref:System.Collections.Generic.IEnumerable%601> 类型。 Lambda 的主体与查询语法中或任何其他 C# 表达式或语句中的表达式完全相同；它可以包含方法调用和其他复杂逻辑。 “返回值”就是表达式结果。  
  
 若要开始使用 LINQ，不必大量使用 lambda。 但是，某些查询只能采用方法语法进行表示，而其中一些查询需要 lambda 表达式。 进一步熟悉 lambda 之后，你会发现它们是 LINQ 工具箱中一种强大而灵活的工具。 有关详细信息，请参阅 [Lambda 表达式](../../../language-reference/operators/lambda-expressions.md)。  
  
## <a name="composability-of-queries"></a>查询的可组合性  

 在前面的代码示例中，请注意，`OrderBy` 方法通过对 `Where` 调用使用点运算符来调用。 `Where` 会生成经过筛选的序列，然后 `Orderby` 通过进行排序来对该序列进行操作。 由于查询返回 `IEnumerable`，因此可通过将方法调用链接在一起在方法语法中撰写查询。 这是当你使用查询语法编写查询时，编译器在幕后进行的工作。 因为查询变量不存储查询的结果，所以可以随时修改它或将它用作新查询的基础（即使在执行过它之后）。  
