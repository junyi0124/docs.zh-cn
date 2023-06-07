---
description: '- \- 和 -= 运算符 - C# 参考'
title: '- \- 和 -= 运算符 - C# 参考'
ms.date: 05/27/2019
f1_keywords:
- -_CSharpKeyword
- -=_CSharpKeyword
helpviewer_keywords:
- subtraction operator [C#]
- delegate removal [C#]
- '- operator [C#]'
- subtraction assignment operator [C#]
- event unsubscription [C#]
- -= operator [C#]
ms.assetid: 4de7a4fa-c69d-48e6-aff1-3130af970b2d
ms.openlocfilehash: 871067d8049c66f2b8d863987b668e5287b36911
ms.sourcegitcommit: d579fb5e4b46745fd0f1f8874c94c6469ce58604
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/30/2020
ms.locfileid: "89124690"
---
# <a name="--and---operators-c-reference"></a>- 和 -= 运算符（C# 参考）

内置[整型](../builtin-types/integral-numeric-types.md)和[浮点](../builtin-types/floating-point-numeric-types.md)数字类型以及[委托](../builtin-types/reference-types.md#the-delegate-type)类型支持 `-` 和 `-=` 运算符。

有关算术 `-` 运算符的信息，请参阅[一元加和减运算符](arithmetic-operators.md#unary-plus-and-minus-operators)和[算术运算符](arithmetic-operators.md)文章的[减法运算符 -](arithmetic-operators.md#subtraction-operator--) 部分。

## <a name="delegate-removal"></a>委托删除

对于[委托](../builtin-types/reference-types.md#the-delegate-type)类型相同的操作数，`-` 运算符返回如下计算的委托实例：

- 如果两个操作数都为非空，并且右侧操作数的调用列表是左侧操作数调用列表的正确连续子列表，则该操作的结果是通过从左侧操作数的调用列表中删除右侧操作数的条目而获得的新调用列表。 如果右侧操作数的列表与左侧操作数列表中的多个连续子列表匹配，则仅删除最右侧的匹配子列表。 如果删除行为导致出现空列表，则结果为 `null`。

  [!code-csharp-interactive[delegate removal](snippets/shared/SubtractionOperator.cs#DelegateRemoval)]

- 如果右侧操作数的调用列表不是左侧操作数调用列表的正确连续子列表，则该操作的结果是左侧操作数。 例如，删除不属于多播委托的委托不会执行任何操作，从而导致不变的多播委托。

  [!code-csharp-interactive[delegate removal with no effect](snippets/shared/SubtractionOperator.cs#DelegateRemovalNoChange)]

  前面的示例还演示了在删除委托期间对委托实例进行比较。 例如，通过计算相同的 [Lambda 表达式](lambda-expressions.md)生成的委托不相等。 有关委托相等性的详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的[委托相等运算符](~/_csharplang/spec/expressions.md#delegate-equality-operators)部分。

- 如果左侧操作数为 `null`，则操作结果为 `null`。 如果右侧操作数为 `null`，则操作的结果是左侧操作数。

  [!code-csharp-interactive[delegate removal and null](snippets/shared/SubtractionOperator.cs#DelegateRemovalAndNull)]

若要合并委托，请使用 [`+` 运算符](addition-operator.md#delegate-combination)。

有关委托类型的详细信息，请参阅[委托](../../programming-guide/delegates/index.md)。

## <a name="subtraction-assignment-operator--"></a>减法赋值运算符 -=

使用 `-=` 运算符的表达式，例如

```csharp
x -= y
```

等效于

```csharp
x = x - y
```

不同的是 `x` 只计算一次。

下面的示例演示 `-=` 运算符的用法：

[!code-csharp-interactive[-= examples](snippets/shared/SubtractionOperator.cs#SubtractAndAssign)]

还可以使用 `-=` 运算符指定在取消订阅[事件](../keywords/event.md)时要删除的事件处理程序方法。 有关详细信息，请参阅[如何订阅和取消订阅事件](../../programming-guide/events/how-to-subscribe-to-and-unsubscribe-from-events.md)。

## <a name="operator-overloadability"></a>运算符可重载性

用户定义的类型可以[重载](operator-overloading.md)`-` 运算符。 重载二元 `-` 运算符后，也会隐式重载 `-=` 运算符。 用户定义类型不能显式重载 `-=` 运算符。

## <a name="c-language-specification"></a>C# 语言规范

有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)的[一元减运算符](~/_csharplang/spec/expressions.md#unary-minus-operator)和[减法运算符](~/_csharplang/spec/expressions.md#subtraction-operator)部分。

## <a name="see-also"></a>另请参阅

- [C# 参考](../index.md)
- [C# 运算符和表达式](index.md)
- [事件](../../programming-guide/events/index.md)
- [算术运算符](arithmetic-operators.md)
- [+ 和 += 运算符](addition-operator.md)
