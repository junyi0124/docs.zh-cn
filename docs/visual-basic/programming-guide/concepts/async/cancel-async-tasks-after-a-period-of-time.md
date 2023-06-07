---
title: 一段时间后取消异步任务
ms.date: 07/20/2015
ms.assetid: a48045a3-6a99-42af-b824-af340f0b9a5d
ms.openlocfilehash: 048d4c19d459905ea579ede96c69230e718d55aa
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396683"
---
# <a name="cancel-async-tasks-after-a-period-of-time-visual-basic"></a>在一段时间后取消异步任务 (Visual Basic)

如果不希望等待操作结束，可使用 <xref:System.Threading.CancellationTokenSource.CancelAfter%2A?displayProperty=nameWithType> 方法在一段时间后取消异步操作。 此方法会计划取消未在 `CancelAfter` 表达式指定的时间段内完成的任何关联任务。

此示例添加到[取消异步任务或任务列表 (Visual Basic)](cancel-an-async-task-or-a-list-of-tasks.md) 中开发的代码，以下载网站列表并显示每个网站的内容长度。

> [!NOTE]
> 若要运行示例，计算机上必须安装有 Visual Studio 2012 或更高版本和 .NET Framework 4.5 或更高版本 。

## <a name="downloading-the-example"></a>下载示例

若要下载完整的 Windows Presentation Foundation (WPF) 项目，请参阅 [Async Sample: Fine Tuning Your Application](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)（异步示例：微调应用程序），然后遵循以下步骤。

1. 解压缩下载的文件，然后启动 Visual Studio。

2. 在菜单栏上，依次选择 **“文件”** 、 **“打开”** 和 **“项目/解决方案”** 。

3. 在“打开项目”**** 对话框中，打开保存已解压的示例代码的文件夹，然后打开 AsyncFineTuningVB 的解决方案 (.sln) 文件。

4. 在“解决方案资源管理器”  中，打开“CancelAfterTime”  项目的快捷菜单，然后选择“设为启动项目”  。

5. 选择 F5 键运行该项目。

     选择 Ctrl+F5 键运行该项目，而不进行调试。

6. 多次运行程序以验证输出是否显示所有网站的输出、不显示网站的输出或显示某些网站的输出。

 如果不想下载项目，可在本主题末尾处查看 MainWindow.xaml.vb 文件。

## <a name="building-the-example"></a>生成示例

本主题中的示例添加到[取消异步任务或任务列表 (Visual Basic)](cancel-an-async-task-or-a-list-of-tasks.md) 中开发的项目，以取消任务列表。 该示例使用相同的 UI，但未显示使用“取消”  按钮。

若要自行生成示例，请按“下载示例”部分的说明逐步操作，选择“CancelAListOfTasks”  作为“启动项目”  。 将此主题中的更改添加到该项目。

若要指定将任务标记为取消之前的最长时间，请将对 `CancelAfter` 的调用添加到 `startButton_Click`，如以下示例所示。 新增内容标有星号。

```vb
Private Async Sub startButton_Click(sender As Object, e As RoutedEventArgs)

    ' Instantiate the CancellationTokenSource.
    cts = New CancellationTokenSource()

    resultsTextBox.Clear()

    Try
        ' ***Set up the CancellationTokenSource to cancel after 2.5 seconds. (You
        ' can adjust the time.)
        cts.CancelAfter(2500)

        Await AccessTheWebAsync(cts.Token)
        resultsTextBox.Text &= vbCrLf & "Downloads complete."

    Catch ex As OperationCanceledException
        resultsTextBox.Text &= vbCrLf & "Downloads canceled." & vbCrLf

    Catch ex As Exception
        resultsTextBox.Text &= vbCrLf & "Downloads failed." & vbCrLf
    End Try

    ' Set the CancellationTokenSource to Nothing when the download is complete.
    cts = Nothing
End Sub
```

多次运行程序以验证输出是否显示所有网站的输出、不显示网站的输出或显示某些网站的输出。 下面的输出是一个示例：

```console
Length of the downloaded string: 35990.

Length of the downloaded string: 407399.

Length of the downloaded string: 226091.

Downloads canceled.
```

