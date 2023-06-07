---
title: 异步
ms.date: 07/20/2015
f1_keywords:
- vb.Async
helpviewer_keywords:
- Async [Visual Basic]
- Async keyword [Visual Basic]
ms.assetid: 1be8b4b5-9689-41b5-bd33-b906bfd53bc5
ms.openlocfilehash: 35df7a464937647c6d110142a3e2801cebbea505
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84373149"
---
# <a name="async-visual-basic"></a>Async (Visual Basic)

`Async`修饰符指示它修改的方法或[lambda 表达式](../../programming-guide/language-features/procedures/lambda-expressions.md)是异步的。 此类方法称为*异步方法*。

异步方法提供了一种简便方式来完成可能需要长时间运行的工作，而不必阻止调用方的线程。 异步方法的调用方可以恢复其工作，而无需等待异步方法完成。

> [!NOTE]
> `Async` 和 `Await` 关键字是在 Visual Studio 2012 中引入的。 有关异步编程的介绍，请参阅[使用 async 和 Await 进行异步编程](../../programming-guide/concepts/async/index.md)。

下面的示例显示一个异步方法的结构。 按照约定，异步方法名的结尾为“Async”。

```vb
Public Async Function ExampleMethodAsync() As Task(Of Integer)
    ' . . .

    ' At the Await expression, execution in this method is suspended and,
    ' if AwaitedProcessAsync has not already finished, control returns
    ' to the caller of ExampleMethodAsync. When the awaited task is
    ' completed, this method resumes execution.
    Dim exampleInt As Integer = Await AwaitedProcessAsync()

    ' . . .

    ' The return statement completes the task. Any method that is
    ' awaiting ExampleMethodAsync can now get the integer result.
    Return exampleInt
End Function
```

通常，由关键字修改的方法 `Async` 包含至少一个[Await](async.md)表达式或语句。 方法同步运行，直至到达第一个 `Await`，此时暂停，直到等待的任务完成。 同时，控制权返回给方法的调用方。 如果该方法不包含 `Await` 表达式或语句，则该方法不会被挂起并作为同步方法执行。 编译器警告将通知你不包含的任何异步方法， `Await` 因为该情况可能表示存在错误。 有关详细信息，请参阅[编译器错误](../error-messages/bc42358.md)。

`Async` 关键字是一个非保留的关键字。 在修饰方法或 lambda 表达式时，它是关键字。 在所有其他上下文中，都会将其解释为标识符。

## <a name="return-types"></a>返回类型

异步方法可以是[Sub](../../programming-guide/language-features/procedures/sub-procedures.md)过程，也可以是返回类型为或的[函数](../../programming-guide/language-features/procedures/function-procedures.md)过程 <xref:System.Threading.Tasks.Task> <xref:System.Threading.Tasks.Task%601> 。 方法不能声明任何[ByRef](byref.md)参数。

`Task(Of TResult)`如果方法的[返回](../statements/return-statement.md)语句具有类型为 TResult 的操作数，则为异步方法的返回类型指定。 如果当方法完成时未返回有意义的值，则应使用 `Task`。 即对方法的调用返回 `Task`，但 `Task` 完成时，等待 `Await` 的任何 `Task` 语句不会产生结果值。

异步子例程主要用于定义需要 `Sub` 程序的事件处理程序。 异步子程序的调用方不能等待它，并且无法捕获该方法引发的异常。

有关详细信息和示例，请参阅[异步返回类型](../../programming-guide/concepts/async/async-return-types.md)。

## <a name="example"></a>示例

下面的示例显示一个异步事件处理程序、一个异步 lambda 表达式和一个异步方法。 有关使用这些元素的完整示例，请参阅[演练：使用 Async 和 Await 访问 Web](../../programming-guide/concepts/async/walkthrough-accessing-the-web-by-using-async-and-await.md)。 可从[开发人员代码示例](https://code.msdn.microsoft.com/Async-Sample-Accessing-the-9c10497f)下载演练代码。

```vb
' An event handler must be a Sub procedure.
Async Sub button1_Click(sender As Object, e As RoutedEventArgs) Handles button1.Click
    textBox1.Clear()
    ' SumPageSizesAsync is a method that returns a Task.
    Await SumPageSizesAsync()
    textBox1.Text = vbCrLf & "Control returned to button1_Click."
End Sub

' The following async lambda expression creates an equivalent anonymous
' event handler.
AddHandler button1.Click, Async Sub(sender, e)
                              textBox1.Clear()
                              ' SumPageSizesAsync is a method that returns a Task.
                              Await SumPageSizesAsync()
                              textBox1.Text = vbCrLf & "Control returned to button1_Click."
                          End Sub

' The following async method returns a Task(Of T).
' A typical call awaits the Byte array result:
'      Dim result As Byte() = Await GetURLContents("https://msdn.com")
Private Async Function GetURLContentsAsync(url As String) As Task(Of Byte())

    ' The downloaded resource ends up in the variable named content.
    Dim content = New MemoryStream()

    ' Initialize an HttpWebRequest for the current URL.
    Dim webReq = CType(WebRequest.Create(url), HttpWebRequest)

    ' Send the request to the Internet resource and wait for
    ' the response.
    Using response As WebResponse = Await webReq.GetResponseAsync()
        ' Get the data stream that is associated with the specified URL.
        Using responseStream As Stream = response.GetResponseStream()
            ' Read the bytes in responseStream and copy them to content.
            ' CopyToAsync returns a Task, not a Task<T>.
            Await responseStream.CopyToAsync(content)
        End Using
    End Using

    ' Return the result as a byte array.
    Return content.ToArray()
End Function
```

## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.CompilerServices.AsyncStateMachineAttribute>
- [Await 运算符](../operators/await-operator.md)
- [采用 Async 和 Await 的异步编程](../../programming-guide/concepts/async/index.md)
- [演练：使用 Async 和 Await 访问 Web](../../programming-guide/concepts/async/walkthrough-accessing-the-web-by-using-async-and-await.md)
