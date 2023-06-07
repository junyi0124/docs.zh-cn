---
title: 可为空的值类型 - C# 参考
description: 了解 C# 中可以为 null 的值类型及其使用方法
ms.date: 11/04/2019
helpviewer_keywords:
- nullable value types [C#]
ms.openlocfilehash: 3ab2dff6b7399b0458a69d4498b2ebda24f6c5cc
ms.sourcegitcommit: 870bc4b4087510f6fba3c7b1c0d391f02bcc1f3e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/23/2020
ms.locfileid: "92471830"
---
# <a name="nullable-value-types-c-reference"></a>可为空的值类型（C# 参考）

可为 null 值类型  `T?` 表示其基础[值类型](value-types.md) `T` 的所有值及额外的 [null](../keywords/null.md) 值。 例如，可以将以下三个值中的任意一个指定给 `bool?` 变量：`true`、`false` 或 `null`。 基础值类型 `T` 本身不能是可为空的值类型。

> [!NOTE]
> C# 8.0 引入了可为空引用类型功能。 有关详细信息，请参阅[可为空引用类型](nullable-reference-types.md)。 从 C# 2 开始，提供可为空的值类型。

任何可为空的值类型都是泛型 <xref:System.Nullable%601?displayProperty=nameWithType> 结构的实例。 可使用以下任何一种可互换形式引用具有基础类型 `T` 的可为空值类型：`Nullable<T>` 或 `T?`。

需要表示基础值类型的未定义值时，通常使用可为空的值类型。 例如，布尔值或 `bool` 变量只能为 `true` 或 `false`。 但是，在某些应用程序中，变量值可能未定义或缺失。 例如，某个数据库字段可能包含 `true` 或 `false`，或者它可能不包含任何值，即 `NULL`。 在这种情况下，可以使用 `bool?` 类型。

## <a name="declaration-and-assignment"></a>声明和赋值

由于值类型可隐式转换为相应的可为空的值类型，因此可以像向其基础值类型赋值一样，向可为空值类型的变量赋值。 还可分配 `null` 值。 例如：

[!code-csharp[declare and assign](snippets/shared/NullableValueTypes.cs#Declaration)]

可为空值类型的默认值表示 `null`，也就是说，它是其 <xref:System.Nullable%601.HasValue%2A?displayProperty=nameWithType> 属性返回 `false` 的实例。

## <a name="examination-of-an-instance-of-a-nullable-value-type"></a>检查可为空值类型的实例

从 C# 7.0 开始，可以将 [`is` 运算符与类型模式 ](../operators/type-testing-and-cast.md#type-testing-with-pattern-matching) 结合使用，既检查 `null` 的可为空值类型的实例，又检索基础类型的值：

[!code-csharp-interactive[use pattern matching](snippets/shared/NullableValueTypes.cs#PatternMatching)]

始终可以使用以下只读属性来检查和获取可为空值类型变量的值：

- <xref:System.Nullable%601.HasValue%2A?displayProperty=nameWithType> 指示可为空值类型的实例是否有基础类型的值。

- 如果 <xref:System.Nullable%601.HasValue%2A> 为 `true`，则 <xref:System.Nullable%601.Value%2A?displayProperty=nameWithType> 获取基础类型的值。 如果 <xref:System.Nullable%601.HasValue%2A> 为 `false`，则 <xref:System.Nullable%601.Value%2A> 属性将引发 <xref:System.InvalidOperationException>。

以下示例中的使用 `HasValue` 属性在显示值之前测试变量是否包含该值：

[!code-csharp-interactive[use HasValue](snippets/shared/NullableValueTypes.cs#HasValue)]

还可将可为空的值类型的变量与 `null` 进行比较，而不是使用 `HasValue` 属性，如以下示例所示：

[!code-csharp-interactive[use comparison with null](snippets/shared/NullableValueTypes.cs#CompareWithNull)]

## <a name="conversion-from-a-nullable-value-type-to-an-underlying-type"></a>从可为空的值类型转换为基础类型

如果要将可为空值类型的值分配给不可以为 null 的值类型变量，则可能需要指定要分配的替代 `null` 的值。 使用 [Null 合并操作符`??`](../operators/null-coalescing-operator.md)执行此操作（也可将 <xref:System.Nullable%601.GetValueOrDefault(%600)?displayProperty=nameWithType> 方法用于相同的目的）：

[!code-csharp-interactive[?? operator](snippets/shared/NullableValueTypes.cs#NullCoalescing)]

如果要使用基础值类型的[默认值](default-values.md)来替代 `null`，请使用 <xref:System.Nullable%601.GetValueOrDefault?displayProperty=nameWithType> 方法。

还可以将可为空的值类型显式强制转换为不可为 null 的类型，如以下示例所示：

[!code-csharp[explicit cast](snippets/shared/NullableValueTypes.cs#Cast)]

在运行时，如果可为空的值类型的值为 `null`，则显式强制转换将抛出 <xref:System.InvalidOperationException>。

不可为 null 的值类型 `T` 隐式转换为相应的可为空值类型 `T?`。

## <a name="lifted-operators"></a>提升的运算符

预定义的一元运算符和二元[运算符](../operators/index.md)或值类型 `T` 支持的任何重载运算符也受相应的可为空值类型 `T?` 支持。 如果一个或全部两个操作数为 `null` ，则这些运算符（也称为提升的运算符）将生成 `null`；否则，运算符使用其操作数所包含的值来计算结果。 例如：

[!code-csharp[lifted operators](snippets/shared/NullableValueTypes.cs#LiftedOperator)]

> [!NOTE]
> 对于 `bool?` 类型，预定义的 `&` 和 `|` 运算符不遵循此部分中描述的规则：即使其中一个操作数为 `null`，运算符计算结果也可以不为 NULL。 有关详细信息，请参阅[布尔逻辑运算符](../operators/boolean-logical-operators.md)一文的[可以为 null 的布尔逻辑运算符](../operators/boolean-logical-operators.md#nullable-boolean-logical-operators)部分。

对于[比较运算符](../operators/comparison-operators.md) `<`、`>`、`<=` 和 `>=`，如果一个或全部两个操作数都为 `null`，则结果为 `false`；否则，将比较操作数的包含值。 请勿作出如下假定：由于某个特定的比较（例如 `<=`）返回 `false`，则相反的比较 (`>`) 返回 `true`。 以下示例显示 10

- 既不大于等于 `null`，
- 也不小于 `null`

[!code-csharp-interactive[relational and equality operators](snippets/shared/NullableValueTypes.cs#ComparisonOperators)]

对于[相等运算符](../operators/equality-operators.md#equality-operator-) `==`，如果两个操作数都为 `null`，则结果为 `true`；如果只有一个操作数为 `null`，则结果为 `false`；否则，将比较操作数的包含值。

对于[不等运算符](../operators/equality-operators.md#inequality-operator-) `!=`，如果两个操作数都为 `null`，则结果为 `false`；如果只有一个操作数为 `null`，则结果为 `true`；否则，将比较操作数的包含值。

如果在两个值类型之间存在[用户定义的转换](../operators/user-defined-conversion-operators.md)，则还可在相应的可为空值类型之间使用同一转换。

## <a name="boxing-and-unboxing"></a>装箱和取消装箱

可为空值类型的实例 `T?`[已装箱](../../programming-guide/types/boxing-and-unboxing.md)，如下所示：

- 如果 <xref:System.Nullable%601.HasValue%2A> 返回 `false`，则生成空引用。
- 如果 <xref:System.Nullable%601.HasValue%2A> 返回 `true`，则基础值类型 `T` 的对应值将装箱，而不对 <xref:System.Nullable%601> 的实例进行装箱。

可将值类型 `T` 的已装箱值取消装箱到相应的可为空值类型 `T?`，如以下示例所示：

[!code-csharp-interactive[boxing and unboxing](snippets/shared/NullableValueTypes.cs#Boxing)]

## <a name="how-to-identify-a-nullable-value-type"></a>如何确定可为空的值类型

下面的示例演示了如何确定 <xref:System.Type?displayProperty=nameWithType> 实例是否表示已构造的可为空值类型，即，具有指定类型参数 `T` 的 <xref:System.Nullable%601?displayProperty=nameWithType> 类型：

[!code-csharp-interactive[whether Type is nullable](snippets/shared/NullableValueTypes.cs#IsTypeNullable)]

如示例所示，使用 [typeof](../operators/type-testing-and-cast.md#typeof-operator) 运算符来创建 <xref:System.Type?displayProperty=nameWithType> 实例。

如果要确定实例是否是可为空的值类型，请不要使用 <xref:System.Object.GetType%2A?displayProperty=nameWithType> 方法获取要通过前面的代码测试的 <xref:System.Type> 实例。 如果对值类型可为空的实例调用 <xref:System.Object.GetType%2A?displayProperty=nameWithType> 方法，该实例将[装箱](#boxing-and-unboxing)到 <xref:System.Object>。 由于对可为空的值类型的非 NULL 实例的装箱等同于对基础类型的值的装箱，因此 <xref:System.Object.GetType%2A> 会返回表示可为空的值类型的基础类型的 <xref:System.Type> 实例：

[!code-csharp-interactive[GetType example](snippets/shared/NullableValueTypes.cs#GetType)]

另外，请勿使用 [is](../operators/type-testing-and-cast.md#is-operator) 运算符来确定实例是否是可为空的值类型。 如以下示例所示，无法使用 `is` 运算符区分可为空值类型实例的类型与其基础类型实例：

[!code-csharp-interactive[is operator example](snippets/shared/NullableValueTypes.cs#IsOperator)]

可使用以下示例中提供的代码来确定实例是否是可为空的值类型：

[!code-csharp-interactive[whether an instance is of a nullable type](snippets/shared/NullableValueTypes.cs#IsInstanceNullable)]

> [!NOTE]
> 此部分中所述的方法不适用于[可为空的引用类型](nullable-reference-types.md)的情况。

## <a name="c-language-specification"></a>C# 语言规范

有关更多信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的以下部分：

- [可以为 null 的类型](~/_csharplang/spec/types.md#nullable-types)
- [提升的运算符](~/_csharplang/spec/expressions.md#lifted-operators)
- [隐式可为空转换](~/_csharplang/spec/conversions.md#implicit-nullable-conversions)
- [显式可为空转换](~/_csharplang/spec/conversions.md#explicit-nullable-conversions)
- [提升的转换运算符](~/_csharplang/spec/conversions.md#lifted-conversion-operators)

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [“提升”的准确含义是什么？](/archive/blogs/ericlippert/what-exactly-does-lifted-mean)
- <xref:System.Nullable%601?displayProperty=nameWithType>
- <xref:System.Nullable?displayProperty=nameWithType>
- <xref:System.Nullable.GetUnderlyingType%2A?displayProperty=nameWithType>
- [可为空引用类型](nullable-reference-types.md)
