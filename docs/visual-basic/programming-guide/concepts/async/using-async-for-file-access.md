---
title: 使用 Async 以进行文件访问
ms.date: 07/20/2015
ms.assetid: c989305f-08e3-4687-95c3-948465cda202
ms.openlocfilehash: 2e7fa4a78363a08f2ff25e6a961868e85994e200
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91077353"
---
# <a name="using-async-for-file-access-visual-basic"></a>使用异步进行文件访问 (Visual Basic)

可以使用 Async 功能访问文件。 通过使用异步功能，你可以调用异步方法而无需使用回调，也不需要跨多个方法或 Lambda 表达式来拆分代码。 若要使同步代码异步，只需调用异步方法而非同步方法，并向代码中添加几个关键字。  
  
 可能出于以下原因向文件访问调用中添加异步：  
  
- 异步使 UI 应用程序响应速度更快，因为启动该操作的 UI 线程可以执行其他操作。 如果 UI 线程必须执行耗时较长的代码（例如超过 50 毫秒），UI 可能会冻结，直到 I/O 完成，此时 UI 线程可以再次处理键盘和鼠标输入及其他事件。  
  
- 异步可减少对线程的需要，进而提高 ASP.NET 和其他基于服务器的应用程序的可伸缩性。 如果应用程序对每次响应都使用专用线程，同时处理 1000 个请求时，则需要 1000 个线程。 异步操作在等待期间通常不需要使用线程。 异步操作仅需在结束时短暂使用现有 I/O 完成线程。  
  
- 当前条件下，文件访问操作的延迟可能非常低，但以后可能大幅增加。 例如，文件可能会移动到覆盖全球的服务器。  
  
- 使用异步功能所增加的开销很小。  
  
- 异步任务可以轻松地并行运行。  
  
## <a name="running-the-examples"></a>运行示例  

 若要运行本主题中的示例，可创建“WPF 应用程序”或“Windows 窗体应用程序”，然后添加一个“按钮”。 在按钮的 `Click` 事件中，添加对每个示例的第一个方法的调用。  
  
 在下面的示例中，包括以下 `Imports` 语句。  
  
```vb  
Imports System  
Imports System.Collections.Generic  
Imports System.Diagnostics  
Imports System.IO  
Imports System.Text  
Imports System.Threading.Tasks  
```  
  
## <a name="use-of-the-filestream-class"></a>使用 FileStream 类  

 本主题中的示例使用 <xref:System.IO.FileStream> 类，该类包含可导致在操作系统级别出现异步 I/O 的选项。 使用此选项可避免在许多情况下阻止 ThreadPool 线程。 若要启用此选项，可在构造函数调用中指定 `useAsync=true` 或 `options=FileOptions.Asynchronous` 参数。  
  
 如果通过指定文件路径直接打开 <xref:System.IO.StreamReader> 和 <xref:System.IO.StreamWriter>，则无法将此选项与这二者配合使用。 但是，如果为二者提供已由 <xref:System.IO.FileStream> 类打开的 <xref:System.IO.Stream>，则可以使用此选项。 请注意，即使 ThreadPool 线程受到阻止，UI 应用中的异步调用仍然更快，因为 UI 线程在等待期间不会受到阻止。  
  
## <a name="writing-text"></a>编写文本  

 下面的示例将文本写入文件。 在每个 await 语句中，该方法会立即退出。 文件 I/O 完成后，该方法将在 await 语句后面的语句中继续。 请注意，async 修饰符在使用 await 语句的方法的定义中。  
  
```vb  
Public Async Sub ProcessWrite()  
    Dim filePath = "temp2.txt"  
    Dim text = "Hello World" & ControlChars.CrLf  
  
    Await WriteTextAsync(filePath, text)  
End Sub  
  
Private Async Function WriteTextAsync(filePath As String, text As String) As Task  
    Dim encodedText As Byte() = Encoding.Unicode.GetBytes(text)  
  
    Using sourceStream As New FileStream(filePath,  
        FileMode.Append, FileAccess.Write, FileShare.None,  
        bufferSize:=4096, useAsync:=True)  
  
        Await sourceStream.WriteAsync(encodedText, 0, encodedText.Length)  
    End Using  
End Function  
```  
  
 原始示例包含 `Await sourceStream.WriteAsync(encodedText, 0, encodedText.Length)` 语句，它是下面两个语句的缩写式：  
  
```vb  
Dim theTask As Task = sourceStream.WriteAsync(encodedText, 0, encodedText.Length)  
Await theTask  
```  
  
 第一条语句返回任务，并会导致文件处理启动。 具有 await 的第二条语句将使方法立即退出并返回一个不同的任务。 文件处理稍后完成后，执行将返回到 await 后面的语句中。 有关详细信息，请参阅  [异步程序中的控制流 (Visual Basic) ](control-flow-in-async-programs.md)。  
  
