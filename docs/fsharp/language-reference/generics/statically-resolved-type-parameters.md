---
title: 静态解析的类型参数
description: 了解如何使用在编译F#时（而不是在运行时）替换为实际类型的静态解析的类型参数。
ms.date: 05/16/2016
ms.openlocfilehash: 017c18dd3caaa484ddc653557573f548e3224ca0
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425000"
---
# <a name="statically-resolved-type-parameters"></a>静态解析的类型参数

*静态解析的类型参数*是一个类型参数，它在编译时（而不是在运行时）替换为实际类型。 它们前面有一个插入符号（^）符号。

## <a name="syntax"></a>语法

```fsharp
ˆtype-parameter
```

## <a name="remarks"></a>备注

在F#语言中，有两种不同类型的类型参数。 第一种类型是标准泛型类型参数。 它们由撇号（'）表示，如 `'T` 和 `'U`中。 它们等效于其他 .NET Framework 语言的泛型类型参数。 另一种类型是静态解析的，它由一个插入符号表示，如 `^T` 和 `^U`中所示。

静态解析的类型参数主要用于与成员约束结合使用，后者是允许您指定类型参数必须具有特定成员才能使用的约束。 无法使用常规泛型类型参数创建这种约束。

下表总结了两种类型参数之间的相似性和差异。

|功能|泛型|静态解析|
|-------|-------|-------------------|
|语法|`'T`，`'U`|`^T`，`^U`|
|解决时间|运行时|编译时间|
|成员约束|不能与成员约束一起使用。|可以与成员约束一起使用。|
|代码生成|具有标准泛型类型参数的类型（或方法）导致生成单个泛型类型或方法。|生成类型和方法的多个实例化，每个实例对应于所需的类型。|
|与类型一起使用|可用于类型。|不能用于类型。|
|与内联函数一起使用|否。 不能使用标准泛型类型参数参数化内联函数。|可以。 静态解析的类型参数不能用于非内联的函数或方法。|

许多F#核心库函数（特别是运算符）都有静态解析的类型参数。 这些函数和运算符是内联的，为数值计算生成了高效的代码生成。

使用运算符或使用具有静态解析的类型参数的其他函数的内联方法和函数也可以使用静态解析的类型参数。 通常，类型推理会将此类内联函数推断为具有静态解析的类型参数。 下面的示例演示了一个运算符定义，该运算符定义被推断为具有静态解析的类型参数。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-3/snippet401.fs)]

`(+@)` 的解析类型基于 `(+)` 和 `(*)`的使用，这两种方法都会导致类型推理推断静态解析的类型参数上的成员约束。 解析后的类型（如F#解释器中所示）如下所示。

```fsharp
^a -> ^c -> ^d
when (^a or ^b) : (static member ( + ) : ^a * ^b -> ^d) and
(^a or ^c) : (static member ( * ) : ^a * ^c -> ^b)
```

输出如下所示。

```console
2
1.500000
```

从F# 4.1 开始，还可以在静态解析的类型参数签名中指定具体的类型名称。  在以前版本的语言中，类型名称实际上可能由编译器推断，但实际上无法在签名中指定。  从F# 4.1，你还可以在静态解析的类型参数签名中指定具体的类型名称。 以下是一个示例：

```fsharp
let inline konst x _ = x

type CFunctor() =
    static member inline fmap (f: ^a -> ^b, a: ^a list) = List.map f a
    static member inline fmap (f: ^a -> ^b, a: ^a option) =
        match a with
        | None -> None
        | Some x -> Some (f x)

    // default implementation of replace
    static member inline replace< ^a, ^b, ^c, ^d, ^e when ^a :> CFunctor and (^a or ^d): (static member fmap: (^b -> ^c) * ^d -> ^e) > (a, f) =
        ((^a or ^d) : (static member fmap : (^b -> ^c) * ^d -> ^e) (konst a, f))

    // call overridden replace if present
    static member inline replace< ^a, ^b, ^c when ^b: (static member replace: ^a * ^b -> ^c)>(a: ^a, f: ^b) =
        (^b : (static member replace: ^a * ^b -> ^c) (a, f))

let inline replace_instance< ^a, ^b, ^c, ^d when (^a or ^c): (static member replace: ^b * ^c -> ^d)> (a: ^b, f: ^c) =
        ((^a or ^c): (static member replace: ^b * ^c -> ^d) (a, f))

// Note the concrete type 'CFunctor' specified in the signature
let inline replace (a: ^a) (f: ^b): ^a0 when (CFunctor or  ^b): (static member replace: ^a *  ^b ->  ^a0) =
    replace_instance<CFunctor, _, _, _> (a, f)
```

## <a name="see-also"></a>请参阅

- [泛型](index.md)
- [类型推断](../type-inference.md)
- [自动泛化](automatic-generalization.md)
- [约束](constraints.md)
- [内联函数](../functions/inline-functions.md)
