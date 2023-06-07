---
title: 尝试 .。。Catch .。。Finally 语句
description: 了解如何对 Visual Basic Try/Catch/Finally 语句使用异常处理。
ms.date: 12/07/2018
f1_keywords:
- vb.Try...Catch...Finally
- vb.when
- vb.Finally
- vb.Catch
- vb.Try
helpviewer_keywords:
- Try...Catch...Finally statements
- Try statement [Visual Basic]
- try-catch exception handling, Try...Catch...Finally statements
- error handling, while running code
- Try statement [Visual Basic], Try...Catch...Finally
- Finally keyword [Visual Basic], Try...Catch...Finally
- Catch statement [Visual Basic]
- When keyword [Visual Basic]
- Visual Basic code, handling errors while running
- structured exception handling, Try...Catch...Finally statements
ms.assetid: d6488026-ccb3-42b8-a810-0d97b9d6472b
ms.openlocfilehash: 22f1611786a3da512632b5b547b7ef141c8f65c6
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84391763"
---
# <a name="trycatchfinally-statement-visual-basic"></a>Try...Catch...Finally 语句 (Visual Basic)

提供了一种方法，用于处理给定代码块中可能发生的一些或所有可能的错误，同时仍在运行代码。

## <a name="syntax"></a>语法

```vb
Try
    [ tryStatements ]
    [ Exit Try ]
[ Catch [ exception [ As type ] ] [ When expression ]
    [ catchStatements ]
    [ Exit Try ] ]
[ Catch ... ]
[ Finally
    [ finallyStatements ] ]
End Try
```

## <a name="parts"></a>组成部分

|术语|定义|
|---|---|
|`tryStatements`|可选。 可能发生错误的语句。 可以是复合语句。|
|`Catch`|可选。 允许多个 `Catch` 块。 如果在处理块时出现异常 `Try` ， `Catch` 则将以文本顺序检查每个语句，以确定它是否处理异常，并 `exception` 表示已引发的异常。|
|`exception`|可选。 任何变量名称。 `exception` 的初始值是引发的错误的值。 与一起使用 `Catch` 以指定捕获的错误。 如果省略，则该 `Catch` 语句将捕获任何异常。|
|`type`|可选。 指定类筛选器的类型。 如果的值 `exception` 是由 `type` 或派生类型指定的类型，则该标识符将绑定到异常对象。|
|`When`|可选。 `Catch`带有子句的语句 `When` 仅在计算结果为时才捕获异常 `expression` `True` 。 `When`子句仅在检查异常的类型后应用，并且 `expression` 可以引用表示异常的标识符。|
|`expression`|可选。 必须可隐式转换为 `Boolean` 。 描述泛型筛选器的任何表达式。 通常用于按错误号进行筛选。 与关键字一起使用 `When` ，以指定捕获错误的情况。|
|`catchStatements`|可选。 用于处理关联块中发生的错误的语句 `Try` 。 可以是复合语句。|
|`Exit Try`|可选。 用于中断结构的关键字 `Try...Catch...Finally` 。 继续执行语句后面的代码 `End Try` 。 `Finally`语句仍将被执行。 不允许在 `Finally` 块中使用。|
|`Finally`|可选。 `Finally`当执行离开语句的任何部分时，始终会执行块 `Try...Catch` 。|
|`finallyStatements`|可选。 在所有其他错误处理发生之后执行的语句。|
|`End Try`|终止 `Try...Catch...Finally` 结构。|

## <a name="remarks"></a>备注

如果预计在代码的特定部分中可能出现特定的异常，请将代码放在块中， `Try` 使用 `Catch` 块保留控件并处理异常（如果发生）。

`Try…Catch`语句由一个 `Try` 块后跟一个或多个 `Catch` 子句组成，这些子句指定不同异常的处理程序。 在块中引发异常时 `Try` ，Visual Basic 将查找 `Catch` 处理异常的语句。 如果找不到匹配的 `Catch` 语句，Visual Basic 将检查调用当前方法的方法，并在调用堆栈上向上进行。 如果未 `Catch` 找到任何块，则 Visual Basic 向用户显示未处理的异常消息，并停止执行程序。

您可以 `Catch` 在一个语句中使用多个语句 `Try…Catch` 。 如果这样做，子句的顺序很 `Catch` 重要，因为它们是按顺序进行检查的。 在使用更笼统的子句之前获取特定性更强的异常。

以下 `Catch` 语句条件是最不具体的条件，将捕获派生自类的所有异常 <xref:System.Exception> 。 在 `Catch` `Try...Catch...Finally` 捕获所需的所有特定异常后，通常应将这些变体中的一个作为结构的最后一个块。 控制流永远不会到达 `Catch` 遵循这些变化之一的块。

- `type`为 `Exception` ，例如：`Catch ex As Exception`

- 语句没有 `exception` 变量，例如：`Catch`

如果 `Try…Catch…Finally` 语句嵌套在另一个 `Try` 块中，Visual Basic 首先检查 `Catch` 最内层块中的每个语句 `Try` 。 如果未找到任何匹配 `Catch` 的语句，则搜索将继续对 `Catch` 外部块的语句执行 `Try…Catch…Finally` 。

