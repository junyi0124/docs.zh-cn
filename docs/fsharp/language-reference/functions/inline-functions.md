---
title: 内联函数
description: 了解如何使用F#直接集成到调用代码中的内联函数。
ms.date: 05/16/2016
ms.openlocfilehash: 2830d1ada5b3005c3fcae975a44e85a7c84554f7
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68630678"
---
# <a name="inline-functions"></a>内联函数

*内联函数*是直接集成到调用代码中的函数。

## <a name="using-inline-functions"></a>使用内联函数

使用静态类型参数时, 必须以内联方式为类型参数参数化的任何函数。 这可确保编译器可以解析这些类型参数。 当使用普通泛型类型参数时, 没有此类限制。

除了允许使用成员约束外, 内联函数对于优化代码非常有用。 但是, 如果过度利用内联函数, 则可能会导致您的代码不受编译器优化和库函数实现中的更改。 出于此原因, 应避免使用内联函数进行优化, 除非已尝试所有其他优化方法。 使函数或方法为内联可能会提高性能, 但并非总是如此。 因此, 您还应该使用性能度量来验证使任何给定函数的内联确实会产生积极影响。

`inline`修饰符可应用于顶层的函数、模块级别或类中的方法级别。

下面的代码示例演示顶级的内联函数、内联实例方法和内联静态方法。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-3/snippet201.fs)]

## <a name="inline-functions-and-type-inference"></a>内联函数和类型推理

存在影响类型`inline`推理的情况。 这是因为内联函数可以具有静态解析的类型参数, 而非内联函数不能。 下面的代码示例演示了一个很`inline`有用的情况, 因为你使用的是具有静态解析的类型参数的`float`函数 (即转换运算符)。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-3/snippet202.fs)]

`inline` 如果`int`不使用修饰符, 则类型推理强制函数采用特定类型 (在本例中为)。 但对于`inline`修饰符, 还会将函数推断为具有静态解析的类型参数。 `inline`通过修饰符, 将类型推断为以下内容:

```fsharp
^a -> unit when ^a : (static member op_Explicit : ^a -> float)
```

这意味着函数接受支持转换为**float**类型的任何类型。

## <a name="see-also"></a>请参阅

- [函数](index.md)
- [约束](../generics/constraints.md)
- [静态解析的类型参数](../generics/statically-resolved-type-parameters.md)
