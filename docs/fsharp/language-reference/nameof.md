---
title: Nameof
description: 了解 nameof 运算符，它是一项元编程功能，可用于在源代码中生成任何符号的名称。
ms.date: 11/12/2020
ms.openlocfilehash: 9947d7172d1b73db5c181297ec8b1131382e1f5e
ms.sourcegitcommit: 34968a61e9bac0f6be23ed6ffb837f52d2390c85
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/17/2020
ms.locfileid: "94688626"
---
# <a name="nameof"></a>Nameof

`nameof`表达式生成一个字符串常量，该常量与 source 中几乎任何 F # 构造的源中的名称匹配。

## <a name="syntax"></a>语法

```fsharp
nameof symbol
nameof<'TGeneric>
```

## <a name="remarks"></a>备注

`nameof` 通过解析传递给它的符号，并生成该符号的名称，因为它是在源代码中声明的。 这在各种情况下非常有用，例如日志记录，并可防止日志记录在源代码中发生更改。

```fsharp
let months =
    [
        "January"; "February"; "March"; "April";
        "May"; "June"; "July"; "August"; "September";
        "October"; "November"; "December"
    ]

let lookupMonth month =
    if (month > 12 || month < 1) then
        invalidArg (nameof month) ($"Value passed in was %d{month}.")

    months.[month-1]

printfn "%s" (lookupMonth 12)
printfn "%s" (lookupMonth 1)
printfn "%s" (lookupMonth 13)
```

最后一行将引发异常，并 `"month"` 将显示在错误消息中。

你可以创建几乎每个 F # 构造的名称：

```fsharp
module M =
    let f x = nameof x

printfn $"{(M.f 12)]}"
printfn $"{(nameof M)}"
printfn $"{(nameof M.f)}"
```

`nameof` 不是第一类函数，因此不能使用。 这意味着不能对其进行部分应用，并且无法通过 F # 管道运算符将值通过管道传递给它。

## <a name="nameof-on-operators"></a>运算符上的 Nameof

F # 中的运算符可通过两种方式使用：作为运算符文本本身，或表示已编译的窗体的符号。 `nameof` 运算符上的将生成运算符的名称，因为它是在源中声明的。 若要获取已编译的名称，请在源中使用编译的名称：

```fsharp
nameof(+) // "+"
nameof op_Addition // "op_Addition"
```

## <a name="nameof-on-generics"></a>泛型上的 Nameof

您还可以采用泛型类型参数的名称，但语法不同：

```fsharp
let f<'a> () = nameof<'a>
f() // "a"
```

`nameof<'TGeneric>` 将采用源中定义的符号名称，而不是在调用站点替换的类型的名称。

语法不同的原因在于与和等其他 F # 内部运算符对齐 `typeof<>` `typedefof<>` 。 这使 F # 对于作用于泛型类型的运算符和源中的任何其他内容都保持一致。

## <a name="nameof-in-pattern-matching"></a>模式匹配中的 Nameof

[ `nameof` 模式](pattern-matching.md#nameof-pattern)允许 `nameof` 在模式匹配表达式中使用，如下所示：

```fsharp
let f (str: string) =
    match str with
    | nameof str -> "It's 'str'!"
    | _ -> "It is not 'str'!"

f "str" // matches
f "asdf" // does not match
```
