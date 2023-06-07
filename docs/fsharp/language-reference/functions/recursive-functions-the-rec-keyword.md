---
title: 递归函数：rec 关键字
description: '了解如何使用 F # "rec" 关键字来定义递归函数。'
ms.date: 08/12/2020
ms.openlocfilehash: 1ab00ff9400129e531fd7320861b3d9625cad08c
ms.sourcegitcommit: b4a46f6d7ebf44c0035627d00924164bcae2db30
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/29/2020
ms.locfileid: "91438080"
---
# <a name="recursive-functions-the-rec-keyword"></a>递归函数：rec 关键字

`rec`关键字与关键字一起用于 `let` 定义递归函数。

## <a name="syntax"></a>语法

```fsharp
// Recursive function:
let rec function-nameparameter-list =
    function-body

// Mutually recursive functions:
let rec function1-nameparameter-list =
    function1-body

and function2-nameparameter-list =
    function2-body
...
```

## <a name="remarks"></a>备注

递归函数-调用自身的函数-在 F # 语言中显式标识为 `rec` 关键字。 `rec`关键字可使绑定的名称 `let` 在其主体中可用。

下面的示例演示一个使用数学定义计算第*n*<sup>th</sup>个斐波那契数的递归函数。

```fsharp
let rec fib n =
    match n with
    | 0 | 1 -> 1
    | n -> fib (n-1) + fib (n-2)
```

> [!NOTE]
> 实际上，与上一示例类似的代码并不理想，因为它 unecessarily 已计算的重新计算值。 这是因为它不是尾递归，本文对此进行了进一步说明。

方法在定义它们的类型内隐式递归，这意味着无需添加 `rec` 关键字。 例如：

```fsharp
type MyClass() =
    member this.Fib(n) =
        match n with
        | 0 | 1 -> 1
        | n -> this.Fib(n-1) + this.Fib(n-2)
```

不过，类中的 Let 绑定不隐式递归。 所有 `let` 绑定函数都需要 `rec` 关键字。

## <a name="tail-recursion"></a>结尾递归

对于某些递归函数，必须将更多 "纯" 定义重构为 [尾递归](https://cs.stackexchange.com/questions/6230/what-is-tail-recursion)。 这可以防止不必要的 recomputations。 例如，可以按如下所示重写上一个波那契数字生成器：

```fsharp
let fib n =
    let rec loop acc1 acc2 n =
        match n with
        | 0 -> acc1
        | 1 -> acc2
        | _ ->
            loop acc2 (acc1 + acc2) (n - 1)
    loop 0 1 n
```

这是一个更复杂的实现。 生成波那契数字是 "简单" 算法的一个很好的示例，它在数学上纯粹简单，但实际上是低效的。 在 F # 中，有几个方面使其效率更高，同时仍以递归方式定义：

* 名为的递归内部函数 `loop` ，它是一个惯用 F # 模式。
* 将累积值传递到递归调用的两个累加器参数。
* 对的值 `n` 进行检查以返回特定的累计。

如果此示例是用循环迭代编写的，则在满足特定条件之前，代码将类似于两个不同的值累积号。

这是结尾递归的原因是因为递归调用不需要在调用堆栈上保存任何值。 所有计算的中间值都将通过输入累积到内部函数。 这也允许 F # 编译器优化代码，就像编写了像循环一样快 `while` 。

如前面的示例所示，编写以递归方式处理与内部和外部函数有关的内容的 F # 代码是很常见的。 内部函数使用尾递归，而外部函数具有更好的调用方接口。

## <a name="mutually-recursive-functions"></a>相互递归函数

有时，函数是 *相互递归*的，这意味着，调用会形成一个圆圈，其中一个函数调用另一个函数，而后者又调用第一个，并在之间进行任意数量的调用。 必须在一个绑定中一起定义此类函数 `let` ，使用 `and` 关键字将它们链接在一起。

下面的示例演示两个相互递归函数。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet4002.fs)]

## <a name="recursive-values"></a>递归值

你还可以将 `let` 绑定值定义为 recursive。 这对于日志记录有时是如此。 通过 F # 5 和 `nameof` 函数，你可以执行以下操作：

```fsharp
let rec nameDoubles = nameof nameDoubles + nameof nameDoubles
```

## <a name="see-also"></a>请参阅

- [函数](index.md)
