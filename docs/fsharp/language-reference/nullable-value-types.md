---
title: 可以为 null 的值类型
description: '了解如何使用可以为 null 的值类型，这种方法表示在 F # 中也可以为 null 的值类型。'
ms.date: 11/19/2020
ms.openlocfilehash: e28cbfc57c5631573f46ac36462517cf011e96d2
ms.sourcegitcommit: 81f1bba2c97a67b5ca76bcc57b37333ffca60c7b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97009633"
---
# <a name="nullable-value-types"></a>可以为 null 的值类型

可以 _为 null 的值类型_ `Nullable<'T>` 表示也可以为的任何 [结构](structures.md) 类型 `null` 。 当与库和组件进行交互时，可能会选择将这些类型的类型（如整数）与值相结合，从而 `null` 提高效率。 支持此构造的基础类型是 <xref:System.Nullable%601?displayProperty=nameWithType> 。

## <a name="syntax"></a>语法

```fsharp
Nullable<'T>
Nullable value
```

## <a name="declare-and-assign-with-values"></a>用值声明和赋值

声明可为 null 的值类型，就像在 F # 中声明任何类似包装器的类型一样：

```fsharp
open System

let x = 12
let nullableX = Nullable<int> x
```

你还可以 elide 泛型类型参数，并允许类型推理解决该问题：

```fsharp
open System

let x = 12
let nullableX = Nullable x
```

若要分配给可以为 null 的值类型，还需要是显式的。 对于 F # 定义的可以为 null 的值类型，不存在隐式转换：

```fsharp
open System

let mutable x = Nullable 12
x <- Nullable 13
```

## <a name="assign-null"></a>赋值 null

不能直接分配 `null` 给可以为 null 的值类型。 `Nullable()`改用：

```fsharp
let mutable a = Nullable 42
a <- Nullable()
```

这是因为不 `Nullable<'T>` 具有 `null` 正确的值。

## <a name="pass-and-assign-to-members"></a>传递并分配给成员

使用成员与 F # 值之间的主要区别在于可以在使用成员时隐式推断可以为 null 的值类型。 请考虑以下方法，该方法采用可以为 null 的值类型作为输入：

```fsharp
type C() =
    member _.M(x: Nullable<int>) = x.HasValue
    member val NVT = Nullable 12 with get, set

let c = C()
c.M(12)
c.NVT <- 12
```

在上面的示例中，可以将传递 `12` 给方法 `M` 。 你还可以分配 `12` 给 auto 属性 `NVT` 。 如果可以将输入构造为可以为 null 的值类型并且它与目标类型匹配，则 F # 编译器将隐式转换此类调用或分配。

## <a name="examine-a-nullable-value-type-instance"></a>检查可以为 null 的值类型实例

与 [选项](options.md)不同，后者是表示可能值的通用构造，可以为 null 的值类型不与模式匹配一起使用。 相反，您需要使用 [`if`](conditional-expressions-if-then-else.md) 表达式并检查 `HasValue` 属性。

若要获取基础值，请在 `Value` 检查后使用属性 `HasValue` ，如下所示：

```fsharp
open System

let a = Nullable 42

if a.HasValue then
    printfn $"{a} is {a.Value}"
else
    printfn $"{a} has no value."
```

## <a name="nullable-operators"></a>可以为 null 的运算符

对可以为 null 的值类型（例如算术或比较）的操作，可能需要使用 [可以为 null 的运算符](symbol-and-operator-reference/nullable-operators.md)。

可以使用命名空间中的转换运算符从一个可为 null 的值类型转换为另一个值类型 `FSharp.Linq` ：

```fsharp
open System
open FSharp.Linq

let nullableInt = Nullable 10
let nullableFloat = Nullable.float nullableInt
```

你还可以使用适当的不可为 null 的运算符转换为基元类型，如果该类型没有值，则会产生异常：

```fsharp
open System
open FSharp.Linq

let nullableInt = Nullable 10
let nullableFloat = Nullable.float nullableInt

printfn $"value is %f{float nullableFloat}"
```

你还可以使用可以为 null 的运算符作为检查和的 `HasValue` 简短 `Value` 操作：

```fsharp
open System
open FSharp.Linq

let nullableInt = Nullable 10
let nullableFloat = Nullable.float nullableInt

let isBigger = nullableFloat ?> 1.0
let isBiggerLongForm = nullableFloat.HasValue && nullableFloat.Value > 1.0
```

`?>`如果左侧是可以为 null 的，则比较检查是否可以为 null，并且仅在其具有值时才会成功。 它与其后的行等效。

## <a name="see-also"></a>另请参阅

- [结构](structures.md)
- [F # 选项](options.md)
