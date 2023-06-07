---
title: LINQ 查询操作中的类型关系 (C#)
description: 了解 LINQ 查询中的变量类型如何相互关联。 LINQ 查询操作在数据源、查询及执行中是强类型的。
ms.date: 07/20/2015
helpviewer_keywords:
- inferring type information [LINQ in C#]
- data sources [LINQ in C#], type relationships
- queries [LINQ in C#], type relationships
- relationships [LINQ in C#]
- type relationships [LINQ in C#]
- variable relationships [LINQ in C#]
- type information inferred [LINQ in C#]
- data transformations [LINQ in C#]
- LINQ [C#], type relationships
ms.assetid: 99118938-d47c-4d7e-bb22-2657a9f95268
ms.openlocfilehash: 78cdb550e59bc82386d34f0e2bf6b1cae11d72de
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203977"
---
# <a name="type-relationships-in-linq-query-operations-c"></a>LINQ 查询操作中的类型关系 (C#)

若要有效编写查询，应了解完整的查询操作中的变量类型是如何全部彼此关联的。 如果了解这些关系，就能够更容易地理解文档中的 LINQ 示例和代码示例。 另外，还能了解在使用 `var` 隐式对变量进行类型化时的后台操作。  
  
 LINQ 查询操作在数据源、查询本身及查询执行中是强类型的。 查询中变量的类型必须与数据源中元素的类型和 `foreach` 语句中迭代变量的类型兼容。 此强类型保证在编译时捕获类型错误，以便可以在用户遇到这些错误之前更正它们。  
  
 为了演示这些类型关系，下面的大多数示例对所有变量使用显式类型。 最后一个示例演示在利用使用 [var](../../../language-reference/keywords/var.md) 的隐式类型时，如何应用相同的原则。  
  
## <a name="queries-that-do-not-transform-the-source-data"></a>不转换源数据的查询  

 下图演示不对数据执行转换的 LINQ to Objects 查询操作。 源包含一个字符串序列，查询输出也是一个字符串序列。  
  
 ![关系图显示 LINQ 查询中数据类型的关系。](./media/type-relationships-in-linq-query-operations/linq-query-data-type-relation.png)  
  
1. 数据源的类型参数决定范围变量的类型。  
  
2. 所选对象的类型决定查询变量的类型。 此处的 `name` 是一个字符串。 因此，查询变量是一个 `IEnumerable<string>`。  
  
3. 在 `foreach` 语句中循环访问查询变量。 因为查询变量是一个字符串序列，所以迭代变量也是一个字符串。  
  
## <a name="queries-that-transform-the-source-data"></a>转换源数据的查询  

 下图演示对数据执行简单转换的 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] 查询操作。 查询将一个 `Customer` 对象序列用作输入，并只选择结果中的 `Name` 属性。 因为 `Name` 是一个字符串，所以查询生成一个字符串序列作为输出。  
  
 ![关系图显示转换数据类型的查询。](./media/type-relationships-in-linq-query-operations/linq-query-transform-data-type.png)  
  
1. 数据源的类型参数决定范围变量的类型。  
  
2. `select` 语句返回 `Name` 属性，而非完整的 `Customer` 对象。 因为 `Name` 是一个字符串，所以 `custNameQuery` 的类型参数是 `string`，而非 `Customer`。  
  
3. 因为 `custNameQuery` 是一个字符串序列，所以 `foreach` 循环的迭代变量也必须是 `string`。  
  
 下图演示稍微复杂的转换。 `select` 语句返回只捕获原始 `Customer` 对象的两个成员的匿名类型。  
  
 ![关系图显示转换数据类型的更复杂的查询。](./media/type-relationships-in-linq-query-operations/linq-complex-query-transform-data-type.png)  
  
1. 数据源的类型参数始终为查询中范围变量的类型。  
  
2. 因为 `select` 语句生成匿名类型，所以必须使用 `var` 隐式类型化查询变量。  
  
3. 因为查询变量的类型是隐式的，所以 `foreach` 循环中的迭代变量也必须是隐式的。  
  
## <a name="letting-the-compiler-infer-type-information"></a>让编译器推断类型信息  

 虽然需要了解查询操作中的类型关系，但是也可以选择让编译器执行全部工作。 关键字 [var](../../../language-reference/keywords/var.md) 可用于查询操作中的任何本地变量。 下图与前面讨论的第二个示例相似。 但是，编译器为查询操作中的各个变量提供强类型。  
  
 ![关系图显示具有隐式类型的类型流。](./media/type-relationships-in-linq-query-operations/linq-type-flow-implicit-typing.png)  
  
 有关 `var` 的详细信息，请参阅[隐式类型本地变量](../../classes-and-structs/implicitly-typed-local-variables.md)。  
