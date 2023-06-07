---
title: 可区分联合
description: '了解如何使用 F # 可区分联合。'
ms.date: 08/15/2020
ms.openlocfilehash: f90ac2c2ea21182cf5fd3657d2ada00a763139fe
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96740232"
---
# <a name="discriminated-unions"></a>可区分联合

可区分联合提供的值支持可以是多个命名用例中的一个，可能每个值都有不同的值和类型。 可区分联合适用于异类数据;可能有特殊情况的数据，包括有效和错误事例;不同类型的数据的不同之处在于：作为小型对象层次结构的替代方法。 此外，递归的可区分联合用于表示树数据结构。

## <a name="syntax"></a>语法

```fsharp
[ attributes ]
type [accessibility-modifier] type-name =
    | case-identifier1 [of [ fieldname1 : ] type1 [ * [ fieldname2 : ] type2 ...]
    | case-identifier2 [of [fieldname3 : ]type3 [ * [ fieldname4 : ]type4 ...]

    [ member-list ]
```

## <a name="remarks"></a>备注

可区分联合类似于其他语言中的联合类型，但有一些差异。 与 c + + 中的联合类型或 Visual Basic 中的变量类型一样，值中存储的数据不是固定的;它可以是几个不同的选项之一。 与其他这些语言中的联合不同，每个可能的选项都有一个 *case 标识符*。 Case 标识符是此类型的对象可能具有的各种类型的值的名称;值是可选的。 如果值不存在，则该事例等效于枚举用例。 如果存在值，则每个值可以是指定类型的单个值，也可以是聚合同一类型或不同类型的多个字段的元组。 您可以为单个字段指定一个名称，但该名称是可选的，即使在同一种情况下，也是如此。

可区分的联合的辅助功能默认为 `public` 。

例如，请考虑下面的形状类型声明。

```fsharp
type Shape =
    | Rectangle of width : float * length : float
    | Circle of radius : float
    | Prism of width : float * float * height : float
```

前面的代码声明了一个可区分的联合形状，该形状的值可以是以下三种情况中的任何一种：矩形、圆形和 Prism。 每个事例都有一组不同的字段。 矩形用例有两个命名字段，两个均为类型 `float` ，名称为 width 和 length。 圆形大小写只包含一个命名字段 "半径"。 Prism 用例有三个字段，其中两个字段 (的宽度和高度) 均为 "命名字段"。 未命名字段称为匿名字段。

您可以根据以下示例，为已命名和匿名字段提供值来构造对象。

```fsharp
let rect = Rectangle(length = 1.3, width = 10.0)
let circ = Circle (1.0)
let prism = Prism(5., 2.0, height = 3.0)
```

此代码显示，您可以在初始化中使用已命名的字段，也可以依赖于声明中的字段顺序，而只是提供每个字段的值。 前面代码中的构造函数调用 `rect` 使用命名字段，但的构造函数调用 `circ` 使用排序。 可以混合使用已排序的字段和命名字段，如的构造中所示 `prism` 。

该 `option` 类型是 F # 核心库中的一个简单的可区分联合。 此 `option` 类型声明如下。

```fsharp
// The option type is a discriminated union.
type Option<'a> =
    | Some of 'a
    | None
```

上面的代码指定该类型 `Option` 是具有两个用例（和）的可区分联合 `Some` `None` 。 `Some`事例具有一个关联的值，它由一个匿名字段组成，该字段的类型由类型参数表示 `'a` 。 `None`事例没有关联值。 因此， `option` 类型指定泛型类型，该类型具有某种类型的值或没有值。 该类型 `Option` 还具有一个 `option` 更常用的小写类型别名。

Case 标识符可用作可区分联合类型的构造函数。 例如，下面的代码用于创建类型的值 `option` 。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2001.fs)]

Case 标识符还用于模式匹配表达式。 在模式匹配表达式中，会为与单个用例关联的值提供标识符。 例如，在下面的代码中， `x` 是给定与类型的大小写关联的值的标识符 `Some` `option` 。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2002.fs)]

在模式匹配表达式中，可以使用命名字段来指定可区分的联合匹配。 对于之前声明的形状类型，可以使用命名字段，如以下代码所示，以便提取字段的值。

```fsharp
let getShapeWidth shape =
    match shape with
    | Rectangle(width = w) -> w
    | Circle(radius = r) -> 2. * r
    | Prism(width = w) -> w
```

通常情况下，可以使用 case 标识符，无需使用联合名称来限定它们。 如果希望名称始终使用联合的名称进行限定，则可将 [RequireQualifiedAccess](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-requirequalifiedaccessattribute.html) 特性应用于联合类型定义。

### <a name="unwrapping-discriminated-unions"></a>解包可区分联合

