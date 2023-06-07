---
title: 4\.6 中F#的新增功能- F#指南
description: 获取4.6 中F#提供的新功能的概述。
ms.date: 11/27/2019
ms.openlocfilehash: 620c1edd8ea212fee306a02d5844b6b322808251
ms.sourcegitcommit: 19014f9c081ca2ff19652ca12503828db8239d48
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2020
ms.locfileid: "76980387"
---
# <a name="whats-new-in-f-46"></a>4\.6 中F#的新增功能

F#4.6 向F#语言增加了多项改进。

## <a name="get-started"></a>入门

F#4.6 适用于所有 .NET Core 分发版和 Visual Studio 工具。 [开始F# ](../get-started/index.md)了解更多详细信息。

## <a name="anonymous-records"></a>匿名记录

[匿名记录](../language-reference/anonymous-records.md)是4.6 中F#引入的F#一种新类型。 它们是命名值的简单聚合，在使用之前无需声明。 可以将它们声明为结构或引用类型。 默认情况下，它们是引用类型。

```fsharp
open System

let getCircleStats radius =
    let d = radius * 2.0
    let a = Math.PI * (radius ** 2.0)
    let c = 2.0 * Math.PI * radius

    {| Diameter = d; Area = a; Circumference = c |}

let r = 2.0
let stats = getCircleStats r
printfn "Circle with radius: %f has diameter %f, area %f, and circumference %f"
    r stats.Diameter stats.Area stats.Circumference
```

如果要对值类型进行分组并在性能相关的方案中运行，还可以将它们声明为结构：

```fsharp
open System

let getCircleStats radius =
    let d = radius * 2.0
    let a = Math.PI * (radius ** 2.0)
    let c = 2.0 * Math.PI * radius

    struct {| Diameter = d; Area = a; Circumference = c |}

let r = 2.0
let stats = getCircleStats r
printfn "Circle with radius: %f has diameter %f, area %f, and circumference %f"
    r stats.Diameter stats.Area stats.Circumference
```

它们的功能非常强大，可以在许多方案中使用。 了解有关[匿名记录](../language-reference/anonymous-records.md)的详细信息。

## <a name="valueoption-functions"></a>ValueOption 函数

4\.5 中F#添加的 ValueOption 类型现在具有选项类型的 "模块绑定函数奇偶校验"。 一些更常用的示例如下所示：

```fsharp
// Multiply a value option by 2 if it has  value
let xOpt = ValueSome 99
let result = xOpt |> ValueOption.map (fun v -> v * 2)

// Reverse a string if it exists
let strOpt = ValueSome "Mirror image"
let reverse (str: string) =
    match str with
    | null
    | "" -> ValueNone
    | s ->
        str.ToCharArray()
        |> Array.rev
        |> string
        |> ValueSome

let reversedString = strOpt |> ValueOption.bind reverse
```

这允许在值类型提高性能的情况下，使用类似于 ValueOption 的选项。
