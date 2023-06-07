---
title: 处理异步应用程序中的可重入性
ms.date: 07/20/2015
ms.assetid: ef3dc73d-13fb-4c5f-a686-6b84148bbffe
ms.openlocfilehash: 2d1af14016f82b5de875d3638b132e14c7d2280d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396631"
---
# <a name="handling-reentrancy-in-async-apps-visual-basic"></a>在异步应用程序中处理重入 (Visual Basic)

在应用中包含异步代码时，应考虑并且可以阻止重新进入（指在异步操作完成之前重新进入它）。 如果不识别并处理重新进入的可能性，则它可能会导致意外结果。

> [!NOTE]
> 若要运行该示例，计算机上必须安装 Visual Studio 2012 或更高版本和 .NET Framework 4.5 或更高版本。

> [!NOTE]
> 传输层安全性 (TLS) 版本 1.2 现在是在应用开发中使用的最低版本。 如果应用面向低于 4.7 的 .NET Framework 版本，请参阅以下文章，了解 [.NET Framework 传输层安全性 (TLS) 的最佳做法](../../../../framework/network-programming/tls.md)。

## <a name="recognizing-reentrancy"></a><a name="BKMK_RecognizingReentrancy"></a>识别重新进入

在本主题中的示例中，用户选择“开始”  按钮以启动一个异步应用，该应用下载一系列网站并计算下载的总字节数。 该示例的同步版本以相同方式进行响应（无论用户选择该按钮多少次），因为在第一次选择之后，UI 线程会忽略这些事件，直到应用完成运行。 但是，在异步应用中，UI 线程会继续响应，你可能会在它完成之前重新进入异步操作。

下面的示例显示用户仅选择“开始”  按钮一次时的预期输出。 下载网站的列表会出现，其中包含每个站点的大小（以字节为单位）。 总字节数会在结尾处显示。

```console
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               117152
5. msdn.microsoft.com/library/hh524395.aspx                68959
6. msdn.microsoft.com/library/ms404677.aspx               197325
7. msdn.microsoft.com                                            42972
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591
```

但是，如果用户多次选择按钮，则会反复调用事件处理程序，并且每次都会重新进入下载进程。 因此，会有多个异步操作同时运行，输出会使结果交错，而总字节数令人困惑。

```console
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               117152
5. msdn.microsoft.com/library/hh524395.aspx                68959
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
6. msdn.microsoft.com/library/ms404677.aspx               197325
3. msdn.microsoft.com/library/jj155761.aspx                29019
7. msdn.microsoft.com                                            42972
4. msdn.microsoft.com/library/hh290140.aspx               117152
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591

5. msdn.microsoft.com/library/hh524395.aspx                68959
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
6. msdn.microsoft.com/library/ms404677.aspx               197325
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               117152
7. msdn.microsoft.com                                            42972
5. msdn.microsoft.com/library/hh524395.aspx                68959
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591

6. msdn.microsoft.com/library/ms404677.aspx               197325
7. msdn.microsoft.com                                            42972
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591
```