块中的局部变量 `Try` 在块中不可用， `Catch` 因为它们是单独的块。 如果要在多个块中使用变量，请在结构外声明变量 `Try...Catch...Finally` 。

> [!TIP]
> `Try…Catch…Finally`语句作为 IntelliSense 代码段提供。 在 "代码片段管理器" 中，展开 "**代码模式"-如果为，则依次尝试 Catch、Property**等，然后进行**错误处理（异常）**。 有关详细信息，请参阅[代码片段](/visualstudio/ide/code-snippets)。

## <a name="finally-block"></a>Finally 块

如果有一个或多个必须在退出结构之前运行的语句 `Try` ，请使用 `Finally` 块。 控件在传递到 `Finally` 结构外之前传递到块 `Try…Catch` 。 即使结构内的任何位置发生异常，也是如此 `Try` 。

`Finally`块可用于运行任何必须执行的代码，即使存在异常也是如此。 `Finally`无论块退出的方式如何，都将控制传递到块 `Try...Catch` 。

`Finally`即使代码遇到 `Return` 或块中的语句，块中的代码也会 `Try` 运行 `Catch` 。 在以下情况下，控件不会从 `Try` 或 `Catch` 块传递到相应的 `Finally` 块：

- 在或块中遇到[结束语句](end-statement.md) `Try` `Catch` 。

- 在 <xref:System.StackOverflowException> 或块中引发 `Try` `Catch` 。

将执行显式传输到块中是无效的 `Finally` 。 `Finally`除了通过异常以外，将执行转移到块外是无效的。

如果 `Try` 语句不包含至少一个 `Catch` 块，则它必须包含一个 `Finally` 块。

> [!TIP]
> 如果不必捕获特定的异常，则该语句的 `Using` 行为类似于 `Try…Finally` 块，并保证资源的处置，而不管退出块的方式如何。 即使出现未经处理的异常也是如此。 有关详细信息，请参阅[Using 语句](using-statement.md)。

## <a name="exception-argument"></a>异常参数

`Catch`块 `exception` 参数是类的实例 <xref:System.Exception> ，或者是从类派生的类 `Exception` 。 `Exception`类实例对应于块中发生的错误 `Try` 。

对象的属性 `Exception` 可帮助确定异常的原因和位置。 例如，属性将 <xref:System.Exception.StackTrace%2A> 列出导致异常的被调用方法，从而帮助你查找代码中发生错误的位置。 <xref:System.Exception.Message%2A>返回描述异常的消息。 <xref:System.Exception.HelpLink%2A>返回指向关联帮助文件的链接。 <xref:System.Exception.InnerException%2A>返回 `Exception` 导致当前异常的对象; 或者，如果没有原始异常，则返回该对象 `Nothing` `Exception` 。

## <a name="considerations-when-using-a-trycatch-statement"></a>使用 Try ... 时的注意事项Catch 语句

只使用 `Try…Catch` 语句来表明出现异常或意外的程序事件。 原因如下：

- 在运行时捕获异常将产生额外的开销，并且可能比预检查更慢，以避免异常。

- 如果 `Catch` 未正确处理某个块，则可能不会正确向用户报告该异常。

- 异常处理使程序更复杂。

并非始终需要 `Try…Catch` 语句来检查可能出现的情况。 下面的示例检查文件是否存在，然后再尝试打开它。 这减少了捕获由方法引发的异常的需求 <xref:System.IO.File.OpenText%2A> 。

