---
title: LINQ to Entities
description: '了解如何创建和运行 LINQ to Entities 查询，这些查询允许使用 Visual Basic 或 Visual c # 针对实体框架概念模型编写查询。'
ms.date: 03/30/2017
ms.assetid: 641f9b68-9046-47a1-abb0-1c8eaeda0e2d
ms.openlocfilehash: 809cbd10be6a2bc44bb4bff0ba3ea363da676f7f
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91150786"
---
# <a name="linq-to-entities"></a>LINQ to Entities

LINQ to Entities 提供语言集成查询 (LINQ) 支持，它允许开发人员使用 Visual Basic 或 Visual C# 根据实体框架概念模型编写查询。 针对实体框架的查询由针对对象上下文执行的命令目录树查询表示。 LINQ to Entities 将语言集成查询 (LINQ) 查询转换为命令目录树查询，针对实体框架执行这些查询，并返回可同时由实体框架和 LINQ 使用的对象。 以下是创建和执行 LINQ to Entities 查询的过程：  
  
1. 从 <xref:System.Data.Objects.ObjectQuery%601> 构造 <xref:System.Data.Objects.ObjectContext> 实例。  
  
2. 通过使用 <xref:System.Data.Objects.ObjectQuery%601> 实例在 C# 或 Visual Basic 中编写 LINQ to Entities 查询。  
  
3. 将 LINQ 标准查询运算符和表达式将转换为命令目录树。  
  
4. 对数据源执行命令目录树表示形式的查询。 执行过程中在数据源上引发的任何异常都将直接向上传递到客户端。  
  
5. 将查询结果返回到客户端。  
  
## <a name="constructing-an-objectquery-instance"></a>构造 ObjectQuery 实例  

 <xref:System.Data.Objects.ObjectQuery%601> 泛型类表示一个查询，此查询返回由零个或零个以上类型化实体组成的集合。 对象查询通常从现有对象上下文中构造（而不是手动构造），并且始终属于该对象上下文。 此上下文提供了编写和执行查询所需的连接和元数据信息。 <xref:System.Data.Objects.ObjectQuery%601> 泛型类实现 <xref:System.Linq.IQueryable%601> 泛型接口，借助于该接口的生成器方法，能够以增量方式生成 LINQ 查询。 也可以使用 C# `var` 关键字（在 Visual Basic 中为 `Dim`，启用局部类型推理）让编译器推断实体的类型。  
  
## <a name="composing-the-queries"></a>编写查询  

 <xref:System.Data.Objects.ObjectQuery%601> 泛型类（可实现泛型 <xref:System.Linq.IQueryable%601> 接口）的实例可充当 LINQ to Entities 查询的数据源。 在查询中，您可以确切指定要从数据源检索哪些信息。 查询也可以指定返回信息之前信息的排序、分组和表现方式。 在 LINQ 中，查询存储在变量中。 此查询变量不执行任何操作，也不返回任何数据；它只存储查询信息。 创建查询后必须执行该查询以检索任何数据。  
  
 可以通过两种语法编写 LINQ to Entities 查询：查询表达式语法和基于方法的查询语法。 查询表达式语法和基于方法的查询语法是 C# 3.0 和 Visual Basic 9.0 中的新增功能。  
  
 有关详细信息，请参阅 [LINQ to Entities 中的查询](queries-in-linq-to-entities.md)。  
  
