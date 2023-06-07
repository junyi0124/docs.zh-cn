---
title: Byref
description: '了解 F # 中用于低级别编程的 byref 和 byref 类型。'
ms.date: 11/04/2019
ms.openlocfilehash: ff2c06d8940f7341d5d8b1d942be264bfac586c5
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96740310"
---
# <a name="byrefs"></a>Byref

F # 有两个主要功能区域，用于处理低级别编程的空间：

* `byref` / `inref` / `outref` 类型为托管指针。 它们对使用情况有限制，因此无法编译在运行时无效的程序。
* `byref`类似的结构，它是具有类似语义并且与相同的编译时限制的[结构](structures.md) `byref<'T>` 。 其中一个示例是 <xref:System.Span%601> 。

## <a name="syntax"></a>语法

```fsharp
// Byref types as parameters
let f (x: byref<'T>) = ()
let g (x: inref<'T>) = ()
let h (x: outref<'T>) = ()

// Calling a function with a byref parameter
let mutable x = 3
f &x

// Declaring a byref-like struct
open System.Runtime.CompilerServices

[<Struct; IsByRefLike>]
type S(count1: int, count2: int) =
    member x.Count1 = count1
    member x.Count2 = count2
```

## <a name="byref-inref-and-outref"></a>Byref、inref 和 outref

有三种形式的 `byref` ：

* `inref<'T>`，用于读取基础值的托管指针。
* `outref<'T>`，用于写入基础值的托管指针。
* `byref<'T>`，用于读取和写入基础值的托管指针。

`byref<'T>`可以在 `inref<'T>` 需要时传递。 同样， `byref<'T>` 可以将传递到预期的 `outref<'T>` 位置。

## <a name="using-byrefs"></a>使用 byref

若要使用 `inref<'T>` ，需要使用以下内容获取指针值 `&` ：

```fsharp
open System

let f (dt: inref<DateTime>) =
    printfn $"Now: %O{dt}"

let usage =
    let dt = DateTime.Now
    f &dt // Pass a pointer to 'dt'
```

若要使用或写入指针 `outref<'T>` `byref<'T>` ，还必须使您获取指向的指针的值 `mutable` 。

```fsharp
open System

let f (dt: byref<DateTime>) =
    printfn $"Now: %O{dt}"
    dt <- DateTime.Now

// Make 'dt' mutable
let mutable dt = DateTime.Now

// Now you can pass the pointer to 'dt'
f &dt
```

如果只是编写指针而不是读取它，请考虑使用 `outref<'T>` 而不是 `byref<'T>` 。

### <a name="inref-semantics"></a>Inref 语义

请考虑以下代码：

```fsharp
let f (x: inref<SomeStruct>) = x.SomeField
```

从语义上说，这意味着：

* 指针的刀柄 `x` 只能用它来读取值。
* 获取到 `struct` 嵌套在中的字段 `SomeStruct` 的任何指针都具有给定的类型 `inref<_>` 。

以下情况也成立：

* 不意味着其他线程或别名没有写入访问权限 `x` 。
* 这并不是不可变的，因为这是不可 `SomeStruct` 变的 `x` `inref` 。

但是，对于 **不可变的** F # 值类型，指针将 `this` 推断为 `inref` 。

所有这些规则一起表示指针的持有者 `inref` 可能不会修改所指向的内存的即时内容。

### <a name="outref-semantics"></a>Outref 语义

的用途 `outref<'T>` 是指示只应将指针写入。 意外， `outref<'T>` 不管名称如何，都允许读取基础值。 这是为了实现兼容性。 在语义上， `outref<'T>` 与不同 `byref<'T>` 。

### <a name="interop-with-c"></a>与 C 互操作\#

`in ref` `out ref` 除返回外，c # 还支持和关键字 `ref` 。 下表显示 F # 如何解释 c # 发出的内容：

|C # 构造|F # 推导|
|------------|---------|
|`ref` 返回值|`outref<'T>`|
|`ref readonly` 返回值|`inref<'T>`|
|`in ref` 参数|`inref<'T>`|
|`out ref` 参数|`outref<'T>`|

下表显示 F # 发出的内容：

|F # 构造|发出的构造|
|------------|-----------------|
|`inref<'T>` 参数|`[In]` 参数上的属性|
|`inref<'T>` 返回|`modreq` 值的属性|
|`inref<'T>` 在抽象槽或实现中|`modreq` 参数或返回|
|`outref<'T>` 参数|`[Out]` 参数上的属性|

### <a name="type-inference-and-overloading-rules"></a>类型推理和重载规则

`inref<'T>`在以下情况下，类型由 F # 编译器推断：

