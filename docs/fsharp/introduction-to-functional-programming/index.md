---
title: F# 中的函数编程简介
description: 了解 F# 中的函数编程的基础知识。
ms.date: 10/29/2018
ms.openlocfilehash: 44242a4319a331312a003a555d1483f2a3f1a90d
ms.sourcegitcommit: 9b877e160c326577e8aa5ead22a937110d80fa44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/11/2020
ms.locfileid: "97110580"
---
# <a name="introduction-to-functional-programming-in-f"></a>F\# 中的函数编程简介

函数编程是一种编程方式，这种方式强调函数和不可变数据的使用。 类型化函数编程是指将函数编程与静态类型（如 F#）结合使用。 通常，函数编程中强调了以下概念：

* 函数作为你使用的主构造
* 表达式，而不是语句
* 不可变值优于变量
* 声明性编程优于命令式编程

在此系列中，你将了解使用 F# 的函数编程中的概念和模式。 在此过程中，你也将了解一些 F#。

## <a name="terminology"></a>术语

函数编程与其他编程范例一样附带词汇，你最终需要学习该词汇。 下面是你将看到的一些常见术语：

* 函数 - 函数是在给定输入时将生成输出的构造。 更正式地说，它将项从一个集映射到另一个集。 这种形式通过许多方式变得具体，尤其是在使用对数据集合进行操作的函数时。 它是函数编程中最基本（且最重要）的概念。
* 表达式 - 表达式是代码中生成值的构造。 在 F# 中，此值必须绑定或被显式忽略。 表达式可以完全替换为函数调用。
* 纯度 - 纯度是函数的属性，以致相同参数的返回值总是相同的，并且其计算没有任何副作用。 纯函数完全取决于其参数。
* 引用透明度 - 引用透明度是表达式的属性，以便可以不影响程序行为的情况下将表达式替换为其输出。
* 不可变性 - 不可变性表示值不能就地更改。 这与变量不同，后者可以就地更改。

## <a name="examples"></a>示例

以下示例说明了这些核心概念。

### <a name="functions"></a>函数

函数编程中最常见且最基本的构造是函数。 下面是向整数添加 1 的简单函数：

```fsharp
let addOne x = x + 1
```

其类型签名如下所示：

```fsharp
val addOne: x:int -> int
```

签名可以显示为“`addOne` 接受名为 `x` 的 `int` 并生成 `int`”。 更正式地说，`addOne` 将值从一个整数集映射到另一个整数集。 `->` 令牌表示此映射。 在 F# 中，通常可以查看函数签名以了解它的作用。

那么，为什么签名非常重要？ 在类型化函数编程中，函数的实现通常没有实际类型签名那么重要！ `addOne` 向整数添加值 1 的事实在运行时很有趣，但当你构造程序时，接受并返回 `int` 的事实说明了你实际使用此函数的方式。 此外，一旦正确使用此函数（在类型签名方面），诊断任何问题就只能在 `addOne` 函数的主体中完成。 这是类型化函数编程的推动力。

### <a name="expressions"></a>表达式

表达式是计算结果为值的构造。 与执行操作的语句不同，表达式可以被视为执行返回值的操作。 表达式几乎始终用在函数编程中而不是语句中。

请考虑上一个函数 `addOne`。 `addOne` 的主体是一个表达式：

```fsharp
// 'x + 1' is an expression!
let addOne x = x + 1
```

这是此表达式的结果，用于定义 `addOne` 函数的结果类型。 例如，构成此函数的表达式可以更改为其他类型，例如 `string`：

```fsharp
let addOne x = x.ToString() + "1"
```

函数的签名现在：

```fsharp
val addOne: x:'a -> string
```

由于 F# 中的任何类型都可以对其调用 `ToString()`，因此 `x` 的类型已变为泛型（称为[自动泛化](../language-reference/generics/automatic-generalization.md)），并且结果类型为 `string`。

表达式不仅仅是函数的主体。 可以使用表达式来生成在其他位置使用的值。 一个常见表达式为 `if`：

```fsharp
// Checks if 'x' is odd by using the mod operator
let isOdd x = x % 2 <> 0

let addOneIfOdd input =
    let result =
        if isOdd input then
            input + 1
        else
            input

    result
```

`if` 表达式将生成一个名为 `result` 的值。 请注意，你可以完全省略 `result`，使 `if` 表达式成为 `addOneIfOdd` 函数的主体。 需要记住的重要一点是，表达式会生成值。

有一种特殊类型 `unit`，它在没有返回任何内容时使用。 例如，考虑此简单函数：

