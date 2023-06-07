---
title: 启动多个异步任务并在其完成时进行处理
ms.date: 07/20/2015
ms.assetid: 57ffb748-af40-4794-bedd-bdb7fea062de
ms.openlocfilehash: 52e4d786667a70730f311ca20a30acd7108107e3
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91090364"
---
# <a name="start-multiple-async-tasks-and-process-them-as-they-complete-visual-basic"></a>启动多个异步任务并在其完成时进行处理 (Visual Basic)

通过使用 <xref:System.Threading.Tasks.Task.WhenAny%2A?displayProperty=nameWithType>，可以同时启动多个任务，并在它们完成时逐个对它们进行处理，而不是按照它们的启动顺序进行处理。  
  
 下面的示例使用查询来创建一组任务。 每个任务都下载指定网站的内容。 在对 while 循环的每次迭代中，对 `WhenAny` 的等待调用返回任务集合中首先完成下载的任务。 此任务从集合中删除并进行处理。 循环重复进行，直到集合中不包含任何任务。  
  
> [!NOTE]
> 若要运行示例，计算机上必须安装有 Visual Studio 2012 或更高版本和 .NET Framework 4.5 或更高版本。  
  
## <a name="downloading-the-example"></a>下载示例  

 若要下载完整的 Windows Presentation Foundation (WPF) 项目，请参阅 [Async Sample:Fine Tuning Your Application](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)（异步示例：微调应用程序）。  
  
1. 解压缩下载的文件，然后启动 Visual Studio。  
  
2. 在菜单栏上，依次选择 **“文件”** 、 **“打开”** 和 **“项目/解决方案”** 。  
  
3. 在“打开项目”**** 对话框中，打开保存已解压的示例代码的文件夹，然后打开 AsyncFineTuningVB 的解决方案 (.sln) 文件。  
  
4. 在“解决方案资源管理器”中，打开“ProcessTasksAsTheyFinish”项目的快捷菜单，选择“设为启动项目”。    
  
5. 选择 F5 键运行该项目。  
  
     选择 Ctrl+F5 键运行该项目，而不进行调试。  
  
6. 多次运行此项目以验证并不总是以相同顺序显示已下载的长度。  
  
 如果不想下载项目，可在本主题末尾处查看 MainWindow.xaml.vb 文件。  
  
## <a name="building-the-example"></a>生成示例  

 此示例将添加到在 [完成一个异步任务后取消剩余异步任务 ](cancel-remaining-async-tasks-after-one-is-complete.md) 中开发的代码 (Visual Basic) 并使用相同的 UI。  
  
 若要自行生成示例，请按“下载示例”部分的说明逐步操作，但选择“CancelAfterOneTask”**** 作为“启动项目”****。 将此主题中的更改添加到项目中的 `AccessTheWebAsync` 方法。 这些更改标有星号。  
  
 **CancelAfterOneTask** 项目已包含一个查询，执行此查询时，将创建任务集合。 在以下代码中，每次调用 `ProcessURLAsync` 都将返回一个 <xref:System.Threading.Tasks.Task%601>，其中 `TResult` 是整数。  
  
```vb  
Dim downloadTasksQuery As IEnumerable(Of Task(Of Integer)) =  
    From url In urlList Select ProcessURLAsync(url, client, ct)  
```  
  
 在项目的 Mainwindow.xaml 文件中，对方法进行以下更改 `AccessTheWebAsync` 。  
  
- 通过应用 <xref:System.Linq.Enumerable.ToList%2A?displayProperty=nameWithType> 而非 <xref:System.Linq.Enumerable.ToArray%2A> 执行查询。  
  
    ```vb  
    Dim downloadTasks As List(Of Task(Of Integer)) = downloadTasksQuery.ToList()  
    ```  
  
