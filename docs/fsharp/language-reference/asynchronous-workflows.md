---
title: 异步工作流
description: '了解有关 F # 编程语言的支持，用于以异步方式执行计算，而无需阻塞其他工作的执行。'
ms.date: 08/15/2020
ms.openlocfilehash: 14146cc8a643f31831475075212cc06da5f8d6ff
ms.sourcegitcommit: fe8877e564deb68d77fa4b79f55584ac8d7e8997
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/17/2020
ms.locfileid: "90720265"
---
# <a name="asynchronous-workflows"></a>异步工作流

本文介绍 F # 中的支持异步执行计算，即，不阻止执行其他工作。 例如，异步计算可用于编写应用程序，这些应用程序具有在应用程序执行其他工作时对用户保持响应的 Ui。

## <a name="syntax"></a>语法

```fsharp
async { expression }
```

## <a name="remarks"></a>备注

在前面的语法中，由表示的计算 `expression` 设置为异步运行，也就是说，在执行异步睡眠操作、i/o 和其他异步操作时，不会阻止当前计算线程。 异步计算通常在后台线程上启动，而在当前线程上继续执行。 表达式的类型为 `Async<'T>` ，其中 `'T` 是使用关键字时表达式返回的类型 `return` 。 此类表达式中的代码称为 *异步块*或 *异步块*。

异步编程的方式有多种， [`Async`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-fsharpasync.html) 类提供的方法支持多种方案。 一般方法是创建 `Async` 表示要异步运行的计算或计算的对象，然后使用其中一个触发函数启动这些计算。 各种触发函数提供不同的方法来运行异步计算，使用哪种方法取决于你是要使用当前线程、后台线程还是 .NET Framework 任务对象，以及在计算完成时是否应运行延续函数。 例如，若要在当前线程上启动异步计算，可以使用 [`Async.StartImmediate`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-fsharpasync.html#StartImmediate) 。 当从 UI 线程启动异步计算时，您不会阻止用于处理用户操作（如击键和鼠标活动）的主事件循环，因此您的应用程序将保持响应。

## <a name="asynchronous-binding-by-using-let"></a>使用 let 进行异步绑定！

在异步工作流中，某些表达式和操作是同步的，而有些则是用于以异步方式返回结果的更长的计算。 以异步方式调用方法，而不是使用普通 `let` 绑定 `let!` 。 的作用 `let!` 是在执行计算时允许在其他计算或线程上继续执行。 绑定的右侧 `let!` 返回后，异步工作流的其余部分将继续执行。

下面的代码演示了和之间的差异 `let` `let!` 。 使用的代码行 `let` 只是将异步计算创建为一个对象，您可以在以后使用来运行该对象，例如 `Async.StartImmediate` 或 [`Async.RunSynchronously`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-fsharpasync.html#RunSynchronously) 。 使用的代码行 `let!` 开始计算，然后在结果可用之前挂起线程，此时将继续执行。

```fsharp
// let just stores the result as an asynchronous operation.
let (result1 : Async<byte[]>) = stream.AsyncRead(bufferSize)
// let! completes the asynchronous operation and returns the data.
let! (result2 : byte[])  = stream.AsyncRead(bufferSize)
```

除了之外 `let!` ，还可以使用 `use!` 来执行异步绑定。 与之间的差异与和的差异 `let!` `use!` 相同 `let` `use` 。 对于 `use!` ，在当前范围结束时释放对象。 请注意，在当前版本的 F # 语言中，不 `use!` 允许将值初始化为 null，即使是这样 `use` 。

## <a name="asynchronous-primitives"></a>异步基元

执行单个异步任务并返回结果的方法称为 *异步基元*，它们专用于与一起使用 `let!` 。 F # 核心库中定义了多个异步基元。 本模块中定义了两种 Web 应用程序方法 [`FSharp.Control.WebExtensions`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-webextensions.html) ： [`WebRequest.AsyncGetResponse`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-webextensions.html#AsyncGetResponse) 和 [`WebClient.AsyncDownloadString`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-webextensions.html#AsyncDownloadString) 。 在给定 URL 的情况下，这两个基元都从网页中下载数据。 `AsyncGetResponse` 生成一个 `System.Net.WebResponse` 对象，并 `AsyncDownloadString` 生成一个字符串，该字符串表示网页的 HTML。

模块中包含了用于异步 i/o 操作的几个基元 [`FSharp.Control.CommonExtensions`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-commonextensions.html) 。 类的这些扩展方法 `System.IO.Stream` 为 [`Stream.AsyncRead`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-commonextensions.html#AsyncRead) 和 [`Stream.AsyncWrite`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-commonextensions.html#AsyncWrite) 。

还可以通过定义一个函数，该函数的完整体包含在异步块中来编写自己的异步基元。

若要在使用 F # 异步编程模型为其他异步模型设计的 .NET Framework 中使用异步方法，您需要创建一个返回 F # 对象的函数 `Async` 。 F # 库的功能使此操作变得简单。

此处包含使用异步工作流的一个示例;对于 [异步类](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-fsharpasync.html)的方法，文档中还有许多其他内容。

此示例演示如何使用异步工作流并行执行计算。

在下面的代码示例中，函数将 `fetchAsync` 获取从 Web 请求返回的 HTML 文本。 `fetchAsync`函数包含一个异步代码块。 在此示例中，如果对异步基元的结果进行了绑定， [`AsyncDownloadString`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-webextensions.html#AsyncDownloadString) `let!` 则使用而不是 `let` 。

使用函数 [`Async.RunSynchronously`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-fsharpasync.html#RunSynchronously) 可执行异步操作，并等待其结果。 例如，你可以通过将 [`Async.Parallel`](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-fsharpasync.html#Parallel) 函数与函数结合使用来并行执行多个异步操作 `Async.RunSynchronously` 。 `Async.Parallel`函数获取对象的列表 `Async` ，为每个 `Async` 任务对象设置要并行运行的代码，并返回一个 `Async` 表示并行计算的对象。 与单个操作一样，调用 `Async.RunSynchronously` 开始执行。

`runAll`函数并行启动三个异步工作流，并等待它们全部完成。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet8003.fs)]

## <a name="see-also"></a>另请参阅

- [F# 语言参考](index.md)
- [计算表达式](computation-expressions.md)
- [Control Async 类](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-control-fsharpasync.html)
