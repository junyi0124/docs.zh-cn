---
title: F# 4.7 - F# 指南中的新增功能
description: 获取 F# 4.7 中提供的新功能的概述。
ms.date: 11/27/2019
ms.openlocfilehash: 7a6e744a398719bcb55d168dd700459e0b122dd6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/12/2020
ms.locfileid: "79185876"
---
# <a name="whats-new-in-f-47"></a>F# 4.7 中的新增功能

F# 4.7 为 F# 语言添加了多项改进。

## <a name="get-started"></a>入门

F# 4.7 在所有 .NET 核心发行版和可视化工作室工具中均可用。 [开始使用 F#](../get-started/index.md)了解更多信息。

## <a name="language-version"></a>语言版本

F# 4.7 编译器引入了通过项目文件中的属性设置有效语言版本的能力：

```xml
<PropertyGroup>
    <LangVersion>preview</LangVersion>
</PropertyGroup>
```

可以将其`4.6`设置为 值 、`4.7``latest`和`preview`。 默认为 `latest`。

如果将其设置为`preview`，编译器将激活编译器中实现的所有 F# 预览功能。

## <a name="implicit-yields"></a>隐式收益

不再需要在`yield`可以推断类型的数组、列表、序列或计算表达式中应用关键字。 在下面的示例中，两个表达式都需要 F# 4.7 之前每个条目的`yield`语句：

```fsharp
let s = seq { 1; 2; 3; 4; 5 }

let daysOfWeek includeWeekend =
    [
        "Monday"
        "Tuesday"
        "Wednesday"
        "Thursday"
        "Friday"
        if includeWeekend then
            "Saturday"
            "Sunday"
    ]
```

如果引入单个`yield`关键字，则所有其他项也必须已`yield`应用于该关键字。

在表达式中使用时，隐式收益不会激活，该表达式`yield!`还用于执行诸如拼平序列之类的操作。 在这些情况下，今天必须继续`yield`使用。

## <a name="wildcard-identifiers"></a>通配符标识符

在涉及类的 F# 代码中，自标识符必须始终在成员声明中显式。 但是，在从未使用自标识符的情况下，传统上使用双下划线来指示无名的自标识符是惯例。 现在可以使用单个下划线：

```fsharp
type C() =
    member _.M() = ()
```

这也适用于`for`循环：

```fsharp
for _ in 1..10 do printfn "Hello!"
```

## <a name="indentation-relaxations"></a>缩进松弛

在 F# 4.7 之前，主构造函数和静态成员参数的缩进要求要求过高的缩进。 它们现在只需要一个缩进范围：

```fsharp
type OffsideCheck(a:int,
    b:int, c:int,
    d:int) = class end

type C() =
    static member M(a:int,
        b:int, c:int,
        d:int) = 1
```
