---
title: 运算符重载 - C# 引用
description: 了解如何重载 C# 运算符以及哪些运算符可重载。
ms.date: 07/05/2019
f1_keywords:
- operator_CSharpKeyword
- operator
helpviewer_keywords:
- operator keyword [C#]
- operator overloading [C#]
ms.openlocfilehash: 7f8014750d0255bd93c8fff533d90c4322f8207a
ms.sourcegitcommit: 30a686fd4377fe6472aa04e215c0de711bc1c322
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/10/2020
ms.locfileid: "94439672"
---
# <a name="operator-overloading-c-reference"></a>运算符重载（C# 引用）

用户定义的类型可重载预定义的 C# 运算符。 也就是说，当一个或两个操作数都是某类型时，此类型可提供操作的自定义实现。 [可重载运算符](#overloadable-operators)部分介绍了哪些 C# 运算符可重载。

使用 `operator` 关键字来声明运算符。 运算符声明必须符合以下规则：

- 同时包含 `public` 和 `static` 修饰符。
- 一元运算符有一个输入参数。 二元运算符有两个输入参数。 在每种情况下，都至少有一个参数必须具有类型 `T` 或 `T?`，其中 `T` 是包含运算符声明的类型。

下面的示例定义了一个表示有理数的简单结构。 该结构会重载一些[算术运算符](arithmetic-operators.md)：

[!code-csharp[fraction example](snippets/shared/OperatorOverloading.cs)]

可以通过[定义从 `int` 到 `Fraction` 的隐式转换](user-defined-conversion-operators.md)来扩展前面的示例。 然后，重载运算符将支持这两种类型的参数。 也就是说，可以将一个整数添加到一个分数中，得到一个分数结果。

还可以使用 `operator` 关键字来定义自定义类型转换。 有关详细信息，请参阅[用户定义转换运算符](user-defined-conversion-operators.md)。

## <a name="overloadable-operators"></a>可重载运算符

下表提供了 C# 运算符可重载性的相关信息：

| 运算符 | 可重载性 |
| --------- | --------------- |
|[+x](arithmetic-operators.md#unary-plus-and-minus-operators)、[-x](arithmetic-operators.md#unary-plus-and-minus-operators)、[!x](boolean-logical-operators.md#logical-negation-operator-)、[~x](bitwise-and-shift-operators.md#bitwise-complement-operator-)、[++](arithmetic-operators.md#increment-operator-)、[--](arithmetic-operators.md#decrement-operator---)、[true](true-false-operators.md)、[false](true-false-operators.md)|这些一元运算符可以进行重载。|
|[x + y](addition-operator.md)、[x - y](subtraction-operator.md)、[x \* y](arithmetic-operators.md#multiplication-operator-)、[x / y](arithmetic-operators.md#division-operator-)、[x % y](arithmetic-operators.md#remainder-operator-)、[x & y](boolean-logical-operators.md#logical-and-operator-)、[x &#124; y](boolean-logical-operators.md#logical-or-operator-)、[x ^ y](boolean-logical-operators.md#logical-exclusive-or-operator-)、[x \<\< y](bitwise-and-shift-operators.md#left-shift-operator-), [x >> y](bitwise-and-shift-operators.md#right-shift-operator-)、[x == y](equality-operators.md#equality-operator-)、[x != y](equality-operators.md#inequality-operator-)、[x \< y](comparison-operators.md#less-than-operator-), [x > y](comparison-operators.md#greater-than-operator-)、[x \<= y](comparison-operators.md#less-than-or-equal-operator-), [x >= y](comparison-operators.md#greater-than-or-equal-operator-)|这些二元运算符可以进行重载。 某些运算符必须成对重载；有关详细信息，请查看此表后面的备注。|
|[x && y](boolean-logical-operators.md#conditional-logical-and-operator-)、[x &#124;&#124; y](boolean-logical-operators.md#conditional-logical-or-operator-)|无法重载条件逻辑运算符。 但是，如果具有已重载的 [`true` 和 `false` 运算符](true-false-operators.md)的类型还以某种方式重载了 `&` 或 <code>&#124;</code> 运算符，则可针对此类型的操作数分别计算 `&&` 或 <code>&#124;&#124;</code> 运算符。 有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的[用户定义条件逻辑运算符](~/_csharplang/spec/expressions.md#user-defined-conditional-logical-operators)部分。|
|[a&#91;i&#93;](member-access-operators.md#indexer-operator-), [`a?[i]`](member-access-operators.md#null-conditional-operators--and-)|元素访问不被视为可重载运算符，但你可定义[索引器](../../programming-guide/indexers/index.md)。|
|[(T)x](type-testing-and-cast.md#cast-expression)|强制转换运算符不能重载，但可以定义可由强制转换表达式执行的自定义类型转换。 有关详细信息，请参阅[用户定义转换运算符](user-defined-conversion-operators.md)。|
|[+=](arithmetic-operators.md#compound-assignment)、[-=](arithmetic-operators.md#compound-assignment)、[\*=](arithmetic-operators.md#compound-assignment)、[/=](arithmetic-operators.md#compound-assignment)、[%=](arithmetic-operators.md#compound-assignment)、[&=](boolean-logical-operators.md#compound-assignment)、[&#124;=](boolean-logical-operators.md#compound-assignment)、[^=](boolean-logical-operators.md#compound-assignment)、[\<\<=](bitwise-and-shift-operators.md#compound-assignment), [>>=](bitwise-and-shift-operators.md#compound-assignment)|复合赋值运算符不能显式重载。 但在重载二元运算符时，也会隐式重载相应的复合赋值运算符（若有）。 例如，使用 `+`（可以进行重载）计算 `+=`。|
|[^x](member-access-operators.md#index-from-end-operator-)、[x = y](assignment-operator.md)、[x.y](member-access-operators.md#member-access-expression-)、[`x?.y`](member-access-operators.md#null-conditional-operators--and-)、[c ? t : f](conditional-operator.md)、[x ?? y](null-coalescing-operator.md)、[x ??= y](null-coalescing-operator.md)、[x..y](member-access-operators.md#range-operator-)、[x->y](pointer-related-operators.md#pointer-member-access-operator--)、[=>](lambda-operator.md)、[f(x)](member-access-operators.md#invocation-expression-)、[as](type-testing-and-cast.md#as-operator)、[await](await.md)、[checked](../keywords/checked.md)、[unchecked](../keywords/unchecked.md)、[default](default.md)、[delegate](delegate-operator.md)、[is](type-testing-and-cast.md#is-operator)、[nameof](nameof.md)、[new](new-operator.md)、[sizeof](sizeof.md)、[stackalloc](stackalloc.md)、[switch](switch-expression.md)、[typeof](type-testing-and-cast.md#typeof-operator)、[with](with-expression.md)|这些运算符无法进行重载。|

> [!NOTE]
> 比较运算符必须成对重载。 也就是说，如果重载一对运算符中的任一一个，则另一个运算符也必须重载。 此类配对如下所示：
>
> - `==` 和 `!=` 运算符
> - `<` 和 `>` 运算符
> - `<=` 和 `>=` 运算符

## <a name="c-language-specification"></a>C# 语言规范

有关更多信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的以下部分：

- [运算符重载](~/_csharplang/spec/expressions.md#operator-overloading)
- [运算符](~/_csharplang/spec/classes.md#operators)

## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- [用户定义转换运算符](user-defined-conversion-operators.md)
- [设计准则 - 运算符重载](../../../standard/design-guidelines/operator-overloads.md)
- [设计准则 - 相等运算符](../../../standard/design-guidelines/equality-operators.md)
- [重载运算符为何在 C# 中始终是静态的？](/archive/blogs/ericlippert/why-are-overloaded-operators-always-static-in-c)
