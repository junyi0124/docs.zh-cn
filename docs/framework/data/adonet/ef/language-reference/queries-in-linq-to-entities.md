---
title: LINQ to Entities 中的查询
description: 了解 LINQ 如何为使用编程对象跨各种类型的数据源和格式处理数据提供简单、一致的模型。
ms.date: 03/30/2017
ms.assetid: c015a609-29eb-4e95-abb1-2ca721c6e2ad
ms.openlocfilehash: 2d36d025252aa9354494d23fbcb0f96efed65bc9
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91189287"
---
# <a name="queries-in-linq-to-entities"></a>LINQ to Entities 中的查询

查询是一种从数据源检索数据的表达式。 查询通常用专用查询语言表示，如用于关系数据库的 SQL 和用于 XML 的 XQuery。 因此，开发人员对于他们查询的每种类型的数据源或数据格式，都不得不学习一种新的查询语言。 语言集成查询 (LINQ) 为跨各种数据源和格式处理数据提供了一种更简单的、一致的模型。 在 LINQ 查询中，您始终可以使用编程对象。  
  
 LINQ 查询操作包含三个操作：获得一个或多个数据源、创建查询并执行查询。  
  
 可以通过 LINQ 查询实现 <xref:System.Collections.Generic.IEnumerable%601> 泛型接口或 <xref:System.Linq.IQueryable%601> 泛型接口的数据源。 实现泛型接口的泛型类的实例用作 <xref:System.Data.Objects.ObjectQuery%601> <xref:System.Linq.IQueryable%601> LINQ to Entities 查询的数据源。 <xref:System.Data.Objects.ObjectQuery%601> 泛型类表示一个查询，该查询返回零个或多个类型化对象的集合。 还可以通过使用 Visual Basic) 中的 c # 关键字 (Dim 来使编译器推断实体的类型 `var` 。  
  
 在查询中，您可以确切指定要从数据源检索哪些信息。 查询也可以指定返回信息之前信息的排序、分组和表现方式。 在 LINQ 中，查询存储在变量中。 如果查询返回一系列值，则查询变量本身必须为可查询的类型。 此查询变量不执行任何操作，也不返回任何数据；它只存储查询信息。 创建查询后必须执行该查询以检索任何数据。  
  
## <a name="query-syntax"></a>查询语法  

 可以通过两种语法编写 LINQ to Entities 查询：查询表达式语法和基于方法的查询语法。 查询表达式语法是 C# 3.0 和 Visual Basic 9.0 中的新增功能，它由一组用类似于 Transact-SQL 或 XQuery 的声明性语法所编写的子句组成。 但 .NET Framework 公共语言运行时 (CLR) 不能读取查询表达式语法本身。 因此，在编译时，查询表达式将转换为 CLR 能理解的形式，即方法调用。 这些方法称为 " *标准查询运算符*"。 作为开发人员，您可以选择使用方法语法而不使用查询语法直接调用这些方法。 有关详细信息，请参阅 [LINQ 中的查询语法和方法语法](../../../../../csharp/programming-guide/concepts/linq/query-syntax-and-method-syntax-in-linq.md)。  
  
### <a name="query-expression-syntax"></a>查询表达式语法  

 查询表达式是一种声明性查询语法。 通过这一语法，开发人员可以使用类似于 Transact-SQL 的高级语言格式编写查询。 通过使用查询表达式语法，你可以用最少的代码对数据源执行复杂的筛选、排序和分组操作。 有关详细信息，请参阅 [Visual Basic) 的基本查询操作 (](../../../../../visual-basic/programming-guide/concepts/linq/basic-query-operations.md)。 有关演示如何使用查询表达式语法的示例，请参见以下主题：  
  
- [查询表达式语法示例：投影](query-expression-syntax-examples-projection.md)  
  
- [查询表达式语法示例：筛选](query-expression-syntax-examples-filtering.md)  
  
- [查询表达式语法示例：中间件排序](query-expression-syntax-examples-ordering.md)  
  
- [查询表达式语法示例：聚合运算符](query-expression-syntax-examples-aggregate-operators.md)  
  
- [查询表达式语法示例：分区](query-expression-syntax-examples-partitioning.md)  
  
- [查询表达式语法示例：联接运算符](query-expression-syntax-examples-join-operators.md)  
  
- [查询表达式语法示例：元素运算符](query-expression-syntax-examples-element-operators.md)  
  
- [查询表达式语法示例：分组](query-expression-syntax-examples-grouping.md)  
  
- [查询表达式语法示例：导航关系](query-expression-syntax-examples-navigating-relationships.md)  
  
### <a name="method-based-query-syntax"></a>基于方法的查询语法  

 编写 LINQ to Entities 查询的另一种方法是使用基于方法的查询。 基于方法的查询语法是一系列针对 LINQ 运算符方法的直接方法调用，同时将 lambda 表达式作为参数传递。 有关详细信息，请参阅 [Lambda 表达式](../../../../../csharp/language-reference/operators/lambda-expressions.md)。 有关演示如何使用基于方法的语法的示例，请参见以下主题：  
  
- [基于方法的查询语法示例：投影](method-based-query-syntax-examples-projection.md)  
  
- [基于方法的查询语法示例：筛选](method-based-query-syntax-examples-filtering.md)  
  
- [基于方法的查询语法示例：中间件排序](method-based-query-syntax-examples-ordering.md)  
  
- [基于方法的查询语法示例：聚合运算符](method-based-query-syntax-examples-aggregate-operators.md)  
  
- [基于方法的查询语法示例：分区](method-based-query-syntax-examples-partitioning.md)  
  
- [基于方法的查询语法示例：转换](method-based-query-syntax-examples-conversion.md)  
  
- [基于方法的查询语法示例：联接运算符](method-based-query-syntax-examples-join-operators.md)  
  
- [基于方法的查询语法示例：元素运算符](method-based-query-syntax-examples-element-operators.md)  
  
- [基于方法的查询语法示例：分组](method-based-query-syntax-examples-grouping.md)  
  
- [基于方法的查询语法示例：导航关系](method-based-query-syntax-examples-navigating-relationships.md)  
  
## <a name="see-also"></a>请参阅

- [LINQ to Entities](linq-to-entities.md)
- [C# 中的 LINQ 入门](../../../../../csharp/programming-guide/concepts/linq/index.md)
- [Visual Basic 中的 LINQ 入门](../../../../../visual-basic/programming-guide/concepts/linq/getting-started-with-linq.md)
- [EF 合并选项和已编译的查询](/archive/blogs/dsimmons/ef-merge-options-and-compiled-queries)
