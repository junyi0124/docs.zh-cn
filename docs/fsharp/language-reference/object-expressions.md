---
title: 对象表达式
description: '如果要避免创建新的命名类型所需的额外代码和开销，请了解如何使用 F # 对象表达式。'
ms.date: 02/08/2019
ms.openlocfilehash: 8a3e40b7833b551eefb95ec62b935acd1ba7b1f9
ms.sourcegitcommit: ecd9e9bb2225eb76f819722ea8b24988fe46f34c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/05/2020
ms.locfileid: "96740297"
---
# <a name="object-expressions"></a>对象表达式

*对象表达式* 是基于现有的基类型、接口或接口集创建动态创建的匿名对象类型的新实例的表达式。

## <a name="syntax"></a>语法

```fsharp
// When typename is a class:
{ new typename [type-params]arguments with
    member-definitions
    [ additional-interface-definitions ]
}
// When typename is not a class:
{ new typename [generic-type-args] with
    member-definitions
    [ additional-interface-definitions ]
}
```

## <a name="remarks"></a>备注

在前面的语法中， *typename* 表示现有的类类型或接口类型。 *类型-参数* 描述可选的泛型类型参数。 这些 *参数* 仅用于需要构造函数参数的类类型。 *成员定义* 是基类方法的重写，或基类或接口中抽象方法的实现。

下面的示例演示了几种不同类型的对象表达式。

```fsharp
// This object expression specifies a System.Object but overrides the
// ToString method.
let obj1 = { new System.Object() with member x.ToString() = "F#" }
printfn $"{obj1}"

// This object expression implements the IFormattable interface.
let delimiter(delim1: string, delim2: string, value: string) =
    { new System.IFormattable with
        member x.ToString(format: string, provider: System.IFormatProvider) =
            if format = "D" then
                delim1 + value + delim2
            else
                value }

let obj2 = delimiter("{","}", "Bananas!");

printfn "%A" (System.String.Format("{0:D}", obj2))

// Define two interfaces
type IFirst =
  abstract F : unit -> unit
  abstract G : unit -> unit

type ISecond =
  inherit IFirst
  abstract H : unit -> unit
  abstract J : unit -> unit

// This object expression implements both interfaces.
let implementer() =
    { new ISecond with
        member this.H() = ()
        member this.J() = ()
      interface IFirst with
        member this.F() = ()
        member this.G() = () }
```

## <a name="using-object-expressions"></a>使用对象表达式

如果要避免创建新的命名类型所需的额外代码和开销，请使用对象表达式。 如果使用对象表达式来最大程度地减少在程序中创建的类型的数目，则可以减少代码的行数，并防止不必要的类型激增。 您可以使用对象表达式自定义现有类型，也可以为手头的特定事例提供适当的接口实现，而不是仅创建用于处理特定情况的许多类型。

## <a name="see-also"></a>另请参阅

- [F# 语言参考](index.md)
