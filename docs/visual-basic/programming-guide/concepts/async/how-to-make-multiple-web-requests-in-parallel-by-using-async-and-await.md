---
title: 如何：使用 Async 和 Await 并行发出多个 Web 请求
ms.date: 07/20/2015
ms.assetid: a894b99b-7cfd-4a38-adfb-20d24f986730
ms.openlocfilehash: 40bab392af94ba941c2562e885a8d2e08aeea5b9
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396579"
---
# <a name="how-to-make-multiple-web-requests-in-parallel-by-using-async-and-await-visual-basic"></a>如何：使用 Async 和 Await 并行发出多个 Web 请求 (Visual Basic)

在 async 方法中，任务在创建后即启动。 [Await](../../../language-reference/operators/await-operator.md)运算符应用于方法中的点，在该方法中，处理无法继续，直到任务完成。 通常任务被创建后即等待，如下面的示例所示。

```vb
Dim result = Await someWebAccessMethodAsync(url)
```

但是，如果程序有其他不依赖于任务的完成的工作要完成，则可以将创建任务和等待任务分开。

```vb
' The following line creates and starts the task.
Dim myTask = someWebAccessMethodAsync(url)

' While the task is running, you can do other work that does not depend
' on the results of the task.
' . . . . .

' The application of Await suspends the rest of this method until the task is
' complete.
Dim result = Await myTask
```

在启动任务和等待任务之间，可以启动其他任务。 其他任务以并行方式隐式运行，但不会创建其他线程。

下面的程序启动三个异步 Web 下载任务，然后按照任务的调用顺序等待其完成。 请注意，运行此程序时，任务并不总是按照创建和等待它们的顺序完成。 任务在创建后开始运行，在此方法到达 await 表达式之前可能已完成一个或多个任务。

> [!NOTE]
> 若要完成此项目，计算机上必须安装有 Visual Studio 2012 或更高版本和 .NET Framework 4.5 或更高版本。

对于同时启动多个任务的另一个示例，请参阅[如何：使用 System.threading.tasks.task.whenall 扩展 Async 演练（Visual Basic）](how-to-extend-the-async-walkthrough-by-using-task-whenall.md)。