## <a name="complete-example"></a>完整示例

下列代码是示例的 MainWindow.xaml.vb 文件的完整文本。 对添加到此示例的元素进行了星号标记。

请注意，必须为 <xref:System.Net.Http> 添加引用。

可以从 [Async Sample: Fine Tuning Your Application](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)（异步示例：微调应用程序）下载这些项目。

```vb
' Add an Imports directive and a reference for System.Net.Http.
Imports System.Net.Http

' Add the following Imports directive for System.Threading.
Imports System.Threading

Class MainWindow

    ' Declare a System.Threading.CancellationTokenSource.
    Dim cts As CancellationTokenSource

    Private Async Sub startButton_Click(sender As Object, e As RoutedEventArgs)

        ' Instantiate the CancellationTokenSource.
        cts = New CancellationTokenSource()

        resultsTextBox.Clear()

        Try
            ' ***Set up the CancellationTokenSource to cancel after 2.5 seconds. (You
            ' can adjust the time.)
            cts.CancelAfter(2500)

            Await AccessTheWebAsync(cts.Token)
            resultsTextBox.Text &= vbCrLf & "Downloads complete."

        Catch ex As OperationCanceledException
            resultsTextBox.Text &= vbCrLf & "Downloads canceled." & vbCrLf

        Catch ex As Exception
            resultsTextBox.Text &= vbCrLf & "Downloads failed." & vbCrLf
        End Try

        ' Set the CancellationTokenSource to Nothing when the download is complete.
        cts = Nothing
    End Sub

    ' You can still include a Cancel button if you want to.
    Private Sub cancelButton_Click(sender As Object, e As RoutedEventArgs)

        If cts IsNot Nothing Then
            cts.Cancel()
        End If
    End Sub

    ' Provide a parameter for the CancellationToken.
    ' Change the return type to Task because the method has no return statement.
    Async Function AccessTheWebAsync(ct As CancellationToken) As Task

        Dim client As HttpClient = New HttpClient()

        ' Call SetUpURLList to make a list of web addresses.
        Dim urlList As List(Of String) = SetUpURLList()

        ' Process each element in the list of web addresses.
        For Each url In urlList
            ' GetAsync returns a Task(Of HttpResponseMessage).
            ' Argument ct carries the message if the Cancel button is chosen.
            ' Note that the Cancel button can cancel all remaining downloads.
            Dim response As HttpResponseMessage = Await client.GetAsync(url, ct)

            ' Retrieve the website contents from the HttpResponseMessage.
            Dim urlContents As Byte() = Await response.Content.ReadAsByteArrayAsync()

            resultsTextBox.Text &=
                vbCrLf & $"Length of the downloaded string: {urlContents.Length}." & vbCrLf
        Next
    End Function

    ' Add a method that creates a list of web addresses.
    Private Function SetUpURLList() As List(Of String)

        Dim urls = New List(Of String) From
            {
                "https://msdn.microsoft.com",
                "https://msdn.microsoft.com/library/hh290138.aspx",
                "https://msdn.microsoft.com/library/hh290140.aspx",
                "https://msdn.microsoft.com/library/dd470362.aspx",
                "https://msdn.microsoft.com/library/aa578028.aspx",
                "https://msdn.microsoft.com/library/ms404677.aspx",
                "https://msdn.microsoft.com/library/ff730837.aspx"
            }
        Return urls
    End Function

End Class

' Sample output:

' Length of the downloaded string: 35990.

' Length of the downloaded string: 407399.

' Length of the downloaded string: 226091.

' Downloads canceled.
```

## <a name="see-also"></a>另请参阅

- [使用 Async 和 Await 的异步编程 (Visual Basic)](index.md)
- [演练：使用 Async 和 Await 访问 Web (Visual Basic)](walkthrough-accessing-the-web-by-using-async-and-await.md)
- [取消一个异步任务或一组任务 (Visual Basic)](cancel-an-async-task-or-a-list-of-tasks.md)
- [微调异步应用程序 (Visual Basic)](fine-tuning-your-async-application.md)
- [Async Sample: Fine Tuning Your Application](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)（异步示例：微调应用程序）
