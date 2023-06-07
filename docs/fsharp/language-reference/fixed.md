---
title: Fixed 关键字
description: '了解如何在堆栈上 "固定" 本地以防止使用 F # "fixed" 关键字收集。'
ms.date: 08/15/2020
ms.openlocfilehash: b4b0d1ae101d5f7b65bff80fa070c9fd54de8d66
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96740349"
---
# <a name="the-fixed-keyword"></a>Fixed 关键字

`fixed`关键字允许你将本地固定到堆栈上，以防止在垃圾回收过程中收集或移动该堆栈。  它用于低级编程方案。

## <a name="syntax"></a>语法

```fsharp
use ptr = fixed expression
```

## <a name="remarks"></a>备注

这将扩展表达式的语法，以允许提取指针，并将其绑定到在垃圾回收期间阻止收集或移动的名称。  

通过将关键字通过关键字绑定到标识符，可以通过将表达式中的指针固定 `fixed` `use` 。  此的语义类似于通过关键字进行的资源管理 `use` 。  如果指针在范围内，则将其固定，一旦超出范围，它就不再是固定的。  `fixed` 不能在绑定的上下文之外使用 `use` 。  必须使用将指针绑定到名称 `use` 。

的使用 `fixed` 必须出现在函数或方法的表达式中。  它不能在脚本级或模块级范围内使用。

与所有指针代码一样，这是一项不安全的功能，在使用时将发出警告。

## <a name="example"></a>示例

```fsharp
open Microsoft.FSharp.NativeInterop

type Point = { mutable X: int; mutable Y: int}

let squareWithPointer (p: nativeptr<int>) =
    // Dereference the pointer at the 0th address.
    let mutable value = NativePtr.get p 0

    // Perform some work
    value <- value * value

    // Set the value in the pointer at the 0th address.
    NativePtr.set p 0 value

let pnt = { X = 1; Y = 2 }
printfn $"pnt before - X: %d{pnt.X} Y: %d{pnt.Y}" // prints 1 and 2

// Note that the use of 'fixed' is inside a function.
// You cannot fix a pointer at a script-level or module-level scope.
let doPointerWork() =
    use ptr = fixed &pnt.Y

    // Square the Y value
    squareWithPointer ptr
    printfn $"pnt after - X: %d{pnt.X} Y: %d{pnt.Y}" // prints 1 and 4

doPointerWork()
```

## <a name="see-also"></a>请参阅

- [NativePtr 模块](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-nativeinterop-nativeptrmodule.html)