[!code-vb[VbVbalrStatements#94](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#94)]

确保块中的代码 `Catch` 可以正确地向用户报告异常，无论是通过线程安全日志记录还是适当的消息。 否则，例外可能仍然未知。

## <a name="async-methods"></a>异步方法

如果用[Async](../modifiers/async.md)修饰符标记方法，可以在方法中使用[Await](../operators/await-operator.md)运算符。 带有运算符的语句 `Await` 挂起了方法的执行，直到等待的任务完成。 任务表示正在进行的工作。 当与运算符关联的任务完成时 `Await` ，将在同一方法中继续执行。 有关详细信息，请参阅[异步程序中的控制流](../../programming-guide/concepts/async/control-flow-in-async-programs.md)。

异步方法返回的任务可能会以错误状态结束，这表示它由于未处理的异常而完成。 任务还可以以 "已取消" 状态结束，导致 `OperationCanceledException` 从 await 表达式引发。 若要捕获任一类型的异常，请将 `Await` 与任务关联的表达式放置在 `Try` 块中，并在块中捕获该异常 `Catch` 。 本主题后面提供了一个示例。

任务可能处于错误状态，因为多个异常负责错误的发生。 例如，任务可能是对 <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> 调用的结果。 当你等待此类任务时，捕获的异常只是异常之一，你无法预测将捕获的异常。 本主题后面提供了一个示例。

`Await`表达式不能位于 `Catch` 块或块中 `Finally` 。

## <a name="iterators"></a>迭代器

迭代器函数或 `Get` 访问器对集合执行自定义迭代。 迭代器使用[Yield](yield-statement.md)语句每次返回集合中的每个元素。 您可以通过[对每个 .。。下一语句](for-each-next-statement.md)。

`Yield`语句可以在 `Try` 块内。 `Try`包含语句的块 `Yield` 可以有 `Catch` 块，并且可以有 `Finally` 块。 有关示例，请参阅[迭代](../../programming-guide/concepts/iterators.md)器中的 "Try 块 Visual Basic" 部分。

`Yield`语句不能位于 `Catch` 块或 `Finally` 块中。

如果 `For Each` 主体（在迭代器函数之外）引发异常，则 `Catch` 不会执行迭代器函数中的块，但 `Finally` 会执行迭代器函数中的块。 `Catch`Iterator 函数内的块仅捕获 iterator 函数内发生的异常。

## <a name="partial-trust-situations"></a>部分信任情况

在部分信任的情况下（例如在网络共享上托管的应用程序），不 `Try...Catch...Finally` 会捕获在调用包含调用的方法之前出现的安全异常。 在以下示例中，当你将其放在服务器共享上并从该位置运行时，将产生错误 "SecurityException：请求失败"。 有关安全异常的详细信息，请参阅 <xref:System.Security.SecurityException> 类。

[!code-vb[VbVbalrStatements#85](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#85)]

在这种部分信任情况下，必须将 `Process.Start` 语句放在单独的中 `Sub` 。 对的初始调用 `Sub` 将失败。 这样，便可以 `Try...Catch` 在包含的开始之前捕获它 `Sub` `Process.Start` 并生成安全异常。

## <a name="examples"></a>示例

### <a name="the-structure-of-trycatchfinally"></a>Try ... 的结构Catch .。。最后

下面的示例阐释了语句的结构 `Try...Catch...Finally` 。

[!code-vb[VbVbalrStatements#86](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#86)]  

### <a name="exception-in-a-method-called-from-a-try-block"></a>从 Try 块调用的方法中的异常

在下面的示例中， `CreateException` 方法引发 `NullReferenceException` 。 生成异常的代码不在 `Try` 块中。 因此，该 `CreateException` 方法不会处理异常。 `RunSample`由于对方法的调用 `CreateException` 在块中，因此该方法会处理异常 `Try` 。

该示例包含 `Catch` 多个异常类型的语句，按从最具体到最常见的顺序排序。

[!code-vb[VbVbalrStatements#91](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#91)]

### <a name="the-catch-when-statement"></a>Catch When 语句

下面的示例演示如何使用 `Catch When` 语句根据条件表达式进行筛选。 如果条件表达式的计算结果为 `True` ，则块中的代码将 `Catch` 运行。

[!code-vb[VbVbalrStatements#92](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#92)]

### <a name="nested-try-statements"></a>嵌套 Try 语句

下面的示例包含 `Try…Catch` 包含在块中的语句 `Try` 。 内部 `Catch` 块引发了一个异常，该异常的 `InnerException` 属性设置为原始异常。 外部 `Catch` 块报告其自己的异常和内部异常。

[!code-vb[VbVbalrStatements#93](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#93)]

### <a name="exception-handling-for-async-methods"></a>异步方法的异常处理

下面的示例阐释异步方法的异常处理。 若要捕获适用于异步任务的异常，该 `Await` 表达式位于 `Try` 调用方的块中，而在块中捕获该异常 `Catch` 。

在示例中取消注释 `Throw New Exception` 行以演示异常处理。 异常捕获于 `Catch` 块中，任务的 `IsFaulted` 属性设置为 `True` ，任务的 `Exception.InnerException` 属性设置为异常。

取消注释 `Throw New OperationCancelledException` 行以演示在取消异步进程时发生的情况。 异常捕获到 `Catch` 块中，任务的 `IsCanceled` 属性设置为 `True` 。 但是，在某些不适用于此示例的情况下，将 `IsFaulted` 设置为 `True` ，并将 `IsCanceled` 设置为 `False` 。

[!code-vb[csAsyncExceptions#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csasyncexceptions/vb/class1.vb#1)]

### <a name="handling-multiple-exceptions-in-async-methods"></a>在异步方法中处理多个异常

下面的示例阐释了在多个任务可能导致多个异常的情况中的异常处理。 `Try`块具有返回的 `Await` 任务的表达式 <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> 。 当应用的三个任务完成时，任务完成 <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=nameWithType> 。

三个任务中的每一个都会导致异常。 `Catch`块循环访问异常，这些异常在返回的任务的 `Exception.InnerExceptions` 属性中找到 `Task.WhenAll` 。

[!code-vb[csAsyncExceptions#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/csasyncexceptions/vb/class1.vb#3)]

## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Information.Err%2A>
- <xref:System.Exception>
- [Exit 语句](exit-statement.md)
- [On Error 语句](on-error-statement.md)
- [使用代码片段的最佳做法](/visualstudio/ide/best-practices-for-using-code-snippets)
- [异常处理](../../../standard/parallel-programming/exception-handling-task-parallel-library.md)
- [Throw 语句](throw-statement.md)
