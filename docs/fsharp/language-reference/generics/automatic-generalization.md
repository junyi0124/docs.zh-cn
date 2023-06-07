---
title: 自动泛化
description: 了解如何F#自动通用化函数的自变量和类型, 以便它们可以使用多个类型。
ms.date: 05/16/2016
ms.openlocfilehash: 501749a190d9770cbcd9848e3d528cba32fe6762
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68630623"
---
# <a name="automatic-generalization"></a>自动泛化

F#使用类型推理来计算函数和表达式的类型。 本主题介绍如何F#自动通用化函数的自变量和类型, 以便在可能的情况下使用多个类型。

## <a name="automatic-generalization"></a>自动泛化

F#编译器在对函数执行类型推理时, 确定给定的参数是否可以为泛型。 编译器检查每个参数, 并确定该函数是否依赖于该参数的特定类型。 如果不是, 则将类型推断为泛型。

下面的代码示例阐释了编译器推断为泛型的函数。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-3/snippet101.fs)]

将类型推断`'a -> 'a -> 'a`为。

类型指示这是一个函数, 该函数采用同一未知类型的两个参数, 并返回该类型的值。 上一个函数可以是泛型的原因之一是, 大于运算符 (`>`) 本身是泛型的。 大于运算符具有签名`'a -> 'a -> bool`。 并非所有运算符都是泛型运算符, 并且如果函数中的代码将参数类型与非泛型函数或运算符一起使用, 则不能通用化该参数类型。

由于`max`是泛型的, 因此它可用于类型`int`(如、 `float`等), 如下面的示例中所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-3/snippet102.fs)]

但是, 这两个参数必须属于同一类型。 签名为, `'a -> 'a -> 'a`而不`'a -> 'b -> 'a`是。 因此, 以下代码将生成错误, 因为类型不匹配。

```fsharp
// Error: type mismatch.
let biggestIntFloat = max 2.0 3
```

`max`函数还适用于支持大于运算符的任何类型。 因此, 您还可以对字符串使用它, 如下面的代码所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-3/snippet104.fs)]

## <a name="value-restriction"></a>值限制

编译器仅对具有显式自变量的完整函数定义和简单的不可变值执行自动泛化。

这意味着, 如果您尝试编译的代码未充分约束为特定类型, 但也不能归纳, 则编译器会发出错误。 此问题的错误消息将对值的自动泛化进行此限制, 以作为*值限制*。

通常情况下, 如果您希望构造是泛型的, 但编译器没有足够的信息来通用化它, 或在非泛型构造中无意中省略了足够的类型信息, 则会发生值限制错误。 值限制错误的解决方案是提供更多显式信息以通过以下方式之一来更好地约束类型推理问题:

- 通过将显式类型批注添加到值或参数, 将类型限制为非泛型。

- 如果问题是使用 nongeneralizable 构造来定义泛型函数, 例如函数组合或未完全应用的扩充函数参数, 请尝试将函数重写为普通函数定义。

- 如果问题是过于复杂而无法通用化的表达式, 则通过添加额外的未使用的参数将其置于函数中。

- 添加显式泛型类型参数。 此选项很少使用。

- 下面的代码示例演示了每种情况。

案例 1:表达式太复杂。 在此示例中, 此`counter`列表应`int option ref`为, 但不会将其定义为简单的不可变值。

```fsharp
let counter = ref None
// Adding a type annotation fixes the problem:
let counter : int option ref = ref None
```

案例 2:使用 nongeneralizable 构造定义泛型函数。 在此示例中, 构造是 nongeneralizable 的, 因为它涉及函数参数的部分应用。

```fsharp
let maxhash = max << hash
// The following is acceptable because the argument for maxhash is explicit:
let maxhash obj = (max << hash) obj
```

案例 3:添加多余的未使用参数。 由于此表达式对于通用化而言不够简单, 因此编译器会发出值限制错误。

```fsharp
let emptyList10 = Array.create 10 []
// Adding an extra (unused) parameter makes it a function, which is generalizable.
let emptyList10 () = Array.create 10 []
```

情况 4:添加类型参数。

```fsharp
let arrayOf10Lists = Array.create 10 []
// Adding a type parameter and type annotation lets you write a generic value.
let arrayOf10Lists<'T> = Array.create 10 ([]:'T list)
```

在最后一种情况下, 值变成类型函数, 可用于创建许多不同类型的值, 例如:

```fsharp
let intLists = arrayOf10Lists<int>
let floatLists = arrayOf10Lists<float>
```

## <a name="see-also"></a>请参阅

- [类型推断](../type-inference.md)
- [泛型](index.md)
- [静态解析的类型参数](statically-resolved-type-parameters.md)
- [约束](constraints.md)