## <a name="reading-text"></a>读取文本  

 下面的示例读取文件中的文本。 将会缓冲文本，并且在此情况下，会将其放入 <xref:System.Text.StringBuilder>。 与前一示例不同，await 的计算将生成一个值。 <xref:System.IO.Stream.ReadAsync%2A> 方法返回 <xref:System.Threading.Tasks.Task>\<<xref:System.Int32>>，因此在操作完成后 await 的评估会得出 `Int32` 值 (`numRead`)。 有关详细信息，请参阅 [异步返回类型 (Visual Basic) ](async-return-types.md)。  
  
```vb  
Public Async Sub ProcessRead()  
    Dim filePath = "temp2.txt"  
  
    If File.Exists(filePath) = False Then  
        Debug.WriteLine("file not found: " & filePath)  
    Else  
        Try  
            Dim text As String = Await ReadTextAsync(filePath)  
            Debug.WriteLine(text)  
        Catch ex As Exception  
            Debug.WriteLine(ex.Message)  
        End Try  
    End If  
End Sub  
  
Private Async Function ReadTextAsync(filePath As String) As Task(Of String)  
  
    Using sourceStream As New FileStream(filePath,  
        FileMode.Open, FileAccess.Read, FileShare.Read,  
        bufferSize:=4096, useAsync:=True)  
  
        Dim sb As New StringBuilder  
  
        Dim buffer As Byte() = New Byte(&H1000) {}  
        Dim numRead As Integer  
        numRead = Await sourceStream.ReadAsync(buffer, 0, buffer.Length)  
        While numRead <> 0  
            Dim text As String = Encoding.Unicode.GetString(buffer, 0, numRead)  
            sb.Append(text)  
  
            numRead = Await sourceStream.ReadAsync(buffer, 0, buffer.Length)  
        End While  
  
        Return sb.ToString  
    End Using  
End Function  
```  
  
## <a name="parallel-asynchronous-io"></a>并行异步 I/O  

 下面的示例通过编写 10 个文本文件来演示并行处理。 对于每个文件，<xref:System.IO.Stream.WriteAsync%2A> 方法将返回一个任务，此任务随后将添加到任务列表中。 `Await Task.WhenAll(tasks)` 语句将退出该方法，并在所有任务的文件处理完成时在此方法中继续。  
  
 该示例将在任务完成后关闭 `Finally` 块中的所有 <xref:System.IO.FileStream> 实例。 如果每个 `FileStream` 均已在 `Imports` 语句中创建，则可能在任务完成前释放 `FileStream`。  
  
 请注意，性能提升几乎完全来自并行处理而不是异步处理。 异步的优点在于它不会占用多个线程，也不会占用用户界面线程。  
  
```vb  
Public Async Sub ProcessWriteMult()  
    Dim folder = "tempfolder\"  
    Dim tasks As New List(Of Task)  
    Dim sourceStreams As New List(Of FileStream)  
  
    Try  
        For index = 1 To 10  
            Dim text = "In file " & index.ToString & ControlChars.CrLf  
  
            Dim fileName = "thefile" & index.ToString("00") & ".txt"  
            Dim filePath = folder & fileName  
  
            Dim encodedText As Byte() = Encoding.Unicode.GetBytes(text)  
  
            Dim sourceStream As New FileStream(filePath,  
                FileMode.Append, FileAccess.Write, FileShare.None,  
                bufferSize:=4096, useAsync:=True)  
  
            Dim theTask As Task = sourceStream.WriteAsync(encodedText, 0, encodedText.Length)  
            sourceStreams.Add(sourceStream)  
  
            tasks.Add(theTask)  
        Next  
  
        Await Task.WhenAll(tasks)  
    Finally  
        For Each sourceStream As FileStream In sourceStreams  
            sourceStream.Close()  
        Next  
    End Try  
End Sub  
```  
  
 当使用 <xref:System.IO.Stream.WriteAsync%2A> 和 <xref:System.IO.Stream.ReadAsync%2A> 方法时，可以指定可用于取消操作中间流的 <xref:System.Threading.CancellationToken>。 有关详细信息，请参阅[在托管线程中](../../../../standard/threading/cancellation-in-managed-threads.md) [ (Visual Basic) 和取消来微调异步应用程序](fine-tuning-your-async-application.md)。  
  
## <a name="see-also"></a>请参阅

- [使用 Async 和 Await 的异步编程 (Visual Basic)](index.md)
- [异步返回类型 (Visual Basic)](async-return-types.md)
- [异步程序中的控制流 (Visual Basic)](control-flow-in-async-programs.md)
