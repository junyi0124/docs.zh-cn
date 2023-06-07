---
title: 'F # 4.5 中的新增功能-F # 指南'
description: '获取 F # 4.5 中提供的新功能的概述。'
ms.date: 11/27/2019
ms.openlocfilehash: 2c978c66a4bf231398508cbc1cbb8839228ea8e9
ms.sourcegitcommit: 71b8f5a2108a0f1a4ef1d8d75c5b3e129ec5ca1e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/29/2020
ms.locfileid: "84202355"
---
# <a name="whats-new-in-f-45"></a>F # 4.5 中的新增功能

F # 4.5 添加了对 F # 语言的多项改进。 其中的许多功能都一起添加，使你能够以 F # 编写有效的代码，同时确保此代码是安全的。 这样做意味着在使用这些构造时，将一些概念添加到语言，并进行大量的编译器分析。

## <a name="get-started"></a>入门

F # 4.5 适用于所有 .NET Core 分发版和 Visual Studio 工具。 [开始处理 F #](../get-started/index.md)以了解详细信息。

## <a name="span-and-byref-like-structs"></a>跨越和 byref 类似的结构

<xref:System.Span%601>.Net Core 中引入的类型允许以强类型方式表示内存中的缓冲区，这在 f # 中现在允许 f # 4.5 开始。 下面的示例演示如何重新使用在 <xref:System.Span%601> 具有不同缓冲区表示形式的上操作的函数：

```fsharp
let safeSum (bytes: Span<byte>) =
    let mutable sum = 0
    for i in 0 .. bytes.Length - 1 do
        sum <- sum + int bytes.[i]
    sum

// managed memory
let arrayMemory = Array.zeroCreate<byte>(100)
let arraySpan = new Span<byte>(arrayMemory)

safeSum(arraySpan) |> printfn "res = %d"

// native memory
let nativeMemory = Marshal.AllocHGlobal(100);
let nativeSpan = new Span<byte>(nativeMemory.ToPointer(), 100)

safeSum(nativeSpan) |> printfn "res = %d"
Marshal.FreeHGlobal(nativeMemory)

// stack memory
let mem = NativePtr.stackalloc<byte>(100)
let mem2 = mem |> NativePtr.toVoidPtr
let stackSpan = Span<byte>(mem2, 100)

safeSum(stackSpan) |> printfn "res = %d"
```

这项工作的一个重要方面是，跨越和其他[类似 byref 的结构](../language-reference/structures.md#byreflike-structs)都具有非常严格的静态分析，这些分析会将其使用方式限制为意外的方式。 这是 F # 4.5 中引入的性能、表现力和安全之间的基本折衷。

## <a name="revamped-byrefs"></a>改进 byref

在 F # 4.5 之前，F # 中的[byref](../language-reference/byrefs.md)不安全，错误适用于许多应用程序。 F # 4.5 中解决了有关 byref 的 Soundness 问题，同时还应用了对 span 和类似 byref 的结构执行相同的静态分析。

### <a name="inreft-and-outreft"></a>inref< 不> 和 outref<>

为了表示只读、只写和读/写托管指针的概念，F # 4.5 引入了 `inref<'T>` ， `outref<'T>` 分别表示只读和只写指针的类型。 每个都有不同的语义。 例如，无法写入 `inref<'T>` ：

```fsharp
let f (dt: inref<DateTime>) =
    dt <- DateTime.Now // ERROR - cannot write to an inref!
```

默认情况下，类型推理会将托管指针推断为 `inref<'T>` 包含 F # 代码的不可变性质的行，除非已将某些内容声明为可变。 若要使某些内容成为可写的，需要在将 `mutable` 其地址传递给操作它的函数或成员之前声明类型。 若要了解详细信息，请参阅[byref](../language-reference/byrefs.md)。

## <a name="readonly-structs"></a>只读结构

从 F # 4.5 开始，你可以使用批注结构， <xref:System.Runtime.CompilerServices.IsReadOnlyAttribute> 如下所示：

```fsharp
[<IsReadOnly; Struct>]
type S(count1: int, count2: int) =
    member x.Count1 = count1
    member x.Count2 = count2
```

这不允许您在结构中声明可变成员，并发出允许 F # 和 c # 在从程序集使用时将其视为只读的元数据。 若要了解详细信息，请参阅[ReadOnly 结构](../language-reference/structures.md#readonly-structs)。

## <a name="void-pointers"></a>Void 指针

`voidptr`类型已添加到 F # 4.5，如下函数所示：

* `NativePtr.ofVoidPtr`将 void 指针转换为本机 int 指针
* `NativePtr.toVoidPtr`将本机 int 指针转换为 void 指针

与使用 void 指针的本机组件进行互操作时，这非常有用。

## <a name="the-match-keyword"></a>`match!` 关键字

`match!`关键字可在计算表达式内部增强模式匹配：

```fsharp
// Code that returns an asynchronous option
let checkBananaAsync (s: string) =
    async {
        if s = "banana" then
            return Some s
        else
            return None
    }

// Now you can use 'match!'
let funcWithString (s: string) =
    async {
        match! checkBananaAsync s with
        | Some bananaString -> printfn "It's banana!"
        | None -> printfn "%s" s
}
```

这允许您缩短经常涉及混合选项（或其他类型）和计算表达式（如 async）的代码。 若要了解详细信息，请参阅[match！](../language-reference/computation-expressions.md#match)。

## <a name="relaxed-upcasting-requirements-in-array-list-and-sequence-expressions"></a>数组、列表和序列表达式中的宽松向上转换要求

通常情况下，混合类型可以从数组、列表和序列表达式中的另一个继承，而这种混合类型需要将任何派生类型通过或向上转换为其父类型 `:>` `upcast` 。 这现在是宽松的，如下所示：

```fsharp
let x0 : obj list  = [ "a" ] // ok pre-F# 4.5
let x1 : obj list  = [ "a"; "b" ] // ok pre-F# 4.5
let x2 : obj list  = [ yield "a" :> obj ] // ok pre-F# 4.5

let x3 : obj list  = [ yield "a" ] // Now ok for F# 4.5, and can replace x2
```

## <a name="indentation-relaxation-for-array-and-list-expressions"></a>数组和列表表达式的缩进 relaxation

在 F # 4.5 之前，需要在将数组和列表表达式作为参数传递给方法调用时过度缩进。 不再需要此操作：

```fsharp
module NoExcessiveIndenting =
    System.Console.WriteLine(format="{0}", arg = [|
        "hello"
    |])
    System.Console.WriteLine([|
        "hello"
    |])
```
