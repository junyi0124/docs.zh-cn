---
title: 类型推断
description: 了解F#编译器如何推断值、变量、参数和返回值的类型。
ms.date: 05/16/2016
ms.openlocfilehash: 4b18c1a67a8b7ddadb4fb334ea4736e9fd29feb0
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68630183"
---
# <a name="type-inference"></a>类型推断

本主题介绍F#编译器如何推断值、变量、参数和返回值的类型。

## <a name="type-inference-in-general"></a>类型推理概述

类型推理的理念是, 无需指定F#构造类型, 除非编译器无法最终推断类型。 省略显式类型信息并不意味着这F#是动态类型化语言, 或者中F#的值是弱类型。 F#是一种静态类型化语言, 这意味着编译器将在编译过程中为每个构造推导一个确切的类型。 如果编译器没有足够的信息来推导每个构造的类型, 则必须提供附加类型信息, 通常是在代码中的某个位置添加显式类型批注。

## <a name="inference-of-parameter-and-return-types"></a>参数和返回类型的推理

在参数列表中, 无需指定每个参数的类型。 而且, F#是一种静态类型的语言, 因此每个值和表达式在编译时都有一个明确的类型。 对于未显式指定的类型, 编译器将根据上下文推断类型。 如果未指定该类型, 则将其推断为泛型。 如果代码使用值的方式不一致, 这样一来, 就不存在满足所有值使用的单推断类型, 编译器将报告错误。

函数的返回类型由函数中最后一个表达式的类型确定。

例如, 在下面的代码中, 参数类型`a`和`b`和返回类型全都推断为`int` , 因为文字`100`的类型`int`为。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-3/snippet301.fs)]

您可以通过更改文本来影响类型推理。 `100`如果`uint32`通过追加`b`后缀来`u`创建,则会将、和返回值的类型推断为。`a` `uint32`

您还可以通过使用隐含对类型的限制的其他构造 (如仅适用于特定类型的函数和方法) 来影响类型推理。

此外, 还可以将显式类型批注应用于函数或方法参数或表达式中的变量, 如下面的示例中所示。 如果不同的约束之间发生冲突, 将导致错误。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-3/snippet302.fs)]

还可以通过在所有参数后面提供类型注释来显式指定函数的返回值。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-3/snippet303.fs)]

类型批注对参数有用的常见情况是: 参数为对象类型, 并且你想要使用成员。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-3/snippet304.fs)]

## <a name="automatic-generalization"></a>自动泛化

如果函数代码不依赖于参数的类型, 则编译器会将参数视为泛型。 这称为*自动泛化*, 它是一种功能强大的工具, 可在不增加复杂性的情况下编写泛型代码。

例如, 下面的函数将任意类型的两个参数合并为一个元组。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-3/snippet305.fs)]

将类型推断为

```fsharp
'a -> 'b -> 'a * 'b
```

## <a name="additional-information"></a>其他信息

F#语言规范中更详细地介绍了类型推理。

## <a name="see-also"></a>请参阅

- [自动泛化](./generics/automatic-generalization.md)