可以滚动到本主题末尾来评审生成此输出的代码。 可以通过将解决方案下载到本地计算机，然后运行 WebsiteDownload 项目，或是通过使用本主题末尾的代码创建自己的项目，来体验该代码。有关详细信息和说明，请参阅[检查并运行示例应用](#BKMD_SettingUpTheExample)。

## <a name="handling-reentrancy"></a><a name="BKMK_HandlingReentrancy"></a>处理重新进入

可以采用各种方式处理重新进入，具体取决于希望应用执行的操作。 本主题展示了以下示例：

- [禁用“开始”按钮](#BKMK_DisableTheStartButton)

  在操作运行期间禁用“开始”  按钮，以便用户无法中断它。

- [取消和重启操作](#BKMK_CancelAndRestart)

  当用户再次选择“开始”  按钮时取消仍在运行的任何操作，然后让最近请求的操作继续运行。

- [运行多个操作并将输出排入队列](#BKMK_RunMultipleOperations)

  允许所有请求的操作异步运行，但是会协调输出的显示，以便每个操作的结果按顺序一起显示。

### <a name="disable-the-start-button"></a><a name="BKMK_DisableTheStartButton"></a>禁用“开始”按钮

可以通过在 `StartButton_Click` 事件处理程序顶部禁用“开始”  按钮，在操作运行期间阻止该按钮。 随后可以在操作完成时从 `Finally` 块中重新启用中该按钮，以便用户可以再次运行应用。

下面的代码演示了这些更改（使用星号标记）。 可以将更改添加到本主题末尾的代码中，或从[异步示例：.NET 桌面应用中的重新进入](https://code.msdn.microsoft.com/Async-Sample-Preventing-a8489f06)下载已完成的应用。 项目名是 DisableStartButton。

```vb
Private Async Sub StartButton_Click(sender As Object, e As RoutedEventArgs)
    ' This line is commented out to make the results clearer in the output.
    'ResultsTextBox.Text = ""

    ' ***Disable the Start button until the downloads are complete.
    StartButton.IsEnabled = False

    Try
        Await AccessTheWebAsync()

    Catch ex As Exception
        ResultsTextBox.Text &= vbCrLf & "Downloads failed."
    ' ***Enable the Start button in case you want to run the program again.
    Finally
        StartButton.IsEnabled = True

    End Try
End Sub
```

由于进行了这些更改，所以该按钮在 `AccessTheWebAsync` 下载网站期间不会进行响应，因此无法重新进入该进程。

### <a name="cancel-and-restart-the-operation"></a><a name="BKMK_CancelAndRestart"></a>取消和重启操作

可以使“开始”  按钮保持活动状态而不是禁用该按钮，但是如果用户再次选择该按钮，则取消已在运行的操作，让最近开始的操作继续运行。

有关取消的详细信息，请参阅[微调异步应用程序（Visual Basic）](fine-tuning-your-async-application.md)。

若要设置此方案，请对[检查并运行示例应用](#BKMD_SettingUpTheExample)中提供的基本代码进行以下更改。 还可以从[异步示例：.NET 桌面应用中的重新进入](https://code.msdn.microsoft.com/Async-Sample-Preventing-a8489f06)下载压缩文件。 此项目的名称是 CancelAndRestart。

1. 声明 <xref:System.Threading.CancellationTokenSource> 变量 `cts`，它处于所有方法的范围内。

    ```vb
    Class MainWindow // Or Class MainPage

        ' *** Declare a System.Threading.CancellationTokenSource.
        Dim cts As CancellationTokenSource
    ```

2. 在 `StartButton_Click` 中，确定操作是否已在进行。 如果的值 `cts` 为 `Nothing` ，则没有任何操作处于活动状态。 如果值不是 `Nothing` ，则取消已在运行的操作。

    ```vb
    ' *** If a download process is already underway, cancel it.
    If cts IsNot Nothing Then
        cts.Cancel()
    End If
    ```

3. 将 `cts` 设置为表示当前进程的不同值。

    ```vb
    ' *** Now set cts to cancel the current process if the button is chosen again.
    Dim newCTS As CancellationTokenSource = New CancellationTokenSource()
    cts = newCTS
    ```

4. 结束时 `StartButton_Click` ，当前进程完成，因此将的值设置 `cts` 为 `Nothing` 。

    ```vb
    ' *** When the process completes, signal that another process can proceed.
    If cts Is newCTS Then
        cts = Nothing
    End If
    ```

以下代码显示了 `StartButton_Click` 中的所有更改。 新增内容标有星号。

```vb
Private Async Sub StartButton_Click(sender As Object, e As RoutedEventArgs)

    ' This line is commented out to make the results clearer.
    'ResultsTextBox.Text = ""

    ' *** If a download process is underway, cancel it.
    If cts IsNot Nothing Then
        cts.Cancel()
    End If

    ' *** Now set cts to cancel the current process if the button is chosen again.
    Dim newCTS As CancellationTokenSource = New CancellationTokenSource()
    cts = newCTS

    Try
        ' *** Send a token to carry the message if the operation is canceled.
        Await AccessTheWebAsync(cts.Token)

    Catch ex As OperationCanceledException
        ResultsTextBox.Text &= vbCrLf & "Download canceled." & vbCrLf

    Catch ex As Exception
        ResultsTextBox.Text &= vbCrLf & "Downloads failed."
    End Try

    ' *** When the process is complete, signal that another process can proceed.
    If cts Is newCTS Then
        cts = Nothing
    End If
End Sub
```

在 `AccessTheWebAsync` 中，进行下列更改。

- 添加参数以从 `StartButton_Click` 接受取消标记。

- 使用 <xref:System.Net.Http.HttpClient.GetAsync%2A> 方法下载网站，因为 `GetAsync` 接受 <xref:System.Threading.CancellationToken> 参数。

- 调用 `DisplayResults` 以显示下载的每个网站的结果之前，检查 `ct` 以验证当前操作是否未取消。

 下面的代码演示了这些更改（使用星号标记）。

```vb
' *** Provide a parameter for the CancellationToken from StartButton_Click.
Private Async Function AccessTheWebAsync(ct As CancellationToken) As Task

    ' Declare an HttpClient object.
    Dim client = New HttpClient()

    ' Make a list of web addresses.
    Dim urlList As List(Of String) = SetUpURLList()

    Dim total = 0
    Dim position = 0

    For Each url In urlList
        ' *** Use the HttpClient.GetAsync method because it accepts a
        ' cancellation token.
        Dim response As HttpResponseMessage = Await client.GetAsync(url, ct)

        ' *** Retrieve the website contents from the HttpResponseMessage.
        Dim urlContents As Byte() = Await response.Content.ReadAsByteArrayAsync()

        ' *** Check for cancellations before displaying information about the
        ' latest site.
        ct.ThrowIfCancellationRequested()

        position += 1
        DisplayResults(url, urlContents, position)

        ' Update the total.
        total += urlContents.Length
    Next

    ' Display the total count for all of the websites.
    ResultsTextBox.Text &=
        String.Format(vbCrLf & vbCrLf & "TOTAL bytes returned:  " & total & vbCrLf)
End Function
```

如果在此应用运行期间多次选择 "**开始**" 按钮，则它应生成类似于以下输出的结果：

```console
1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               122505
5. msdn.microsoft.com/library/hh524395.aspx                68959
6. msdn.microsoft.com/library/ms404677.aspx               197325
Download canceled.

1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
Download canceled.

1. msdn.microsoft.com/library/hh191443.aspx                83732
2. msdn.microsoft.com/library/aa578028.aspx               205273
3. msdn.microsoft.com/library/jj155761.aspx                29019
4. msdn.microsoft.com/library/hh290140.aspx               117152
5. msdn.microsoft.com/library/hh524395.aspx                68959
6. msdn.microsoft.com/library/ms404677.aspx               197325
7. msdn.microsoft.com                                            42972
8. msdn.microsoft.com/library/ff730837.aspx               146159

TOTAL bytes returned:  890591
```

若要消除部分列表，请对 `StartButton_Click` 中的第一行代码取消注释以在用户每次重新启动操作时清除文本框。

### <a name="run-multiple-operations-and-queue-the-output"></a><a name="BKMK_RunMultipleOperations"></a>运行多个操作并将输出排入队列

此第三个示例最复杂，因为应用会在用户每次选择“开始”  按钮时启动另一个异步操作，并且所有操作都会运行到完成。 所有请求的操作以异步方式从列表中下载网站，但是操作的输出会按顺序呈现。 也就是说，实际下载活动是交错进行的（如[识别重新进入](#BKMK_RecognizingReentrancy)中的输出所示），但是每个组的结果列表会分开呈现。

操作会共享一个全局 <xref:System.Threading.Tasks.Task> (`pendingWork`)，它用作显示进程的守卫。

可以通过将更改粘贴到[生成应用](#BKMK_BuildingTheApp)中的代码来运行此示例，也可以按照[下载应用](#BKMK_DownloadingTheApp)中的说明下载示例，然后运行 QueueResults 项目。

下面的输出显示用户仅选择“开始”  按钮一次时的结果。 字母标签 A 指示结果来自首次选择“开始”  按钮。 编号显示下载目标列表中 URL 的顺序。

```console
#Starting group A.
#Task assigned for group A.

A-1. msdn.microsoft.com/library/hh191443.aspx                87389
A-2. msdn.microsoft.com/library/aa578028.aspx               209858
A-3. msdn.microsoft.com/library/jj155761.aspx                30870
A-4. msdn.microsoft.com/library/hh290140.aspx               119027
A-5. msdn.microsoft.com/library/hh524395.aspx                71260
A-6. msdn.microsoft.com/library/ms404677.aspx               199186
A-7. msdn.microsoft.com                                            53266
A-8. msdn.microsoft.com/library/ff730837.aspx               148020

TOTAL bytes returned:  918876

#Group A is complete.
```

如果用户选择“开始”  按钮三次，则应用会生成类似于以下各行的输出。 以井号 (#) 开头的信息行会跟踪应用程序的进度。

```console
#Starting group A.
#Task assigned for group A.

A-1. msdn.microsoft.com/library/hh191443.aspx                87389
A-2. msdn.microsoft.com/library/aa578028.aspx               207089
A-3. msdn.microsoft.com/library/jj155761.aspx                30870
A-4. msdn.microsoft.com/library/hh290140.aspx               119027
A-5. msdn.microsoft.com/library/hh524395.aspx                71259
A-6. msdn.microsoft.com/library/ms404677.aspx               199185

#Starting group B.
#Task assigned for group B.

A-7. msdn.microsoft.com                                            53266

#Starting group C.
#Task assigned for group C.

A-8. msdn.microsoft.com/library/ff730837.aspx               148010

TOTAL bytes returned:  916095

B-1. msdn.microsoft.com/library/hh191443.aspx                87389
B-2. msdn.microsoft.com/library/aa578028.aspx               207089
B-3. msdn.microsoft.com/library/jj155761.aspx                30870
B-4. msdn.microsoft.com/library/hh290140.aspx               119027
B-5. msdn.microsoft.com/library/hh524395.aspx                71260
B-6. msdn.microsoft.com/library/ms404677.aspx               199186

#Group A is complete.

B-7. msdn.microsoft.com                                            53266
B-8. msdn.microsoft.com/library/ff730837.aspx               148010

TOTAL bytes returned:  916097

C-1. msdn.microsoft.com/library/hh191443.aspx                87389
C-2. msdn.microsoft.com/library/aa578028.aspx               207089

#Group B is complete.

C-3. msdn.microsoft.com/library/jj155761.aspx                30870
C-4. msdn.microsoft.com/library/hh290140.aspx               119027
C-5. msdn.microsoft.com/library/hh524395.aspx                72765
C-6. msdn.microsoft.com/library/ms404677.aspx               199186
C-7. msdn.microsoft.com                                            56190
C-8. msdn.microsoft.com/library/ff730837.aspx               148010

TOTAL bytes returned:  920526

#Group C is complete.
```

组 B 和 C 会在组 A 完成之前开始，但是每个组的输出会分开显示。 组 A 的所有输出会首先显示，后跟组 B 的所有输出，再然后是组 C 的所有输出。应用会始终按顺序显示组，并且对于每个组，始终按 URL 在 URL 列表中的显示顺序来显示有关各个网站的信息。

但是，无法预测下载实际发生的顺序。 在多个组启动之后，它们生成的下载任务都处于活动状态。 无法假定 A-1 会在 B-1 之前下载，并且无法假定 A-1 会在 A-2 之前下载。

#### <a name="global-definitions"></a>全局定义

示例代码包含所有方法都可见的以下两个全局声明。

```vb
Class MainWindow    ' Class MainPage in Windows Store app.

    ' ***Declare the following variables where all methods can access them.
    Private pendingWork As Task = Nothing
    Private group As Char = ChrW(AscW("A") - 1)
```

`Task` 变量 `pendingWork` 会监视显示进程并防止任何组中断另一个组的显示操作。 字符变量 `group` 标记来自不同组的输出以验证结果是否按预期顺序出现。

#### <a name="the-click-event-handler"></a>单击事件处理程序

事件处理程序 `StartButton_Click` 会在用户每次选择“开始”  按钮时增加组号。 随后处理程序会调用 `AccessTheWebAsync` 以运行下载操作。

```vb
Private Async Sub StartButton_Click(sender As Object, e As RoutedEventArgs)
    ' ***Verify that each group's results are displayed together, and that
    ' the groups display in order, by marking each group with a letter.
    group = ChrW(AscW(group) + 1)
    ResultsTextBox.Text &= String.Format(vbCrLf & vbCrLf & "#Starting group {0}.", group)

    Try
        ' *** Pass the group value to AccessTheWebAsync.
        Dim finishedGroup As Char = Await AccessTheWebAsync(group)

        ' The following line verifies a successful return from the download and
        ' display procedures.
        ResultsTextBox.Text &= String.Format(vbCrLf & vbCrLf & "#Group {0} is complete." & vbCrLf, finishedGroup)

    Catch ex As Exception
        ResultsTextBox.Text &= vbCrLf & "Downloads failed."

    End Try
End Sub
```

#### <a name="the-accessthewebasync-method"></a>AccessTheWebAsync 方法

此示例将 `AccessTheWebAsync` 拆分为两个方法。 第一个方法 `AccessTheWebAsync` 会为组启动所有下载任务并设置 `pendingWork` 以控制显示进程。 该方法使用语言集成查询（LINQ 查询）和 <xref:System.Linq.Enumerable.ToArray%2A> 同时启动所有下载任务。

`AccessTheWebAsync` 随后调用 `FinishOneGroupAsync` 以等待每个下载完成并显示其长度。

`FinishOneGroupAsync` 会返回在 `AccessTheWebAsync` 中分配给 `pendingWork` 的任务。 该值会在任务完成之前阻止另一个操作进行中断。

```vb
Private Async Function AccessTheWebAsync(grp As Char) As Task(Of Char)

    Dim client = New HttpClient()

    ' Make a list of the web addresses to download.
    Dim urlList As List(Of String) = SetUpURLList()

    ' ***Kick off the downloads. The application of ToArray activates all the download tasks.
    Dim getContentTasks As Task(Of Byte())() =
        urlList.Select(Function(addr) client.GetByteArrayAsync(addr)).ToArray()

    ' ***Call the method that awaits the downloads and displays the results.
    ' Assign the Task that FinishOneGroupAsync returns to the gatekeeper task, pendingWork.
    pendingWork = FinishOneGroupAsync(urlList, getContentTasks, grp)

    ResultsTextBox.Text &=
        String.Format(vbCrLf & "#Task assigned for group {0}. Download tasks are active." & vbCrLf, grp)

    ' ***This task is complete when a group has finished downloading and displaying.
    Await pendingWork

    ' You can do other work here or just return.
    Return grp
End Function
```

#### <a name="the-finishonegroupasync-method"></a>FinishOneGroupAsync 方法

此方法会循环访问组中的下载任务（等待每个任务、显示下载网站的长度并将该长度添加到总和中）。

`FinishOneGroupAsync` 中的第一个语句使用 `pendingWork` 确保进入方法不会干扰已在显示进程中或已在等待的操作。 如果这类操作正在进行，则进入操作必须等待轮到它。

```vb
Private Async Function FinishOneGroupAsync(urls As List(Of String), contentTasks As Task(Of Byte())(), grp As Char) As Task

    ' Wait for the previous group to finish displaying results.
    If pendingWork IsNot Nothing Then
        Await pendingWork
    End If

    Dim total = 0

    ' contentTasks is the array of Tasks that was created in AccessTheWebAsync.
    For i As Integer = 0 To contentTasks.Length - 1
        ' Await the download of a particular URL, and then display the URL and
        ' its length.
        Dim content As Byte() = Await contentTasks(i)
        DisplayResults(urls(i), content, i, grp)
        total += content.Length
    Next

    ' Display the total count for all of the websites.
    ResultsTextBox.Text &=
        String.Format(vbCrLf & vbCrLf & "TOTAL bytes returned:  " & total & vbCrLf)
End Function
```

可以通过将更改粘贴到[生成应用](#BKMK_BuildingTheApp)中的代码来运行此示例，也可以按照[下载应用](#BKMK_DownloadingTheApp)中的说明下载示例，然后运行 QueueResults 项目。

#### <a name="points-of-interest"></a>兴趣点

输出中以井号 (#) 开头的信息行阐明了此示例的工作原理。

输出演示以下模式。

- 一个组可以上一组显示其输出期间启动，但上一组的输出显示不会中断。

  ```console
  #Starting group A.
  #Task assigned for group A. Download tasks are active.

  A-1. msdn.microsoft.com/library/hh191443.aspx                87389
  A-2. msdn.microsoft.com/library/aa578028.aspx               207089
  A-3. msdn.microsoft.com/library/jj155761.aspx                30870
  A-4. msdn.microsoft.com/library/hh290140.aspx               119037
  A-5. msdn.microsoft.com/library/hh524395.aspx                71260

  #Starting group B.
  #Task assigned for group B. Download tasks are active.

  A-6. msdn.microsoft.com/library/ms404677.aspx               199186
  A-7. msdn.microsoft.com                                            53078
  A-8. msdn.microsoft.com/library/ff730837.aspx               148010

  TOTAL bytes returned:  915919

  B-1. msdn.microsoft.com/library/hh191443.aspx                87388
  B-2. msdn.microsoft.com/library/aa578028.aspx               207089
  B-3. msdn.microsoft.com/library/jj155761.aspx                30870

  #Group A is complete.

  B-4. msdn.microsoft.com/library/hh290140.aspx               119027
  B-5. msdn.microsoft.com/library/hh524395.aspx                71260
  B-6. msdn.microsoft.com/library/ms404677.aspx               199186
  B-7. msdn.microsoft.com                                            53078
  B-8. msdn.microsoft.com/library/ff730837.aspx               148010

  TOTAL bytes returned:  915908
  ```

- `pendingWork`任务 `Nothing` 仅在组 A 的开头 `FinishOneGroupAsync` ，后者首先启动。 组 A 在它到达 `FinishOneGroupAsync` 时尚未尚未完成 await 表达式。 因此，控制权未返回给 `AccessTheWebAsync`，对 `pendingWork` 的第一个分配尚未发生。

- 下面两行始终在输出中一起显示。 该代码从不会在于 `StartButton_Click` 中启动组操作与将组的任务分配给 `pendingWork` 之间中断。

  ```console
  #Starting group B.
  #Task assigned for group B. Download tasks are active.
  ```

  组进入 `StartButton_Click` 之后，操作在操作进入 `FinishOneGroupAsync` 之前不会完成 await 表达式。 因此，没有其他操作可以在代码段期间获得控制权。

## <a name="reviewing-and-running-the-example-app"></a><a name="BKMD_SettingUpTheExample"></a>检查并运行示例应用

若要更好地了解示例应用，可以下载它，自己生成或查看本主题末尾的代码，而无需实现应用。

> [!NOTE]
> 若要将示例作为 Windows Presentation Foundation (WPF) 桌面应用运行，计算机上必须安装有 Visual Studio 2012 或更高版本和 .NET Framework 4.5 或更高版本。

### <a name="downloading-the-app"></a><a name="BKMK_DownloadingTheApp"></a>下载应用

1. 从[异步示例：.NET 桌面应用中的重新进入](https://code.msdn.microsoft.com/Async-Sample-Preventing-a8489f06)下载压缩文件。

2. 解压缩下载的文件，然后启动 Visual Studio。

3. 在菜单栏上，依次选择 **“文件”** 、 **“打开”** 和 **“项目/解决方案”** 。

4. 导航到保存解压缩的示例代码的文件夹，然后打开解决方案 (.sln) 文件。

5. 在“解决方案资源管理器”  中，打开要运行的项目的快捷菜单，然后选择“设置为 StartUpProject”  。

6. 选择 CTRL+F5 键以生成并运行项目。

### <a name="building-the-app"></a><a name="BKMK_BuildingTheApp"></a>生成应用

以下部分提供用于将示例生成为 WPF 应用的代码。

##### <a name="to-build-a-wpf-app"></a>生成 WPF 应用程序

1. 启动 Visual Studio。

2. 在菜单栏上，依次选择“文件”  、“新建”  、“项目”  。

     **“新建项目”** 对话框随即打开。

3. 在 "**已安装的模板**" 窗格中，展开 " **Visual Basic**"，然后展开 " **Windows**"。

4. 在项目类型列表中，选择“WPF 应用程序”  。

5. 将项目命名为 `WebsiteDownloadWPF`，选择 .NET Framework 版本 4.6 或更高版本，然后单击“确定”按钮  。

     新项目将出现在“解决方案资源管理器”  中。

6. 在 Visual Studio 代码编辑器中，选择 **“MainWindow.xaml”** 选项卡。

     如果此选项卡不可见，则在“解决方案资源管理器”  中，打开 MainWindow.xaml 的快捷菜单，然后选择“查看代码”  。

7. 在 MainWindow.xaml 的“XAML”  视图中，将代码替换为以下代码。

    ```xaml
    <Window x:Class="MainWindow"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:WebsiteDownloadWPF"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid Width="517" Height="360">
            <Button x:Name="StartButton" Content="Start" HorizontalAlignment="Left" Margin="-1,0,0,0" VerticalAlignment="Top" Click="StartButton_Click" Height="53" Background="#FFA89B9B" FontSize="36" Width="518"  />
            <TextBox x:Name="ResultsTextBox" HorizontalAlignment="Left" Margin="-1,53,0,-36" TextWrapping="Wrap" VerticalAlignment="Top" Height="343" FontSize="10" ScrollViewer.VerticalScrollBarVisibility="Visible" Width="518" FontFamily="Lucida Console" />
        </Grid>
    </Window>
    ```

     MainWindow.xaml 的“设计”  视图中将显示一个简单的窗口，其中包含一个文本框和一个按钮。

8. 在“解决方案资源管理器”中，右键单击“引用”并选择“添加引用”    。

     如果尚未选择，请为 <xref:System.Net.Http> 添加引用。

9. 在**解决方案资源管理器**中，打开 mainwindow.xaml 的快捷菜单，然后选择 "**查看代码**"。

10. 在 Mainwindow.xaml 中，将代码替换为以下代码。

    ```vb
    ' Add the following Imports statements, and add a reference for System.Net.Http.
    Imports System.Net.Http
    Imports System.Threading

    Class MainWindow

        Private Async Sub StartButton_Click(sender As Object, e As RoutedEventArgs)
            System.Net.ServicePointManager.SecurityProtocol = System.Net.ServicePointManager.SecurityProtocol Or System.Net.SecurityProtocolType.Tls12

            ' This line is commented out to make the results clearer in the output.
            'ResultsTextBox.Text = ""

            Try
                Await AccessTheWebAsync()

            Catch ex As Exception
                ResultsTextBox.Text &= vbCrLf & "Downloads failed."

            End Try
        End Sub

        Private Async Function AccessTheWebAsync() As Task

            ' Declare an HttpClient object.
            Dim client = New HttpClient()

            ' Make a list of web addresses.
            Dim urlList As List(Of String) = SetUpURLList()

            Dim total = 0
            Dim position = 0

            For Each url In urlList
                ' GetByteArrayAsync returns a task. At completion, the task
                ' produces a byte array.
                Dim urlContents As Byte() = Await client.GetByteArrayAsync(url)

                position += 1
                DisplayResults(url, urlContents, position)

                ' Update the total.
                total += urlContents.Length
            Next

            ' Display the total count for all of the websites.
            ResultsTextBox.Text &=
                String.Format(vbCrLf & vbCrLf & "TOTAL bytes returned:  " & total & vbCrLf)
        End Function

        Private Function SetUpURLList() As List(Of String)
            Dim urls = New List(Of String) From
            {
                "https://msdn.microsoft.com/library/hh191443.aspx",
                "https://msdn.microsoft.com/library/aa578028.aspx",
                "https://msdn.microsoft.com/library/jj155761.aspx",
                "https://msdn.microsoft.com/library/hh290140.aspx",
                "https://msdn.microsoft.com/library/hh524395.aspx",
                "https://msdn.microsoft.com/library/ms404677.aspx",
                "https://msdn.microsoft.com",
                "https://msdn.microsoft.com/library/ff730837.aspx"
            }
            Return urls
        End Function

        Private Sub DisplayResults(url As String, content As Byte(), pos As Integer)
            ' Display the length of each website. The string format is designed
            ' to be used with a monospaced font, such as Lucida Console or
            ' Global Monospace.

            ' Strip off the "http:'".
            Dim displayURL = url.Replace("https://", "")
            ' Display position in the URL list, the URL, and the number of bytes.
            ResultsTextBox.Text &= String.Format(vbCrLf & "{0}. {1,-58} {2,8}", pos, displayURL, content.Length)
        End Sub
    End Class
    ```

11. 选择 CTRL+F5 键以运行程序，然后多次选择“开始”  按钮。

12. 从[禁用“开始”按钮](#BKMK_DisableTheStartButton)、[取消并重启操作](#BKMK_CancelAndRestart)或[运行多个操作并将输出排入队列](#BKMK_RunMultipleOperations)中进行更改以处理重新进入。

## <a name="see-also"></a>请参阅

- [演练：使用 Async 和 Await 访问 Web (Visual Basic)](walkthrough-accessing-the-web-by-using-async-and-await.md)
- [使用 Async 和 Await 的异步编程 (Visual Basic)](index.md)