1. 具有特性的 .NET 参数或返回类型 `IsReadOnly` 。
2. `this`结构类型上没有可变字段的指针。
3. 派生自另一个指针的内存位置的地址 `inref<_>` 。

当使用的是隐式地址时 `inref` ，具有类型的参数的重载 `SomeType` 优先于具有类型的参数的重载 `inref<SomeType>` 。 例如：

```fsharp
type C() =
    static member M(x: System.DateTime) = x.AddDays(1.0)
    static member M(x: inref<System.DateTime>) = x.AddDays(2.0)
    static member M2(x: System.DateTime, y: int) = x.AddDays(1.0)
    static member M2(x: inref<System.DateTime>, y: int) = x.AddDays(2.0)

let res = System.DateTime.Now
let v =  C.M(res)
let v2 =  C.M2(res, 4)
```

在这两种情况下，将解析重载， `System.DateTime` 而不是重载 `inref<System.DateTime>` 。

## <a name="byref-like-structs"></a>类似 Byref 的结构

除了 `byref` / `inref` / `outref` 三个，还可以定义自己的结构，这些结构可以遵循 `byref` 类似的语义。 这是通过属性完成的 <xref:System.Runtime.CompilerServices.IsByRefLikeAttribute> ：

```fsharp
open System
open System.Runtime.CompilerServices

[<IsByRefLike; Struct>]
type S(count1: Span<int>, count2: Span<int>) =
    member x.Count1 = count1
    member x.Count2 = count2
```

`IsByRefLike` 不意味着 `Struct` 。 这两者都必须存在于类型上。

`byref`F # 中的 "like" 结构是堆栈绑定值类型。 永远不会在托管堆上分配。 `byref`类似的结构可用于实现高性能编程，因为它是针对生存期和非捕获的一组强检查强制实施的。 规则如下：

* 它们可用作函数参数、方法参数、局部变量、方法返回。
* 它们不能是类或普通结构的静态成员或实例成员。
* 不能通过任何闭包构造 (`async` 方法或 lambda 表达式来捕获它们) 。
* 它们不能用作泛型参数。

最后一点对于 F # 管道样式编程至关重要，因为它 `|>` 是一个参数化其输入类型的泛型函数。 此限制可能会 `|>` 在将来为宽松，因为它是内联的，不会对其主体中的非内联泛型函数进行任何调用。

尽管这些规则对使用进行了严格的限制，但这样做可确保以安全的方式满足高性能计算的承诺。

## <a name="byref-returns"></a>Byref 返回

可以生成和使用来自 F # 函数或成员的 Byref 返回。 使用 `byref` 返回方法时，会隐式取消引用该值。 例如：

```fsharp
let squareAndPrint (data : byref<int>) =
    let squared = data*data    // data is implicitly dereferenced
    printfn $"%d{squared}"
```

若要返回值 byref，则包含值的变量的生存期必须长于当前范围。
此外，若要返回 byref，请使用 `&value` (其中，值是生存时间比当前范围) 长的变量。

```fsharp
let mutable sum = 0
let safeSum (bytes: Span<byte>) =
    for i in 0 .. bytes.Length - 1 do
        sum <- sum + int bytes.[i]
    &sum  // sum lives longer than the scope of this function.
```

若要避免隐式取消引用（如通过多个链式调用传递引用），请使用 `&x` (，其中 `x` 是) 值。

还可以直接分配给返回 `byref` 。 请考虑以下 (高命令式) 程序：

```fsharp
type C() =
    let mutable nums = [| 1; 3; 7; 15; 31; 63; 127; 255; 511; 1023 |]

    override _.ToString() = String.Join(' ', nums)

    member _.FindLargestSmallerThan(target: int) =
        let mutable ctr = nums.Length - 1

        while ctr > 0 && nums.[ctr] >= target do ctr <- ctr - 1

        if ctr > 0 then &nums.[ctr] else &nums.[0]

[<EntryPoint>]
let main argv =
    let c = C()
    printfn $"Original sequence: %O{c}"

    let v = &c.FindLargestSmallerThan 16

    v <- v*2 // Directly assign to the byref return

    printfn $"New sequence:      %O{c}"

    0 // return an integer exit code
```

以下是输出：

```console
Original sequence: 1 3 7 15 31 63 127 255 511 1023
New sequence:      1 3 7 30 31 63 127 255 511 1023
```

## <a name="scoping-for-byrefs"></a>Byref 的作用域

`let`绑定值的引用不能超过其定义的范围。 例如，不允许以下操作：

```fsharp
let test2 () =
    let x = 12
    &x // Error: 'x' exceeds its defined scope!

let test () =
    let x =
        let y = 1
        &y // Error: `y` exceeds its defined scope!
    ()
```

这会阻止你获取不同的结果，具体取决于你是否编译了优化。
