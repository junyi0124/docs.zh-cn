---
title: 异常：raise 函数
description: 了解如何使用F# "raise" 函数指示发生了错误或异常情况。
ms.date: 05/16/2016
ms.openlocfilehash: e0cc8da8310203c537b8081af8a225671bd8c6a3
ms.sourcegitcommit: f20dd18dbcf2275513281f5d9ad7ece6a62644b4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/30/2019
ms.locfileid: "68630286"
---
# <a name="exceptions-the-raise-function"></a>异常：raise 函数

`raise`函数用于指示发生了错误或异常情况。 有关错误的信息在异常对象中捕获。

## <a name="syntax"></a>语法

```fsharp
raise (expression)
```

## <a name="remarks"></a>备注

`raise`函数生成异常对象并启动堆栈展开过程。 堆栈展开进程由公共语言运行时 (CLR) 管理, 因此此进程的行为与任何其他 .NET 语言中的行为相同。 堆栈展开过程是一个搜索与生成的异常匹配的异常处理程序。 搜索从当前`try...with`表达式开始 (如果有)。 将按顺序检查`with`块中的每个模式。 当找到匹配的异常处理程序时, 该异常将被视为已处理;否则, 将展开堆栈并`with`检查调用链, 直到找到匹配的处理程序。 如果`finally`堆栈展开, 则在调用链中遇到的任何块也将按顺序执行。

函数等效于或C++中的C# `throw` `raise` 在`reraise` catch 处理程序中使用以在调用链中向上传播相同的异常。

下面的代码示例演示了如何使用`raise`函数生成异常。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5801.fs)]

`raise`函数还可用于引发 .net 异常, 如下面的示例中所示。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet5802.fs)]

## <a name="see-also"></a>请参阅

- [异常处理](index.md)
- [异常类型](exception-types.md)
- [异常：`try...with`表达式](the-try-with-expression.md)
- [异常：`try...finally`表达式](the-try-finally-expression.md)
- [异常：`failwith`函数](the-failwith-function.md)
- [异常：`invalidArg`函数](the-invalidArg-function.md)
