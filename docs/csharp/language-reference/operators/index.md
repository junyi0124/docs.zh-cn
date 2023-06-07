---
title: C# 运算符和表达式 - C# 参考
description: 了解 C# 运算符和表达式、运算符优先级和运算符结合性
ms.date: 08/04/2020
f1_keywords:
- cs.operators
helpviewer_keywords:
- operators [C#]
- operator precedence [C#]
- operator associativity [C#]
- expressions [C#]
ms.assetid: 0301e31f-22ad-49af-ac3c-d5eae7f0ac43
ms.openlocfilehash: 9e7ca2087938317f7369043e21fd455dbad7f07b
ms.sourcegitcommit: 30a686fd4377fe6472aa04e215c0de711bc1c322
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94439652"
---
# <a name="c-operators-and-expressions-c-reference"></a>C# 运算符和表达式（C# 参考）

C# 提供了许多运算符。 其中许多都受到[内置类型](../builtin-types/built-in-types.md)的支持，可用于对这些类型的值执行基本操作。 这些运算符包括以下组：

- [算术运算符](arithmetic-operators.md)，将对数值操作数执行算术运算
- [比较运算符](comparison-operators.md)，将比较数值操作数
- [布尔逻辑运算符](boolean-logical-operators.md)，将对 [`bool`](../builtin-types/bool.md) 操作数执行逻辑运算
- [位运算符和移位运算符](bitwise-and-shift-operators.md)，将对整数类型的操作数执行位运算或移位运算
- [相等运算符](equality-operators.md)，将检查其操作数是否相等

通常可以[重载](operator-overloading.md)这些运算符，也就是说，可以为用户定义类型的操作数指定运算符行为。

最简单的 C# 表达式是文本（例如[整数](../builtin-types/integral-numeric-types.md#integer-literals)和[实数](../builtin-types/floating-point-numeric-types.md#real-literals)）和变量名称。 可以使用运算符将它们组合成复杂的表达式。 运算符[优先级](#operator-precedence)和[结合性](#operator-associativity)决定了表达式中操作的执行顺序。 可以使用括号更改由运算符优先级和结合性决定的计算顺序。

在下面的代码中，表达式的示例位于赋值的右侧：

[!code-csharp[expression examples](snippets/shared/Overview.cs#Expressions)]

通常情况下，表达式会生成结果，并可包含在其他表达式中。 [`void`](../builtin-types/void.md) 方法调用是不生成结果的表达式的示例。 它只能用作[语句](../../programming-guide/statements-expressions-operators/statements.md)，如下面的示例所示：

```csharp
Console.WriteLine("Hello, world!");
```

下面是 C# 提供的一些其他类型的表达式：

- [内插字符串表达式](../tokens/interpolated.md)，提供创建格式化字符串的便利语法：

  [!code-csharp-interactive[interpolated string](snippets/shared/Overview.cs#InterpolatedString)]

- [Lambda 表达式](lambda-expressions.md)，可用于创建匿名函数：

  [!code-csharp-interactive[lambda expression](snippets/shared/Overview.cs#Lambda)]

- [查询表达式](../keywords/query-keywords.md)，可用于直接以 C# 使用查询功能：

  [!code-csharp-interactive[query expression](snippets/shared/Overview.cs#Query)]

可使用[表达式主体定义](../../programming-guide/statements-expressions-operators/expression-bodied-members.md)为方法、构造函数、属性、索引器或终结器提供简洁的定义。

## <a name="operator-precedence"></a>运算符优先级

在包含多个运算符的表达式中，先按优先级较高的运算符计算，再按优先级较低的运算符计算。 在下面的示例中，首先执行乘法，因为其优先级高于加法：

```csharp-interactive
var a = 2 + 2 * 2;
Console.WriteLine(a); //  output: 6
```

使用括号更改运算符优先级所施加的计算顺序：

```csharp-interactive
var a = (2 + 2) * 2;
Console.WriteLine(a); //  output: 8
```

下表按最高优先级到最低优先级的顺序列出 C# 运算符。 每行中运算符的优先级相同。

| 运算符 | 类别或名称 |
| --------- | ---------------- |
| [x.y](member-access-operators.md#member-access-expression-)、[f(x)](member-access-operators.md#invocation-expression-)、[a[i]](member-access-operators.md#indexer-operator-)、[`x?.y`](member-access-operators.md#null-conditional-operators--and-)、[`x?[y]`](member-access-operators.md#null-conditional-operators--and-)、[x++](arithmetic-operators.md#increment-operator-)、[x--](arithmetic-operators.md#decrement-operator---)、[x!](null-forgiving.md)、[new](new-operator.md)、[typeof](type-testing-and-cast.md#typeof-operator)、[checked](../keywords/checked.md)、[unchecked](../keywords/unchecked.md)、[default](default.md)、[nameof](nameof.md)、[delegate](delegate-operator.md)、[sizeof](sizeof.md)、[stackalloc](stackalloc.md)、[x->y](pointer-related-operators.md#pointer-member-access-operator--) | 基本 |
| [+x](arithmetic-operators.md#unary-plus-and-minus-operators)、[-x](arithmetic-operators.md#unary-plus-and-minus-operators)、[\!x](boolean-logical-operators.md#logical-negation-operator-)、[~x](bitwise-and-shift-operators.md#bitwise-complement-operator-)、[++x](arithmetic-operators.md#increment-operator-)、[--x](arithmetic-operators.md#decrement-operator---)、[^x](member-access-operators.md#index-from-end-operator-)、[(T)x](type-testing-and-cast.md#cast-expression)、[await](await.md)、[&x](pointer-related-operators.md#address-of-operator-)、[*x](pointer-related-operators.md#pointer-indirection-operator-)、[true 和 false](true-false-operators.md) | 一元 |
| [x..y](member-access-operators.md#range-operator-) | 范围 |
| [switch](switch-expression.md) | `switch` 表达式 |
| [with](with-expression.md) | `with` 表达式 |
| [x * y](arithmetic-operators.md#multiplication-operator-)、[x / y](arithmetic-operators.md#division-operator-)、[x % y](arithmetic-operators.md#remainder-operator-) | 乘法|
| [x + y](arithmetic-operators.md#addition-operator-)、[x – y](arithmetic-operators.md#subtraction-operator--) | 加法 |
| [x \<\<  y](bitwise-and-shift-operators.md#left-shift-operator-), [x >> y](bitwise-and-shift-operators.md#right-shift-operator-) | 移位 |
| [x \< y](comparison-operators.md#less-than-operator-), [x > y](comparison-operators.md#greater-than-operator-)、[x \<= y](comparison-operators.md#less-than-or-equal-operator-), [x >= y](comparison-operators.md#greater-than-or-equal-operator-)、[is](type-testing-and-cast.md#is-operator)、[as](type-testing-and-cast.md#as-operator) | 关系和类型测试 |
| [x == y](equality-operators.md#equality-operator-)、[x != y](equality-operators.md#inequality-operator-) | 相等 |
| `x & y` | [布尔逻辑 AND](boolean-logical-operators.md#logical-and-operator-) 或[按位逻辑 AND](bitwise-and-shift-operators.md#logical-and-operator-) |
| `x ^ y` | [布尔逻辑 XOR](boolean-logical-operators.md#logical-exclusive-or-operator-) 或[按位逻辑 XOR](bitwise-and-shift-operators.md#logical-exclusive-or-operator-) |
| <code>x &#124; y</code> | [布尔逻辑 OR](boolean-logical-operators.md#logical-or-operator-) 或[按位逻辑 OR](bitwise-and-shift-operators.md#logical-or-operator-) |
| [x && y](boolean-logical-operators.md#conditional-logical-and-operator-) | 条件“与” |
| [x &#124;&#124; y](boolean-logical-operators.md#conditional-logical-or-operator-) | 条件“或” |
| [x ?? y](null-coalescing-operator.md) | Null 合并运算符 |
| [c ? t : f](conditional-operator.md) | 条件运算符 |
| [x = y](assignment-operator.md)、[x += y](arithmetic-operators.md#compound-assignment)、[x -= y](arithmetic-operators.md#compound-assignment)、[x *= y](arithmetic-operators.md#compound-assignment)、[x /= y](arithmetic-operators.md#compound-assignment)、[x %= y](arithmetic-operators.md#compound-assignment)、[x &= y](boolean-logical-operators.md#compound-assignment)、[x &#124;= y](boolean-logical-operators.md#compound-assignment)、[x ^= y](boolean-logical-operators.md#compound-assignment)、[x <<= y](bitwise-and-shift-operators.md#compound-assignment)、[x >>= y](bitwise-and-shift-operators.md#compound-assignment)、[x ??= y](null-coalescing-operator.md)、[=>](lambda-operator.md) | 赋值和 lambda 声明 |

## <a name="operator-associativity"></a>运算符结合性

当运算符的优先级相同，运算符的结合性决定了运算的执行顺序：

- 左结合运算符按从左到右的顺序计算。 除[赋值运算符](assignment-operator.md)和 [null 合并运算符](null-coalescing-operator.md)外，所有二元运算符都是左结合运算符。 例如，`a + b - c` 将计算为 `(a + b) - c`。
- 右结合运算符按从右到左的顺序计算。 赋值运算符、null 合并运算符和[条件运算符`?:`](conditional-operator.md)是右结合运算符。 例如，`x = y = z` 将计算为 `x = (y = z)`。

使用括号更改运算符结合性所施加的计算顺序：

```csharp-interactive
int a = 13 / 5 / 2;
int b = 13 / (5 / 2);
Console.WriteLine($"a = {a}, b = {b}");  // output: a = 1, b = 6
```

## <a name="operand-evaluation"></a>操作数计算

与运算符的优先级和结合性无关，从左到右计算表达式中的操作数。 以下示例展示了运算符和操作数的计算顺序：

| 表达式 | 计算顺序 |
| ---------- | ------------------- |
|`a + b`|a, b, +|
|`a + b * c`|a, b, c, *, +|
|`a / b + c * d`|a, b, /, c, d, *, +|
|`a / (b + c) * d`|a, b, c, +, /, d, *|

通常，会计算所有运算符操作数。 但是，某些运算符有条件地计算操作数。 也就是说，此类运算符的最左侧操作数的值定义了是否应计算其他操作数，或计算其他哪些操作数。 这些运算符有条件逻辑 [AND (`&&`)](boolean-logical-operators.md#conditional-logical-and-operator-) 和 [OR (`||`)](boolean-logical-operators.md#conditional-logical-or-operator-) 运算符、[null 合并运算符 `??` 和 `??=`](null-coalescing-operator.md)、[null 条件运算符 `?.` 和 `?[]`](member-access-operators.md#null-conditional-operators--and-) 以及[条件运算符`?:`](conditional-operator.md)。 有关详细信息，请参阅每个运算符的说明。

## <a name="c-language-specification"></a>C# 语言规范

有关更多信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的以下部分：

- [表达式](~/_csharplang/spec/expressions.md)
- [运算符](~/_csharplang/spec/expressions.md#operators)

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [运算符重载](operator-overloading.md)
- [表达式树](../../programming-guide/concepts/expression-trees/index.md)