可以从[开发人员代码示例](https://code.msdn.microsoft.com/Async-Make-Multiple-Web-49adb82e)下载此示例的代码。

### <a name="to-set-up-the-project"></a>设置项目

1. 若要设置 WPF 应用程序，请完成以下步骤。 可以在[演练：使用 Async 和 Await 访问 Web （Visual Basic）](walkthrough-accessing-the-web-by-using-async-and-await.md)中找到有关这些步骤的详细说明。

    - 创建包含一个文本框和一个按钮的 WPF 应用程序。 将按钮命名为 `startButton`，将文本框命名为 `resultsTextBox`。

    - 对 <xref:System.Net.Http> 添加引用。

    - 在 Mainwindow.xaml 文件中， `Imports` 为添加语句 `System.Net.Http` 。

### <a name="to-add-the-code"></a>添加代码

1. 在 "设计" 窗口中，双击 "Mainwindow.xaml" 按钮以 `startButton_Click` 在 mainwindow.xaml 中创建事件处理程序。

2. 复制以下代码，并将其粘贴到 `startButton_Click` mainwindow.xaml 中的正文中。

    ```vb
    resultsTextBox.Clear()
    Await CreateMultipleTasksAsync()
    resultsTextBox.Text &= vbCrLf & "Control returned to button1_Click."
    ```

     此代码调用异步方法 `CreateMultipleTasksAsync`，此方法驱动应用程序。

3. 向项目中添加以下支持方法：

    - `ProcessURLAsync` 使用 <xref:System.Net.Http.HttpClient> 方法将网站内容下载为字节数组。 支持方法 `ProcessURLAsync` 随后显示并返回数组的长度。

    - `DisplayResults` 显示每个 URL 的字节数组中的字节数。 当所有任务完成下载后显示。

     复制以下方法，并将其粘贴到 `startButton_Click` mainwindow.xaml 中的事件处理程序之后。

    ```vb
    Private Async Function ProcessURLAsync(url As String, client As HttpClient) As Task(Of Integer)

        Dim byteArray = Await client.GetByteArrayAsync(url)
        DisplayResults(url, byteArray)
        Return byteArray.Length
    End Function

    Private Sub DisplayResults(url As String, content As Byte())

        ' Display the length of each website. The string format
        ' is designed to be used with a monospaced font, such as
        ' Lucida Console or Global Monospace.
        Dim bytes = content.Length
        ' Strip off the "https://".
        Dim displayURL = url.Replace("https://", "")
        resultsTextBox.Text &= String.Format(vbCrLf & "{0,-58} {1,8}", displayURL, bytes)
    End Sub
    ```

4. 最后，定义方法 `CreateMultipleTasksAsync`，用于执行以下步骤。

    - 该方法声明 `HttpClient` 对象，这需要你访问 `ProcessURLAsync` 中的 <xref:System.Net.Http.HttpClient.GetByteArrayAsync%2A> 方法。

    - 此方法创建并启动三个类型为 <xref:System.Threading.Tasks.Task%601> 的任务，其中 `TResult` 是一个整数。 每个任务完成后，`DisplayResults` 显示任务的 URL 和下载内容的长度。 由于任务是异步运行的，因此显示结果的顺序可能与声明任务的顺序不同。

    - 此方法等待每个任务完成。 每个 `Await` 运算符暂停执行 `CreateMultipleTasksAsync`，直到所等待的任务完成。 此运算符还会从每个已完成的任务的 `ProcessURLAsync` 调用中检索返回值。

    - 当任务已完成并已检索到整数值时，此方法对网站的长度求和，并显示结果。

     复制下面的方法，并将其粘贴到你的解决方案。

    ```vb
    Private Async Function CreateMultipleTasksAsync() As Task

        ' Declare an HttpClient object, and increase the buffer size. The
        ' default buffer size is 65,536.
        Dim client As HttpClient =
            New HttpClient() With {.MaxResponseContentBufferSize = 1000000}

        ' Create and start the tasks. As each task finishes, DisplayResults
        ' displays its length.
        Dim download1 As Task(Of Integer) =
            ProcessURLAsync("https://msdn.microsoft.com", client)
        Dim download2 As Task(Of Integer) =
            ProcessURLAsync("https://msdn.microsoft.com/library/hh156528(VS.110).aspx", client)
        Dim download3 As Task(Of Integer) =
            ProcessURLAsync("https://msdn.microsoft.com/library/67w7t67f.aspx", client)

        ' Await each task.
        Dim length1 As Integer = Await download1
        Dim length2 As Integer = Await download2
        Dim length3 As Integer = Await download3

        Dim total As Integer = length1 + length2 + length3

        ' Display the total count for all of the websites.
        resultsTextBox.Text &= String.Format(vbCrLf & vbCrLf &
                                             "Total bytes returned:  {0}" & vbCrLf, total)
    End Function
    ```

5. 按 F5 键以运行程序，然后选择 **“启动”** 按钮。

     多次运行此程序以确认三个任务并不总是以相同的顺序完成，并且完成的顺序不一定是创建和等待任务的顺序。

## <a name="example"></a>示例

下面的代码包括完整的示例。

```vb
' Add the following Imports statements, and add a reference for System.Net.Http.
Imports System.Net.Http

Class MainWindow

    Async Sub startButton_Click(sender As Object, e As RoutedEventArgs) Handles startButton.Click
        resultsTextBox.Clear()
        Await CreateMultipleTasksAsync()
        resultsTextBox.Text &= vbCrLf & "Control returned to button1_Click."
    End Sub

    Private Async Function CreateMultipleTasksAsync() As Task

        ' Declare an HttpClient object, and increase the buffer size. The
        ' default buffer size is 65,536.
        Dim client As HttpClient =
            New HttpClient() With {.MaxResponseContentBufferSize = 1000000}

        ' Create and start the tasks. As each task finishes, DisplayResults
        ' displays its length.
        Dim download1 As Task(Of Integer) =
            ProcessURLAsync("https://msdn.microsoft.com", client)
        Dim download2 As Task(Of Integer) =
            ProcessURLAsync("https://msdn.microsoft.com/library/hh156528(VS.110).aspx", client)
        Dim download3 As Task(Of Integer) =
            ProcessURLAsync("https://msdn.microsoft.com/library/67w7t67f.aspx", client)

        ' Await each task.
        Dim length1 As Integer = Await download1
        Dim length2 As Integer = Await download2
        Dim length3 As Integer = Await download3

        Dim total As Integer = length1 + length2 + length3

        ' Display the total count for all of the websites.
        resultsTextBox.Text &= String.Format(vbCrLf & vbCrLf &
                                             "Total bytes returned:  {0}" & vbCrLf, total)
    End Function

    Private Async Function ProcessURLAsync(url As String, client As HttpClient) As Task(Of Integer)

        Dim byteArray = Await client.GetByteArrayAsync(url)
        DisplayResults(url, byteArray)
        Return byteArray.Length
    End Function

    Private Sub DisplayResults(url As String, content As Byte())

        ' Display the length of each website. The string format
        ' is designed to be used with a monospaced font, such as
        ' Lucida Console or Global Monospace.
        Dim bytes = content.Length
        ' Strip off the "https://".
        Dim displayURL = url.Replace("https://", "")
        resultsTextBox.Text &= String.Format(vbCrLf & "{0,-58} {1,8}", displayURL, bytes)
    End Sub
End Class
```

## <a name="see-also"></a>另请参阅

- [演练：使用 Async 和 Await 访问 Web (Visual Basic)](walkthrough-accessing-the-web-by-using-async-and-await.md)
- [使用 Async 和 Await 的异步编程 (Visual Basic)](index.md)
- [如何：使用 Task.WhenAll 扩展异步演练 (Visual Basic)](how-to-extend-the-async-walkthrough-by-using-task-whenall.md)
