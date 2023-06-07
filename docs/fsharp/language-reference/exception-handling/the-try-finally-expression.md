---
title: 异常：try...finally 表达式
description: 了解F# "try ...finally 表达式使你可以执行清理代码，即使代码块引发异常也是如此。
ms.date: 05/16/2016
ms.openlocfilehash: 0ddb64ac13b307404864ec5b54f26fd8a7a3d7d8
ms.sourcegitcommit: a2d0e1f66367367065bc8dc0dde488ab536da73f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082997"
---
# <a name="exceptions-the-tryfinally-expression"></a>异常：try...finally 表达式

`try...finally`表达式使你可以执行清理代码，即使代码块引发异常也是如此。

## <a name="syntax"></a>语法

```fsharp
try
    expression1
finally
    expression2
```

## <a name="remarks"></a>备注

表达式可用于执行前面*语法中的*表达式2中的代码，而不管是否在执行表达式2的过程中生成了异常。 `try...finally`

*表达式*2 的类型不影响整个表达式的值;异常不发生时返回的类型是*表达式*= 值中的最后一个值。 发生异常时，不会返回任何值，并且在调用堆栈上，控制流传输到下一个匹配的异常处理程序。 如果未找到异常处理程序，程序将终止。 在执行匹配处理程序中的代码或程序终止之前，执行`finally`分支中的代码。

下面的代码演示如何使用`try...finally`表达式。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5701.fs)]

控制台的输出如下所示。

```console
Closing stream
Exception handled.
```

正如您从输出中看到的那样，流在外部异常处理前关闭，并且该文件`test.txt`包含文本，该文本`test1`指示缓冲区已刷新并写入磁盘（即使传输的异常除外）对外部异常处理程序的控制。

请注意， `try...with`构造是一个独立`try...finally`于构造的构造。 因此，如果代码需要`with`块`finally`和块，则必须嵌套两个构造，如下面的代码示例中所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5702.fs)]

在计算表达式（包括序列表达式和异步工作流）的上下文中，**尝试 ...inally**表达式可以具有自定义实现。 有关详细信息，请参阅[计算表达式](../computation-expressions.md)。

## <a name="see-also"></a>请参阅

- [异常处理](index.md)
- [异常：`try...with`表达式](the-try-with-expression.md)
