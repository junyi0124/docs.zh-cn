---
title: 什么是 F#
description: '了解 F # 编程语言的定义，以及 F # 编程的作用。 了解丰富的数据类型、函数以及它们如何组合在一起。'
ms.date: 08/03/2018
ms.openlocfilehash: a6bad3e1db63c3fe948b5916925d5eb24a18a41c
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96739472"
---
# <a name="what-is-f"></a>什么是 F\#

F # 是一种函数编程语言，可方便编写正确且可维护的代码。

F # 编程主要涉及如何定义自动推断和通用化的类型和函数。 这样，你的关注点将保留在问题域上并操作其数据，而不是编程的详细信息。

```fsharp
open System // Gets access to functionality in System namespace.

// Defines a function that takes a name and produces a greeting.
let getGreeting name = $"Hello, {name}! Isn't F# great?"

[<EntryPoint>]
let main args =
    // Defines a list of names
    let names = [ "Don"; "Julia"; "Xi" ]

    // Prints a greeting for each name!
    names
    |> List.map getGreeting
    |> List.iter (fun greeting -> printfn $"{greeting}")

    0
```

F # 具有许多功能，包括：

* 轻型语法
* 默认情况下不可变
* 类型推理和自动通用化
* 第一类函数
* 强大的数据类型
* 模式匹配
* 异步编程

[F # 语言参考](./language-reference/index.md)中记录了一组完整的功能。

## <a name="rich-data-types"></a>丰富的数据类型

数据类型（如 [记录](./language-reference/records.md) 和可 [区分联合](./language-reference/discriminated-unions.md) ）允许您表示复杂的数据和域。

```fsharp
// Group data with Records
type SuccessfulWithdrawal =
    { Amount: decimal
      Balance: decimal }

type FailedWithdrawal =
    { Amount: decimal
      Balance: decimal
      IsOverdraft: bool }

// Use discriminated unions to represent data of 1 or more forms
type WithdrawalResult =
    | Success of SuccessfulWithdrawal
    | InsufficientFunds of FailedWithdrawal
    | CardExpired of System.DateTime
    | UndisclosedFailure
```

F # 记录和可区分联合默认情况下为非 null、不可变和可比较，因此它们非常易于使用。

## <a name="enforced-correctness-with-functions-and-pattern-matching"></a>使用函数和模式匹配强制实现的正确性

F # 函数在实践中易于声明和强大。 与 [模式匹配](./language-reference/pattern-matching.md)一起使用时，它们允许您定义由编译器强制执行的行为。

```fsharp
// Returns a WithdrawalResult
let withdrawMoney amount = // Implementation elided

let handleWithdrawal amount =
    let w = withdrawMoney amount

    // The F# compiler enforces accounting for each case!
    match w with
    | Success s -> printfn "Successfully withdrew %f{s.Amount}"
    | InsufficientFunds f -> printfn "Failed: balance is %f{f.Balance}"
    | CardExpired d -> printfn "Failed: card expired on {d}"
    | UndisclosedFailure -> printfn "Failed: unknown :("
```

F # 函数也是第一类，这意味着它们可以作为参数传递，并从其他函数返回。

## <a name="functions-to-define-operations-on-objects"></a>用于定义对象操作的函数

F # 完全支持对象，当你需要混合数据和功能时，它们是有用的数据类型。 F # 函数用于处理对象。

```fsharp
type Set<'T when 'T: comparison>(elements: seq<'T>) =
    member s.IsEmpty = // Implementation elided
    member s.Contains (value) =// Implementation elided
    member s.Add (value) = // Implementation elided
    // ...
    // Further Implementation elided
    // ...
    interface IEnumerable<‘T>
    interface IReadOnlyCollection<‘T>

module Set =
    let isEmpty (set: Set<'T>) = set.IsEmpty

    let contains element (set: Set<'T>) = set.Contains(element)

    let add value (set: Set<'T>) = set.Add(value)
```

不是编写面向对象的代码，在 F # 中，你通常会编写将对象视为函数的另一个数据类型的代码。 [泛型接口](./language-reference/interfaces.md)、[对象表达式](./language-reference/object-expressions.md)和明智使用[成员](./language-reference/members/index.md)等功能在较大的 F # 程序中很常见。

## <a name="next-steps"></a>后续步骤

若要详细了解更多 F # 功能，请查看 [f # 教程](tour.md)。
