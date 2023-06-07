---
title: 元组类型 - C# 参考
description: 了解 C# 元组：轻型数据结构，可用于对相关的数据元素进行松散分组
ms.date: 07/09/2020
helpviewer_keywords:
- value tuples [C#]
ms.openlocfilehash: d996c7afecba1b58bfd8337fa444fd71790dd482
ms.sourcegitcommit: 870bc4b4087510f6fba3c7b1c0d391f02bcc1f3e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92471767"
---
# <a name="tuple-types-c-reference"></a>元组类型（C# 参考）

元组功能在 C# 7.0 及更高版本中可用，它提供了简洁的语法，用于将多个数据元素分组成一个轻型数据结构。 下面的示例演示了如何声明元组变量、对它进行初始化并访问其数据成员：

[!code-csharp-interactive[tuple intro](snippets/shared/ValueTuples.cs#Introduction)]

如前面的示例所示，若要定义元组类型，需要指定其所有数据成员的类型，或者，可以指定[字段名称](#tuple-field-names)。 虽然不能在元组类型中定义方法，但可以使用 .NET 提供的方法，如下面的示例所示：

[!code-csharp-interactive[tuple methods](snippets/shared/ValueTuples.cs#MethodOnTuples)]

从 C# 7.3 开始，元组类型支持[相等运算符](../operators/equality-operators.md) `==` 和 `!=`。 有关详细信息，请参阅[元组相等](#tuple-equality)部分。

元组类型是[值类型](value-types.md)；元组元素是公共字段。 这使得元组为可变的值类型。

> [!NOTE]
> 元组功能需要 <xref:System.ValueTuple?displayProperty=nameWithType> 类型和相关的泛型类型（例如 <xref:System.ValueTuple%602?displayProperty=nameWithType>），这些类型在 .NET Core 和 .NET Framework 4.7 及更高版本中可用。 若要在面向 .NET Framework 4.6.2 或更早版本的项目中使用元组，请将 NuGet 包 [`System.ValueTuple`](https://www.nuget.org/packages/System.ValueTuple/) 添加到项目。

可以使用任意数量的元素定义元组：

[!code-csharp-interactive[large tuple](snippets/shared/ValueTuples.cs#LargeTuple)]

## <a name="use-cases-of-tuples"></a>元组的用例

元组最常见的用例之一是作为方法返回类型。 也就是说，你可以将方法结果分组为元组返回类型，而不是定义 [`out` 方法参数](../keywords/out-parameter-modifier.md)，如以下示例所示：

[!code-csharp-interactive[multiple method outputs](snippets/shared/ValueTuples.cs#MultipleReturns)]

如前面的示例所示，可以直接使用返回的元组实例，或者可以在单独的变量中[析构](#tuple-assignment-and-deconstruction)它。

还可以使用元组类型，而不是[匿名类型](../../programming-guide/classes-and-structs/anonymous-types.md)；例如，在 LINQ 查询中。 有关详细信息，请参阅[在匿名类型和元组类型之间选择](../../../standard/base-types/choosing-between-anonymous-and-tuple.md)。

通常，你会使用元组对相关的数据元素进行松散分组。 这通常在专用和内部实用工具方法中有用。 对于公共 API 用例，请考虑定义[类](../keywords/class.md)或[结构](struct.md)类型。

## <a name="tuple-field-names"></a>元组字段名称

可以在元组初始化表达式中或元组类型的定义中显式指定元组字段的名称，如下面的示例所示：

[!code-csharp-interactive[explicit field names](snippets/shared/ValueTuples.cs#ExplicitFieldNames)]

从 C# 7.1 开始，如果未指定字段名称，则可以根据元组初始化表达式中相应变量的名称推断出此名称，如下面的示例所示：

[!code-csharp-interactive[inferred field names](snippets/shared/ValueTuples.cs#InferFieldNames)]

这称为元组投影初始值设定项。 在以下情况下，变量名称不会被投影到元组字段名称中：

- 候选名称是元组类型的成员名称，例如 `Item3`、`ToString` 或 `Rest`。
- 候选名称是另一元组的显式或隐式字段名称的重复项。

在这些情况下，你可以显式指定字段的名称，或按字段的默认名称访问字段。

元组字段的默认名称为 `Item1`、`Item2`、`Item3` 等。 始终可以使用字段的默认名称，即使字段名称是显式指定的或推断出的，如下面的示例所示：

[!code-csharp-interactive[default field names](snippets/shared/ValueTuples.cs#DefaultFieldNames)]

[元组赋值](#tuple-assignment-and-deconstruction)和[元组相等比较](#tuple-equality)不会考虑字段名称。

在编译时，编译器会将非默认字段名称替换为相应的默认名称。 因此，显式指定或推断的字段名称在运行时不可用。

## <a name="tuple-assignment-and-deconstruction"></a>元组赋值和析构

C# 支持满足以下两个条件的元组类型之间的赋值：

- 两个元组类型有相同数量的元素
- 对于每个元组位置，右侧元组元素的类型与左侧相应的元组元素的类型相同或可以隐式转换为左侧相应的元组元素的类型

元组元素是按照元组元素的顺序赋值的。 元组字段的名称会被忽略且不会被赋值，如下面的示例所示：

[!code-csharp-interactive[tuple assignment](snippets/shared/ValueTuples.cs#Assignment)]

还可以使用赋值运算符 `=` 在单独的变量中析构元组实例。 为此，可以使用以下方式之一进行操作：

- 在括号内显式声明每个变量的类型：

  [!code-csharp-interactive[specify types of variables](snippets/shared/ValueTuples.cs#DeconstructExplicit)]

- 在括号外使用 `var` 关键字来声明隐式类型化变量，并让编译器推断其类型：

  [!code-csharp-interactive[implicitly typed variables](snippets/shared/ValueTuples.cs#DeconstructVar)]

- 使用现有变量：

  [!code-csharp-interactive[existing variables](snippets/shared/ValueTuples.cs#DeconstructExisting)]

有关析构元组和其他类型的详细信息，请参阅[析构元组和其他类型](../../deconstruct.md)。

## <a name="tuple-equality"></a>元组相等

从 C# 7.3 开始，元组类型支持 `==` 和 `!=` 运算符。 这些运算符按照元组元素的顺序将左侧操作数的成员与相应的右侧操作数的成员进行比较。

[!code-csharp-interactive[tuple equality](snippets/shared/ValueTuples.cs#TupleEquality)]

如前面的示例所示，`==` 和 `!=` 操作不会考虑元组字段名称。

同时满足以下两个条件时，两个元组可比较：

- 两个元组具有相同数量的元素。 例如，如果 `t1` 和 `t2` 具有不同数目的元素，`t1 != t2` 则不会进行编译。
- 对于每个元组位置，可以使用 `==` 和 `!=` 运算符对左右侧元组操作数中的相应元素进行比较。 例如，`(1, (2, 3)) == ((1, 2), 3)` 不会进行编译，因为 `1` 不可与 `(1, 2)` 比较。

`==` 和 `!=` 运算符将以短路方式对元组进行比较。 也就是说，一旦遇见一对不相等的元素或达到元组的末尾，操作将立即停止。 但是，在进行任何比较之前，将对所有元组元素进行计算，如以下示例所示：

[!code-csharp-interactive[tuple element evaluation](snippets/shared/ValueTuples.cs#TupleEvaluationForEquality)]

## <a name="tuples-as-out-parameters"></a>元组作为 out 参数

通常，你会将具有 [`out` 参数](../keywords/out-parameter-modifier.md)的方法重构为返回元组的方法。 但是，在某些情况下，`out` 参数可以是元组类型。 下面的示例演示了如何将元组作为 `out` 参数使用：

[!code-csharp-interactive[tuple as out parameter](snippets/shared/ValueTuples.cs#TupleAsOutParameter)]

## <a name="tuples-vs-systemtuple"></a>元组与 `System.Tuple`

<xref:System.ValueTuple?displayProperty=nameWithType> 类型支持的 C# 元组不同于 <xref:System.Tuple?displayProperty=nameWithType> 类型表示的元组。 主要区别如下：

- `ValueTuple` 类型是[值类型](value-types.md)。 `Tuple` 类型是[引用类型](../keywords/reference-types.md)。
- `ValueTuple` 类型是可变的。 `Tuple` 类型是不可变的。
- `ValueTuple` 类型的数据成员是字段。 `Tuple` 类型的数据成员是属性。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅以下功能建议说明：

- [推断元组名称（也称为元组投影初始值设定项）](~/_csharplang/proposals/csharp-7.1/infer-tuple-names.md)
- [支持对元组类型使用 `==` 和 `!=`](~/_csharplang/proposals/csharp-7.3/tuple-equality.md)

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [值类型](value-types.md)
- [在匿名类型和元组类型之间选择](../../../standard/base-types/choosing-between-anonymous-and-tuple.md)
- <xref:System.ValueTuple?displayProperty=nameWithType>
