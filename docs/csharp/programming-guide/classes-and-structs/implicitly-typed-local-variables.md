---
title: 隐式类型本地变量 - C# 编程指南
description: C# 中的 var 关键字指示编译器通过初始化语句右侧的表达式推断变量的类型。
ms.date: 07/20/2015
helpviewer_keywords:
- implicitly-typed local variables [C#]
- var [C#]
ms.assetid: b9218fb2-ef5d-4814-8a8e-2bc29b0bbc9b
ms.openlocfilehash: 6badb8588dedda80227ab38bee027cf2890c8672
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864210"
---
# <a name="implicitly-typed-local-variables-c-programming-guide"></a>隐式类型本地变量（C# 编程指南）

可声明局部变量而无需提供显式类型。 `var` 关键字指示编译器通过初始化语句右侧的表达式推断变量的类型。 推断类型可以是内置类型、匿名类型、用户定义类型或 .NET 类库中定义的类型。 有关如何使用 `var` 初始化数组的详细信息，请参阅[隐式类型化数组](../arrays/implicitly-typed-arrays.md)。

以下示例演示使用 `var` 声明局部变量的各种方式：

[!code-csharp[csProgGuideLINQ#43](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csRef30LangFeatures_2.cs#43)]

重要的是了解 `var` 关键字并不意味着“变体”，并且并不指示变量是松散类型或是后期绑定。 它只表示由编译器确定并分配最适合的类型。

在以下上下文中，可使用 `var` 关键字：

- 在局部变量（在方法范围内声明的变量）上，如前面的示例所示。

- 在 [for](../../language-reference/keywords/for.md) 初始化语句中。

    ```csharp
    for (var x = 1; x < 10; x++)
    ```

- 在 [foreach](../../language-reference/keywords/foreach-in.md) 初始化语句中。

    ```csharp
    foreach (var item in list) {...}
    ```

- 在 [using](../../language-reference/keywords/using-statement.md) 域间中。

    ```csharp
    using (var file = new StreamReader("C:\\myfile.txt")) {...}
    ```

有关详细信息，请参阅[如何在查询表达式中使用隐式类型化局部变量和数组](how-to-use-implicitly-typed-local-variables-and-arrays-in-a-query-expression.md)。

## <a name="var-and-anonymous-types"></a>var 和匿名类型

在许多情况下，使用 `var` 是可选的，只是一种语法便利。 但是，在使用匿名类型初始化变量时，如果需要在以后访问对象的属性，则必须将变量声明为 `var`。 这是 LINQ 查询表达式中的常见方案。 有关详细信息，请参阅[匿名类型](anonymous-types.md)。

从源代码角度来看，匿名类型没有名称。 因此，如果使用 `var` 初始化了查询变量，则访问返回对象序列中的属性的唯一方法是在 `foreach` 语句中将 `var` 用作迭代变量的类型。

[!code-csharp[csProgGuideLINQ#44](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csRef30LangFeatures_2.cs#44)]

## <a name="remarks"></a>备注

以下限制适用于隐式类型化变量声明：

- 仅当局部变量在相同语句中进行声明和初始化时，才能使用 `var`；变量不能初始化为 null，也不能初始化为方法组或匿名函数。

- `var` 不能在类范围内对字段使用。

- 使用 `var` 声明的变量不能在初始化表达式中使用。 换句话说，此表达式是合法的：`int i = (i = 20);`，但是此表达式会生成编译时错误：`var i = (i = 20);`

- 不能在相同语句中初始化多个隐式类型化变量。

- 如果一种名为 `var` 的类型处于范围内，则 `var` 关键字会解析为该类型名称，不会被视为隐式类型化局部变量声明的一部分。

带 `var` 关键字的隐式类型只能应用于本地方法范围内的变量。 隐式类型不可用于类字段，因为 C# 编译器在处理代码时会遇到逻辑悖论：编译器需要知道字段的类型，但它在分析赋值表达式前无法确定类型，而表达式在不知道类型的情况下无法进行计算。 考虑下列代码：

```csharp
private var bookTitles;
```

`bookTitles` 是类型为 `var` 的类字段。 由于该字段没有要计算的表达式，编译器无法推断出 `bookTitles` 应该是哪种类型。 此外，向该字段添加表达式（就像对本地变量执行的操作一样）也是不够的：

```csharp
private var bookTitles = new List<string>();
```

当编译器在代码编译期间遇到字段时，它会在处理与其关联的任何表达式之前记录每个字段的类型。 编译器在尝试分析 `bookTitles` 时遇到相同的悖论：它需要知道字段的类型，但编译器通常会通过分析表达式来确定 `var` 的类型，这在事先不知道类型的情况下无法实现。

你可能会发现，对于在其中难以确定查询变量的确切构造类型的查询表达式，`var` 也可能会十分有用。 这可能会针对分组和排序操作发生。

当变量的特定类型在键盘上键入时很繁琐、或是显而易见、或是不会提高代码的可读性时，`var` 关键字也可能非常有用。 `var` 采用此方法提供帮助的一个示例是针对嵌套泛型类型（如用于分组操作的类型）。 在下面的查询中，查询变量的类型是 `IEnumerable<IGrouping<string, Student>>`。 只要你和必须维护你的代码的其他人了解这一点，使用隐式类型化实现便利性和简便性时便不会出现问题。

[!code-csharp[cscsrefQueryKeywords#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Group.cs#13)]

使用 `var` 有助于简化代码，但是它的使用应该限制在需要使用它的情况下，或在它可使代码更易于读取的情况下。 有关何时正确使用 `var` 的详细信息，请参阅 C# 编码指南一文中的[隐式类型本地变量](../inside-a-program/coding-conventions.md#implicitly-typed-local-variables)节。

## <a name="see-also"></a>请参阅

- [C# 参考](../../language-reference/index.md)
- [隐式类型化数组](../arrays/implicitly-typed-arrays.md)
- [如何在查询表达式中使用隐式类型化局部变量和数组](how-to-use-implicitly-typed-local-variables-and-arrays-in-a-query-expression.md)
- [匿名类型](anonymous-types.md)
- [对象和集合初始值设定项](object-and-collection-initializers.md)
- [var](../../language-reference/keywords/var.md)
- [C# 中的 LINQ](../../linq/index.md)
- [LINQ（语言集成查询）](../../linq/index.md)
- [for](../../language-reference/keywords/for.md)
- [foreach, in](../../language-reference/keywords/foreach-in.md)
- [using 语句](../../language-reference/keywords/using-statement.md)
