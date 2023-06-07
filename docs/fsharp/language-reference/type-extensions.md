---
title: 类型扩展
description: '了解 F # 类型扩展如何允许您向以前定义的对象类型添加新成员。'
ms.date: 02/05/2020
ms.openlocfilehash: c9adddb3133a4af57a12be0b09c22954a8bff6a7
ms.sourcegitcommit: c3093e9d106d8ca87cc86eef1f2ae4ecfb392118
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/22/2020
ms.locfileid: "97737263"
---
# <a name="type-extensions"></a>类型扩展

类型扩展 (也称为 _扩大_) 是一系列功能，可让你向之前定义的对象类型添加新成员。 这三种功能包括：

- 内部类型扩展
- 可选类型扩展
- 扩展方法

每个可以在不同的方案中使用，并且具有不同的折衷。

## <a name="syntax"></a>语法

```fsharp
// Intrinsic and optional extensions
type typename with
    member self-identifier.member-name =
        body
    ...

// Extension methods
open System.Runtime.CompilerServices

[<Extension>]
type Extensions() =
    [<Extension>]
    static member extension-name (ty: typename, [args]) =
        body
    ...
```

## <a name="intrinsic-type-extensions"></a>内部类型扩展

内部类型扩展是扩展用户定义类型的类型扩展。

内部类型扩展必须在同一文件中定义 **，并** 在与它们所扩展的类型相同的命名空间或模块中定义。 任何其他定义都将导致其成为 [可选类型扩展](type-extensions.md#optional-type-extensions)。

内部类型扩展有时是将功能与类型声明分开的更清晰的方式。 下面的示例演示如何定义内部类型扩展：

```fsharp
namespace Example

type Variant =
    | Num of int
    | Str of string
  
module Variant =
    let print v =
        match v with
        | Num n -> printf "Num %d" n
        | Str s -> printf "Str %s" s

// Add a member to Variant as an extension
type Variant with
    member x.Print() = Variant.print x
```

使用类型扩展可以分隔以下各项：

- 类型的声明 `Variant`
- `Variant`根据类的 "形状" 打印类的功能
- 使用对象样式表示法访问打印功能的一种方法 `.`

这是将所有内容定义为成员的替代方法 `Variant` 。 尽管它不是原本更好的方法，但在某些情况下，它可能是功能的更清晰表示形式。

内部类型扩展将编译为它们增加的类型的成员，并在反射检查类型时在类型上显示。

## <a name="optional-type-extensions"></a>可选类型扩展

可选类型扩展是在要扩展的类型的原始模块、命名空间或程序集的外部显示的扩展。

可选类型扩展对于扩展你自己尚未定义的类型很有用。 例如：

```fsharp
module Extensions

type IEnumerable<'T> with
    /// Repeat each element of the sequence n times
    member xs.RepeatElements(n: int) =
        seq {
            for x in xs do
                for _ in 1 .. n -> x
        }
```

你现在可以访问，就像它是的成员，只要 `RepeatElements` <xref:System.Collections.Generic.IEnumerable%601> `Extensions` 模块是在你所使用的范围中打开的。

可选扩展在由反射检查时不会出现在扩展类型上。 可选扩展必须在模块中，且仅当包含该扩展的模块处于打开状态或处于范围内时，它们才在范围内。

可选扩展成员将编译为静态成员，其对象实例将被隐式传递为第一个参数。 但是，它们的作用就像是实例成员或静态成员的声明方式。

可选扩展成员也不适用于 c # 或 Visual Basic 使用者。 它们只能在其他 F # 代码中使用。

## <a name="generic-limitation-of-intrinsic-and-optional-type-extensions"></a>内部和可选类型扩展的泛型限制

可以在类型变量受到约束的泛型类型上声明类型扩展。 要求是扩展声明的约束与声明类型的约束匹配。

但是，即使在声明的类型和类型扩展之间进行了匹配，约束仍有可能由扩展成员的主体推断，该扩展成员在类型参数上施加不同于声明类型的要求。 例如：

```fsharp
open System.Collections.Generic

// NOT POSSIBLE AND FAILS TO COMPILE!
//
// The member 'Sum' has a different requirement on 'T than the type IEnumerable<'T>
type IEnumerable<'T> with
    member this.Sum() = Seq.sum this
```

无法获取此代码以使用可选类型扩展：

- 同样， `Sum` 成员对 `'T` (`static member get_Zero` 和) 具有不同 `static member (+)` 于类型扩展所定义的约束。
- 修改类型扩展以使相同的约束不再 `Sum` 与中定义的约束匹配 `IEnumerable<'T>` 。
- 更改 `member this.Sum` 为 `member inline this.Sum` 将发出错误，指出类型约束不匹配。

所需的是 "在空间中浮动" 的静态方法，并可以显示为扩展类型。 这就是需要扩展方法的地方。

## <a name="extension-methods"></a>扩展方法

最后，扩展方法 (有时称为 "c # 样式扩展成员" ) 可在 F # 中声明为类上的静态成员方法。

当您希望在将约束类型变量的泛型类型上定义扩展时，扩展方法非常有用。 例如：

```fsharp
namespace Extensions

open System.Collections.Generic
open System.Runtime.CompilerServices

[<Extension>]
type IEnumerableExtensions =
    [<Extension>]
    static member inline Sum(xs: IEnumerable<'T>) = Seq.sum xs
```

使用时，此代码将使其看起来像 `Sum` 是在上定义的 <xref:System.Collections.Generic.IEnumerable%601> ，只要 `Extensions` 打开或在范围内。

为了使扩展可用于 VB.NET 代码，在 `ExtensionAttribute` 程序集级别需要额外的内容：

```fsharp
module AssemblyInfo
open System.Runtime.CompilerServices
[<assembly:Extension>]
do ()
```

## <a name="other-remarks"></a>其他备注

类型扩展还具有以下属性：

- 可访问的任何类型都可以进行扩展。
- 内部和可选类型扩展可以定义 _任何_ 成员类型，而不仅仅是方法。 例如，也可以扩展属性。
- `self-identifier`[语法](type-extensions.md#syntax)中的标记表示正在调用的类型的实例，就像普通成员一样。
- 扩展成员可以是静态成员或实例成员。
- 类型扩展上的类型变量必须与声明类型的约束匹配。

对于类型扩展也存在以下限制：

- 类型扩展不支持虚拟或抽象方法。
- 类型扩展不支持作为扩大的重写方法。
- 类型扩展不支持 [静态解析的类型参数](./generics/statically-resolved-type-parameters.md)。
- 可选类型扩展不支持作为扩大的构造函数。
- 不能对 [类型缩写](type-abbreviations.md)定义类型扩展。
- 类型扩展对于 (无效， `byref<'T>` 但它们可以) 声明。
- 类型扩展对于特性 (无效，但它们可以) 声明。
- 你可以定义重载同名的其他方法的扩展，但 F # 编译器会在出现不明确的调用时为非扩展方法提供首选项。

最后，如果一种类型存在多个内部类型扩展，则所有成员都必须是唯一的。 对于可选类型扩展，不同类型扩展中具有相同类型的成员可以具有相同的名称。 仅当客户端代码打开两个不同的作用域定义相同的成员名称时，才会发生多义性错误。

## <a name="see-also"></a>另请参阅

- [F# 语言参考](index.md)
- [成员](./members/index.md)
