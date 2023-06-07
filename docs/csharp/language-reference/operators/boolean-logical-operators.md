---
title: 布尔逻辑运算符 - C# 参考
description: 了解使用布尔操作数执行逻辑非、逻辑与、逻辑或和逻辑异或运算的 C# 运算符。
ms.date: 06/29/2020
author: pkulikov
f1_keywords:
- '!_CSharpKeyword'
- '&_CSharpKeyword'
- ^_CSharpKeyword
- '|_CSharpKeyword'
- '&&_CSharpKeyword'
- '||_CSharpKeyword'
- '|=_CSharpKeyword'
- ^=_CSharpKeyword
- '&=_CSharpKeyword'
helpviewer_keywords:
- logical operators [C#]
- short-circuiting operators [C#]
- logical negation operator [C#]
- NOT operator [C#]
- '! operator [C#]'
- AND operator [C#]
- ampersand operator [C#]
- conjunction operator [C#]
- '& operator [C#]'
- exclusive OR operator [C#]
- XOR operator [C#]
- ^ operator [C#]
- OR operator [C#]
- disjunction operator [C#]
- '| operator [C#]'
- conditional AND operator [C#]
- short-circuiting AND operator [C#]
- '&& operator [C#]'
- conditional OR operator [C#]
- short-circuiting OR operator [C#]
- '|| operator [C#]'
ms.openlocfilehash: 00b1523029ed6562fda6947415029cd3b7a9b405
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916895"
---
# <a name="boolean-logical-operators-c-reference"></a>布尔逻辑运算符（C# 参考）

以下运算符使用 [bool](../builtin-types/bool.md) 操作数执行逻辑运算：

- 一元 [`!`（逻辑非）](#logical-negation-operator-)运算符。
- 二元 [`&`（逻辑与）](#logical-and-operator-)、[`|`（逻辑或）](#logical-or-operator-)和 [`^`（逻辑异或）](#logical-exclusive-or-operator-)运算符。 这些运算符始终计算两个操作数。
- 二元 [`&&`（条件逻辑与）](#conditional-logical-and-operator-)和 [`||`（条件逻辑或）](#conditional-logical-or-operator-)运算符。 这些运算符仅在必要时才计算右侧操作数。

对于[整型数值类型](../builtin-types/integral-numeric-types.md)的操作数，`&`、`|` 和 `^` 运算符执行位逻辑运算。 有关详细信息，请参阅[位运算符和移位运算符](bitwise-and-shift-operators.md)。

## <a name="logical-negation-operator-"></a>逻辑非运算符 !

一元前缀 `!` 运算符计算操作数的逻辑非。 也就是说，如果操作数的计算结果为 `false`，它生成 `true`；如果操作数的计算结果为 `true`，它生成 `false`：

[!code-csharp-interactive[logical negation](snippets/shared/BooleanLogicalOperators.cs#Negation)]

从 C# 8.0 起，一元后缀 `!` 运算符为 [null 包容运算符](null-forgiving.md)。

## <a name="logical-and-operator-amp"></a><a name="logical-and-operator-"></a> 逻辑 AND 运算符 &amp;

`&` 运算符计算操作数的逻辑与。 如果 `x` 和 `y` 的计算结果都为 `true`，则 `x & y` 的结果为 `true`。 否则，结果为 `false`。

即使左侧操作数计算结果为 `false`，`&` 运算符也会计算这两个操作数，而在这种情况下，无论右侧操作数的值为何，运算结果都为 `false`。

在下面的示例中，`&` 运算符的右侧操作数是方法调用，无论左侧操作数的值如何，都会执行方法调用：

[!code-csharp-interactive[logical AND](snippets/shared/BooleanLogicalOperators.cs#And)]

[条件逻辑 AND 运算符](#conditional-logical-and-operator-) `&&` 也计算操作数的逻辑 AND，但如果左侧操作数的计算结果为 `false`，它就不会计算右侧操作数。

对于[整型数值类型](../builtin-types/integral-numeric-types.md)的操作数，`&` 运算符计算其操作数的[位逻辑 AND](bitwise-and-shift-operators.md#logical-and-operator-)。 一元 `&` 运算符是 [address-of 运算符](pointer-related-operators.md#address-of-operator-)。

## <a name="logical-exclusive-or-operator-"></a>逻辑异或运算符 ^

`^` 运算符计算操作数的逻辑异或（亦称为“逻辑 XOR”）。 如果 `x` 计算结果为 `true` 且 `y` 计算结果为 `false`，或者 `x` 计算结果为 `false` 且 `y` 计算结果为 `true`，那么 `x ^ y` 的结果为 `true`。 否则，结果为 `false`。 也就是说，对于 `bool` 操作数，`^` 运算符的计算结果与[不等运算符](equality-operators.md#inequality-operator-) `!=` 相同。

[!code-csharp-interactive[logical exclusive OR](snippets/shared/BooleanLogicalOperators.cs#Xor)]

对于[整型数值类型](../builtin-types/integral-numeric-types.md)的操作数，`^` 运算符计算其操作数的[位逻辑异或](bitwise-and-shift-operators.md#logical-exclusive-or-operator-)。

## <a name="logical-or-operator-"></a>逻辑或运算符 |

`|` 运算符计算操作数的逻辑或。 如果 `x` 或 `y` 的计算结果为 `true`，则 `x | y` 的结果为 `true`。 否则，结果为 `false`。

即使左侧操作数计算结果为 `true`，`|` 运算符也会计算这两个操作数，而在这种情况下，无论右侧操作数的值为何，运算结果都为 `true`。

在下面的示例中，`|` 运算符的右侧操作数是方法调用，无论左侧操作数的值如何，都会执行方法调用：

[!code-csharp-interactive[logical OR](snippets/shared/BooleanLogicalOperators.cs#Or)]

[条件逻辑 OR 运算符](#conditional-logical-or-operator-) `||` 也计算操作数的逻辑 OR，但如果左侧操作数的计算结果为 `true`，它就不会计算右侧操作数。

对于[整型数值类型](../builtin-types/integral-numeric-types.md)的操作数，`|` 运算符计算其操作数的[位逻辑 OR](bitwise-and-shift-operators.md#logical-or-operator-)。

## <a name="conditional-logical-and-operator-ampamp"></a><a name="conditional-logical-and-operator-"></a> 条件逻辑 AND 运算符 &amp;&amp;

条件逻辑与运算符 `&&`（亦称为“短路”逻辑与运算符）计算操作数的逻辑与。 如果 `x` 和 `y` 的计算结果都为 `true`，则 `x && y` 的结果为 `true`。 否则，结果为 `false`。 如果 `x` 的计算结果为 `false`，则不计算 `y`。

在下面的示例中，`&&` 运算符的右侧操作数是方法调用，如果左侧操作数的计算结果为 `false`，就不执行方法调用：

[!code-csharp-interactive[conditional logical AND](snippets/shared/BooleanLogicalOperators.cs#ConditionalAnd)]

[逻辑 AND 运算符](#logical-and-operator-) `&` 也计算操作数的逻辑 AND，但始终计算两个操作数。

## <a name="conditional-logical-or-operator-"></a>条件逻辑或运算符 ||

条件逻辑或运算符 `||`（亦称为“短路”逻辑或运算符）计算操作数的逻辑或。 如果 `x` 或 `y` 的计算结果为 `true`，则 `x || y` 的结果为 `true`。 否则，结果为 `false`。 如果 `x` 的计算结果为 `true`，则不计算 `y`。

在下面的示例中，`||` 运算符的右侧操作数是方法调用，如果左侧操作数的计算结果为 `true`，就不执行方法调用：

[!code-csharp-interactive[conditional logical OR](snippets/shared/BooleanLogicalOperators.cs#ConditionalOr)]

[逻辑 OR 运算符](#logical-or-operator-) `|` 也计算操作数的逻辑 OR，但始终计算两个操作数。

## <a name="nullable-boolean-logical-operators"></a>可以为 null 的布尔逻辑运算符

对于 `bool?` 操作数，[`&`（逻辑与）](#logical-and-operator-)和 [`|`（逻辑或）](#logical-or-operator-)运算符支持三值逻辑，如下所示：

- 仅当其两个操作数的计算结果都为 `true` 时，`&` 运算符才生成 `true`。 如果 `x` 或 `y` 的计算结果为 `false`，则 `x & y` 将生成 `false`（即使另一个操作数的计算结果为 `null`）。 否则，`x & y` 的结果为 `null`。

- 仅当其两个操作数的计算结果都为 `false` 时，`|` 运算符才生成 `false`。 如果 `x` 或 `y` 的计算结果为 `true`，则 `x | y` 将生成 `true`（即使另一个操作数的计算结果为 `null`）。 否则，`x | y` 的结果为 `null`。

下表显示了该语义：

|x|y|x 和 y|x&#124;y|  
|----|----|----|----|  
|true|true|true|true|  
|true|false|false|true|  
|true|null|null|true|  
|false|true|false|true|  
|false|false|false|false|  
|false|null|false|null|  
|null|true|null|true|  
|null|false|false|null|  
|null|null|null|null|  

这些运算符的行为不同于值类型可以为 null 的典型运算符行为。 通常情况下，为值类型的操作数定义的运算符也能与值类型可以为 null 的相应操作数一起使用。 如果其任一操作数的计算结果为 `null`，此类运算符都会生成 `null`。 不过，即使操作数之一的计算结果为 `null`，`&` 和 `|` 运算符也可以生成非 null。 有关值类型可为空的运算符行为的详细信息，请参阅[可为空的值类型](../builtin-types/nullable-value-types.md)一文的[提升的运算符](../builtin-types/nullable-value-types.md#lifted-operators)部分。

此外，你还可以将 `!` 和 `^` 运算符与 `bool?` 操作数结合使用，如下例所示：

[!code-csharp-interactive[lifted negation and xor](snippets/shared/BooleanLogicalOperators.cs#WithNullableBoolean)]

条件逻辑运算符 `&&` 和 `||` 不支持 `bool?` 操作数。

## <a name="compound-assignment"></a>复合赋值

对于二元运算符 `op`，窗体的复合赋值表达式

```csharp
x op= y
```

等效于

```csharp
x = x op y
```

不同的是 `x` 只计算一次。

`&`、`|` 和 `^` 运算符支持复合赋值，如下面的示例所示：

[!code-csharp-interactive[compound assignment](snippets/shared/BooleanLogicalOperators.cs#CompoundAssignment)]

> [!NOTE]
> 条件逻辑运算符 `&&` 和 `||` 不支持复合赋值。

## <a name="operator-precedence"></a>运算符优先级

以下列表按优先级从高到低的顺序对逻辑运算符进行排序：

- 逻辑非运算符 `!`
- 逻辑与运算符 `&`
- 逻辑异或运算符 `^`
- 逻辑或运算符 `|`
- 条件逻辑与运算符 `&&`
- 条件逻辑或运算符 `||`

使用括号 `()` 可以更改运算符优先级决定的计算顺序：

[!code-csharp-interactive[operator precedence](snippets/shared/BooleanLogicalOperators.cs#Precedence)]

要了解按优先级排序的完整 C# 运算符列表，请参阅 [C# 运算符](index.md#operator-precedence)一文中的[运算符优先级](index.md)部分。

## <a name="operator-overloadability"></a>运算符可重载性

用户定义类型可以[重载](operator-overloading.md)`!`、`&`、`|` 和 `^` 运算符。 重载二元运算符时，对应的复合赋值运算符也会隐式重载。 用户定义类型不能显式重载复合赋值运算符。

用户定义类型无法重载条件逻辑运算符 `&&` 和 `||`。 不过，如果用户定义类型以某种方式重载 [true 和 false 运算符](true-false-operators.md)以及 `&` 或 `|` 运算符，可以对相应类型的操作数执行 `&&` 或 `||` 运算。 有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的[用户定义条件逻辑运算符](~/_csharplang/spec/expressions.md#user-defined-conditional-logical-operators)部分。

## <a name="c-language-specification"></a>C# 语言规范

有关更多信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的以下部分：

- [逻辑非运算符](~/_csharplang/spec/expressions.md#logical-negation-operator)
- [逻辑运算符](~/_csharplang/spec/expressions.md#logical-operators)
- [条件逻辑运算符](~/_csharplang/spec/expressions.md#conditional-logical-operators)
- [复合赋值](~/_csharplang/spec/expressions.md#compound-assignment)

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- [位运算符和移位运算符](bitwise-and-shift-operators.md)
