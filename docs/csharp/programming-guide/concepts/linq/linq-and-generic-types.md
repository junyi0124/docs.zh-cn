---
title: LINQ 和泛型类型 (C#)
description: 了解 C# 中支持查询的泛型类型的基本概念。  LINQ 查询基于泛型类型。
ms.date: 07/20/2015
helpviewer_keywords:
- LINQ [C#], generic types
- generic types [LINQ]
- generics [LINQ]
ms.assetid: 660e3799-25ca-462c-8c4a-8bce04fbb031
ms.openlocfilehash: a71957eabea788fb06195df1ef026390947f013d
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91170462"
---
# <a name="linq-and-generic-types-c"></a>LINQ 和泛型类型 (C#)

LINQ 查询基于 .NET Framework 版本 2.0 中引入的泛型类型。 无需深入了解泛型即可开始编写查询。 但是，可能需要了解 2 个基本概念：  
  
1. 创建泛型集合类（如 <xref:System.Collections.Generic.List%601>）的实例时，需将“T”替换为列表将包含的对象类型。 例如，字符串列表表示为 `List<string>`，`Customer` 对象列表表示为 `List<Customer>`。 泛型列表属于强类型，与将其元素存储为 <xref:System.Object> 的集合相比，泛型列表具备更多优势。 如果尝试将 `Customer` 添加到 `List<string>`，则会在编译时收到错误。 泛型集合易于使用的原因是不必执行运行时类型转换。  
  
2. <xref:System.Collections.Generic.IEnumerable%601> 是一个接口，通过该接口，可以使用 `foreach` 语句来枚举泛型集合类。 泛型集合类支持 <xref:System.Collections.Generic.IEnumerable%601>，正如非泛型集合类（如 <xref:System.Collections.ArrayList>）支持 <xref:System.Collections.IEnumerable>。  
  
 有关泛型的详细信息，请参阅[泛型](../../generics/index.md)。  
  
## <a name="ienumerablet-variables-in-linq-queries"></a>LINQ 查询中的 IEnumerable<T\> 变量  

 LINQ 查询变量被类型化为 <xref:System.Collections.Generic.IEnumerable%601> 或派生类型（如 <xref:System.Linq.IQueryable%601>）。 看到类型化为 `IEnumerable<Customer>` 的查询变量时，这只意味着执行查询时，该查询将生成包含零个或多个 `Customer` 对象的序列。  
  
 [!code-csharp[csLINQGettingStarted#34](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#34)]  
  
 有关详细信息，请参阅 [LINQ 查询操作中的类型关系](./type-relationships-in-linq-query-operations.md)。  
  
## <a name="letting-the-compiler-handle-generic-type-declarations"></a>让编译器处理泛型类型声明  

 如果愿意，可以使用 [var](../../../language-reference/keywords/var.md) 关键字来避免使用泛型语法。 `var` 关键字指示编译器通过查看在 `from` 子句中指定的数据源来推断查询变量的类型。 以下示例生成与上例相同的编译代码：  
  
 [!code-csharp[csLINQGettingStarted#35](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#35)]  
  
 变量的类型明显或显式指定嵌套泛型类型（如由组查询生成的那些类型）并不重要时，`var` 关键字很有用。 通常，我们建议如果使用 `var`，应意识到这可能使他人更难以理解代码。 有关详细信息，请参阅[隐式类型局部变量](../../classes-and-structs/implicitly-typed-local-variables.md)。  
  
## <a name="see-also"></a>请参阅

- [泛型](../../generics/index.md)