## <a name="query-conversion"></a>查询转换  

 若要针对实体框架执行 LINQ to Entities 查询，必须将 LINQ 查询转换为可针对实体框架执行的命令目录树表示形式。  
  
 LINQ to Entities 查询由 LINQ 标准查询运算符组成 (例如，、 <xref:System.Linq.Queryable.Select%2A> <xref:System.Linq.Queryable.Where%2A> 和 <xref:System.Linq.Queryable.GroupBy%2A>) 和表达式 (x > 10、Contact 等) 。 LINQ 运算符并非由类定义，而是由类中的方法定义。 在 LINQ 中，表达式可包含 <xref:System.Linq.Expressions> 命名空间内的类型所允许的任何内容，并且，通过扩展，可包含可在 lambda 函数中表示的任何内容。 这是实体框架允许的表达式的超集，这些表达式在定义上限于数据库所允许的操作，并且受到 <xref:System.Data.Objects.ObjectQuery%601> 支持。  
  
 在实体框架中，运算符和表达式同时由单一类型层次结构表示，这些运算符和表达式随后会放入命令目录树中。 实体框架使用命令目录树来执行此查询。 如果 LINQ 查询无法表示为命令目录树，则当转换查询时，将引发异常。 LINQ to Entities 查询的转换涉及两个子转换：标准查询运算符的转换和表达式转换。  
  
 一些 LINQ 标准查询运算符在 LINQ to Entities 中没有有效的转换。 尝试使用这些运算符将在查询转换期间导致异常。 有关支持的 LINQ to Entities 运算符的列表，请参阅 [LINQ to Entities) 中支持和不支持的 LINQ 方法 (](supported-and-unsupported-linq-methods-linq-to-entities.md)。  
  
 有关在 LINQ to Entities 中使用标准查询运算符的详细信息，请参阅 [LINQ to Entities 查询中的标准查询运算符](standard-query-operators-in-linq-to-entities-queries.md)。  
  
 通常，LINQ to Entities 中的表达式将在服务器上求值，这样，表达式的行为不会遵循 CLR 语义。 有关详细信息，请参阅 [LINQ to Entities 查询中的表达式](expressions-in-linq-to-entities-queries.md)。  
  
 有关 CLR 方法调用如何映射到数据源中的规范函数的信息，请参阅 [Clr 方法到规范函数的映射](clr-method-to-canonical-function-mapping.md)。  
  
 有关如何从 LINQ to Entities 查询中调用规范、数据库和自定义函数的信息，请参阅 [LINQ to Entities 查询中调用函数](calling-functions-in-linq-to-entities-queries.md)。  
  
## <a name="query-execution"></a>查询执行  

 在用户创建 LINQ 查询之后，该查询将转换为与实体框架兼容的表示形式（采用命令树形式），然后针对数据源执行此查询。 在查询执行时间，将在客户端或服务器上计算所有查询表达式（或查询的组成部分）的值。 这包括在结果具体化或实体投影中使用的表达式。 有关详细信息，请参阅 [查询执行](query-execution.md)。 若要了解如何通过编译查询一次，然后使用不同的参数多次执行它来提高性能，请参阅 [)  (LINQ to Entities 的编译查询 ](compiled-queries-linq-to-entities.md)。  
  
## <a name="materialization"></a>具体化  

 具体化是将查询结果作为 CLR 类型返回到客户端的过程。 在 LINQ to Entities 中，从不返回查询结果数据记录；始终存在一个后备 CLR 类型，该类型由用户或实体框架定义，或者由编译器生成（匿名类型）。 所有对象具体化均由实体框架执行。 由于无法在实体框架与 CLR 之间进行映射而导致的任何错误都将导致在对象具体化过程中引发异常。  
  
 查询结果通常以下面的某一种形式返回：  
  
- 概念模型中定义的零个或多个类型化实体对象的集合或复杂类型的投影。  
  
- 实体框架支持的 CLR 类型。  
  
- 内联集合。  
  
- 匿名类型。  
  
 有关详细信息，请参阅 [查询结果](query-results.md)。  
  
## <a name="in-this-section"></a>本节内容  

 [LINQ to Entities 中的查询](queries-in-linq-to-entities.md)  
  
 [LINQ to Entities 查询中的表达式](expressions-in-linq-to-entities-queries.md)  
  
 [在 LINQ to Entities 查询中调用函数](calling-functions-in-linq-to-entities-queries.md)  
  
 [已编译查询 (LINQ to Entities) ](compiled-queries-linq-to-entities.md)  
  
 [查询执行](query-execution.md)  
  
 [查询结果](query-results.md)  
  
 [LINQ to Entities 查询中的标准查询运算符](standard-query-operators-in-linq-to-entities-queries.md)  
  
 [CLR 方法至规范函数映射](clr-method-to-canonical-function-mapping.md)  
  
 [支持和不支持的 LINQ 方法 (LINQ to Entities)](supported-and-unsupported-linq-methods-linq-to-entities.md)  
  
 [LINQ to Entities 中的已知问题和注意事项](known-issues-and-considerations-in-linq-to-entities.md)  
  
## <a name="see-also"></a>请参阅

- [LINQ to Entities 中的已知问题和注意事项](known-issues-and-considerations-in-linq-to-entities.md)
- [语言集成查询 (LINQ) - C#](../../../../../csharp/programming-guide/concepts/linq/index.md)
- [语言集成查询 (LINQ) - Visual Basic](../../../../../visual-basic/programming-guide/concepts/linq/index.md)
- [LINQ 和 ADO.NET](../../linq-and-ado-net.md)
- [ADO.NET 实体框架](../index.md)