在 F # 中，可区分联合通常用于包装单个类型的域建模。 也可以通过模式匹配轻松提取基础值。 对于一个事例，无需使用 match 表达式：

```fsharp
let ([UnionCaseIdentifier] [values]) = [UnionValue]
```

下面的示例演示这一操作：

```fsharp
type ShaderProgram = | ShaderProgram of id:int

let someFunctionUsingShaderProgram shaderProgram =
    let (ShaderProgram id) = shaderProgram
    // Use the unwrapped value
    ...
```

函数参数中也可以直接使用模式匹配，因此，可以将一个事例解包到其中：

```fsharp
let someFunctionUsingShaderProgram (ShaderProgram id) =
    // Use the unwrapped value
    ...
```

## <a name="struct-discriminated-unions"></a>结构可区分联合

您还可以将可区分联合表示为结构。  此操作是通过属性实现的 `[<Struct>]` 。

```fsharp
[<Struct>]
type SingleCase = Case of string

[<Struct>]
type Multicase =
    | Case1 of Case1 : string
    | Case2 of Case2 : int
    | Case3 of Case3 : double
```

由于这些是值类型而不是引用类型，因此与引用可区分联合相比，有一些额外的注意事项：

1. 它们被复制为值类型并具有值类型语义。
2. 不能将递归类型定义用于 multicase 结构可区分联合。
3. 必须为 multicase 结构可区分联合提供唯一的事例名称。

## <a name="using-discriminated-unions-instead-of-object-hierarchies"></a>使用可区分联合而不是对象层次结构

通常，可以将可区分联合用作小型对象层次结构的更简单的替代方法。 例如，可以使用以下可区分联合，而不是 `Shape` 具有用于圆形、方形等的派生类型的基类。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2003.fs)]

与在面向对象的实现中使用的一样，可以使用模式匹配分支到适当的公式来计算这些数量，而不是使用虚方法来计算区域或外围网络。 在下面的示例中，使用不同的公式来计算区域，具体取决于形状。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2004.fs)]

输出如下所示：

```console
Area of circle that has radius 15.000000: 706.858347
Area of square that has side 10.000000: 100.000000
Area of rectangle that has height 5.000000 and width 10.000000 is 50.000000
```

## <a name="using-discriminated-unions-for-tree-data-structures"></a>对树形数据结构使用可区分联合

可区分联合可以是递归的，这意味着可将联合本身包含在一个或多个事例的类型中。 递归的可区分联合可用于创建树结构，用于在编程语言中为表达式建模。 在下面的代码中，递归的可区分联合用于创建二进制树数据结构。 联合包括两个事例，即 `Node` 一个具有整数值的节点、左右子树和 `Tip` ，后者终止树。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2005.fs)]

在前面的代码中， `resultSumTree` 的值为10。 下图显示了的树结构 `myTree` 。

![显示 myTree 的树结构的关系图。](../media/discriminated-unions/tree-structure-mytree.png)

如果树中的节点是异类的，则可区分的联合工作良好。 在下面的代码中，类型 `Expression` 表示简单编程语言中表达式的抽象语法树，它支持数字和变量的加法和乘法。 某些联合用例不是递归的，表示 `Number` ()  () 或变量的数字 `Variable` 。 其他情况是递归的，表示操作 (`Add` 和 `Multiply`) ，其中操作数也是表达式。 `Evaluate`函数使用 match 表达式以递归方式处理语法树。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet2006.fs)]

执行此代码时，的值 `result` 为5。

## <a name="members"></a>成员

可以在可区分的联合上定义成员。 下面的示例演示如何定义属性和实现接口：

```fsharp
open System

type IPrintable =
    abstract Print: unit -> unit

type Shape =
    | Circle of float
    | EquilateralTriangle of float
    | Square of float
    | Rectangle of float * float

    member this.Area =
        match this with
        | Circle r -> 2.0 * Math.PI * r
        | EquilateralTriangle s -> s * s * sqrt 3.0 / 4.0
        | Square s -> s * s
        | Rectangle(l, w) -> l * w

    interface IPrintable with
        member this.Print () =
            match this with
            | Circle r -> printfn $"Circle with radius %f{r}"
            | EquilateralTriangle s -> printfn $"Equilateral Triangle of side %f{s}"
            | Square s -> printfn $"Square with side %f{s}"
            | Rectangle(l, w) -> printfn $"Rectangle with length %f{l} and width %f{w}"
```

## <a name="common-attributes"></a>常见属性

以下属性通常出现在可区分联合中：

- `[<RequireQualifiedAccess>]`
- `[<NoEquality>]`
- `[<NoComparison>]`
- `[<Struct>]`

## <a name="see-also"></a>另请参阅

- [F# 语言参考](index.md)