- 添加 while 循环，针对集合中的每个任务执行以下步骤。  
  
    1. 等待调用 `WhenAny`，以标识集合中首个完成下载的任务。  
  
        ```vb  
        Dim firstFinishedTask As Task(Of Integer) = Await Task.WhenAny(downloadTasks)  
        ```  
  
    2. 从集合中移除任务。  
  
        ```vb  
        downloadTasks.Remove(firstFinishedTask)  
        ```  
  
    3. 等待 `firstFinishedTask`，由对 `ProcessURLAsync` 的调用返回。 `firstFinishedTask` 变量是 <xref:System.Threading.Tasks.Task%601>，其中 `TReturn` 是整数。 任务已完成，但需等待它检索已下载网站的长度，如以下示例所示。  
  
        ```vb  
        Dim length = Await firstFinishedTask  
        resultsTextBox.Text &= String.Format(vbCrLf & "Length of the downloaded website:  {0}" & vbCrLf, length)  
        ```  
  
 应多次运行此项目以验证并不总是以相同顺序显示已下载的长度。  
  
> [!CAUTION]
> 如示例所示，可以在循环中使用 `WhenAny` 来解决涉及少量任务的问题。 但是，如果要处理大量任务，可以采用其他更高效的方法。 有关详细信息和示例，请参阅 [处理任务完成](https://devblogs.microsoft.com/pfxteam/processing-tasks-as-they-complete/)。  
  
## <a name="complete-example"></a>完整的示例  

 下列代码是示例的 MainWindow.xaml.vb 文件的完整文本。 对添加到此示例的元素进行了星号标记。  
  
 请注意，必须为 <xref:System.Net.Http> 添加引用。  
  
 可以从 [Async Sample:Fine Tuning Your Application](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)（异步示例：微调应用程序）下载这些项目。  
  
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
  
        ' ***Create a query that, when executed, returns a collection of tasks.  
        Dim downloadTasksQuery As IEnumerable(Of Task(Of Integer)) =  
            From url In urlList Select ProcessURLAsync(url, client, ct)  
  
        ' ***Use ToList to execute the query and start the download tasks.
        Dim downloadTasks As List(Of Task(Of Integer)) = downloadTasksQuery.ToList()  
  
        ' ***Add a loop to process the tasks one at a time until none remain.  
        While downloadTasks.Count > 0  
            ' ***Identify the first task that completes.  
            Dim firstFinishedTask As Task(Of Integer) = Await Task.WhenAny(downloadTasks)  
  
            ' ***Remove the selected task from the list so that you don't  
            ' process it more than once.  
            downloadTasks.Remove(firstFinishedTask)  
  
            ' ***Await the first completed task and display the results.  
            Dim length = Await firstFinishedTask  
            resultsTextBox.Text &= String.Format(vbCrLf & "Length of the downloaded website:  {0}" & vbCrLf, length)  
        End While  
  
    End Function  
  
    ' Bundle the processing steps for a website into one async method.  
    Async Function ProcessURLAsync(url As String, client As HttpClient, ct As CancellationToken) As Task(Of Integer)  
  
        ' GetAsync returns a Task(Of HttpResponseMessage).
        Dim response As HttpResponseMessage = Await client.GetAsync(url, ct)  
  
        ' Retrieve the website contents from the HttpResponseMessage.  
        Dim urlContents As Byte() = Await response.Content.ReadAsByteArrayAsync()  
  
        Return urlContents.Length  
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
  
' Length of the download:  226093  
' Length of the download:  412588  
' Length of the download:  175490  
' Length of the download:  204890  
' Length of the download:  158855  
' Length of the download:  145790  
' Length of the download:  44908  
' Downloads complete.  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Threading.Tasks.Task.WhenAny%2A>
- [微调异步应用程序 (Visual Basic)](fine-tuning-your-async-application.md)
- [使用 Async 和 Await 的异步编程 (Visual Basic)](index.md)
- [异步示例：微调应用程序](https://code.msdn.microsoft.com/Async-Fine-Tuning-Your-a676abea)