```fsharp
let printString (str: string) =
    printfn $"String is: {str}"
```

签名如下所示：

```fsharp
val printString: str:string -> unit
```

`unit` 类型指示没有返回实际值。 这在尽管工作没有返回任何值，但通常必须“工作”的情况下非常有用。

这与命令式编程形成鲜明对比，其中等效的 `if` 构造是一个语句，生成值通常是使用转变变量来完成的。 例如，在 C# 中，代码可能如下所示：

```csharp
bool IsOdd(int x) => x % 2 != 0;

int AddOneIfOdd(int input)
{
    var result = input;

    if (IsOdd(input))
    {
        result = input + 1;
    }

    return result;
}
```

值得注意的是，C# 和其他 C 样式语言都支持[三元表达式](../../csharp/language-reference/operators/conditional-operator.md)，以进行基于表达式的条件编程。

在函数编程中，很少使用语句转变值。 尽管某些函数语言支持语句和转变，但在函数编程中使用这些概念并不常见。

### <a name="pure-functions"></a>纯函数

如前文所述，纯函数是满足以下情况的函数：

* 相同输入的计算结果始终为相同的值。
* 没有任何副作用。

在此上下文中考虑数学函数非常有用。 在数学中，函数仅取决于其参数，并且没有任何副作用。 在数学函数 `f(x) = x + 1` 中，`f(x)` 的值仅取决于 `x` 的值。 函数编程中的纯函数是相同的。

编写纯函数时，该函数必须仅取决于其参数，并且不会执行导致副作用的任何操作。

下面是非纯函数的一个示例，因为它取决于可变的全局状态：

```fsharp
let mutable value = 1

let addOneToValue x = x + value
```

`addOneToValue` 函数明显是非纯函数，因为可以随时更改 `value`，使其值不同于 1。 应避免在函数编程中使用取决于全局值的模式。

下面是非纯函数的另一个示例，因为它产生副作用：

```fsharp
let addOneToValue x =
    printfn $"x is %d{x}"
    x + 1
```

尽管此函数不取决于全局值，但它会将 `x` 的值写入程序的输出。 尽管这样做在本质上没有错误，但意味着函数是非纯函数。 如果程序的其他部分取决于程序的外部内容（如输出缓冲区），则调用此函数可能会影响程序的其他部分。

删除 `printfn` 语句会使函数变纯：

```fsharp
let addOneToValue x = x + 1
```

尽管此函数在本质上比不上带有 `printfn` 语句的先前版本，但它保证了此函数确实会返回值。 调用此函数任意次数都会产生相同的结果：仅生成值。 纯度提供的可预测性是许多函数程序员寻求的目标。

### <a name="immutability"></a>不可变性

最后，类型化函数编程的最基本概念之一是不可变性。 在 F# 中，所有值在默认情况下都是不可变的。 这意味着，除非显式将它们标记为可变，否则无法就地转变它们。

实际上，使用不可变值是指将编程方式从“我需要更改某些内容”更改为“我需要生成新值”。

例如，向某个值添加 1 意味着生成值，而不是转变现有值：

```fsharp
let value = 1
let secondValue = value + 1
```

在 F# 中，以下代码不会转变 `value` 函数；而是执行相等性检查：

```fsharp
let value = 1
value = value + 1 // Produces a 'bool' value!
```

一些函数编程语言根本不支持转变。 在 F# 中，它是受支持的，但它不是值的默认行为。

此概念甚至进一步扩展到数据结构。 在函数编程中，不可变的数据结构（如集等）的实现方式不同于最初预期的实现。 从概念上讲，将项添加到集的操作不会更改集，而会生成添加了值的新集。 在这种情况下，这通常是通过不同的数据结构来完成的，该结构允许有效地跟踪值，从而可以为数据提供适当的表示形式。

这种使用值和数据结构的方式非常重要，因为它强制你将任何修改操作视为创建新版本。 这样，相等性和可比性等情况在程序中保持一致。

## <a name="next-steps"></a>后续步骤

下一部分将全面介绍函数，探索在函数编程中使用它们的不同方式。

[第一类函数](first-class-functions.md)深入探讨函数，演示如何在各种上下文中使用它们。

## <a name="further-reading"></a>延伸阅读

[功能性思考](https://fsharpforfunandprofit.com/posts/thinking-functionally-intro/)系列是另一个有用的资源，可了解使用 F# 的函数编程。 它以一种实用且易于读取的方式介绍函数编程的基础知识，使用 F# 功能来说明这些概念。
