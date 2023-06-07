---
title: 操作文件和目录
ms.date: 07/20/2015
helpviewer_keywords:
- files [Visual Basic], reading text
- reading files [Visual Basic], text
- I/O [Visual Basic], walkthroughs
- text, writing to files
- text, reading from files
- reading text from files [Visual Basic], walkthroughs
- Visual Basic code, file access
- files [Visual Basic], writing text
- I/O [Visual Basic], writing text to files
- file access, walkthroughs
- writing to files [Visual Basic], walkthroughs
- I/O [Visual Basic], reading text from files
ms.assetid: cae77565-9f78-4e46-8e42-eb2f9f8e1ffd
ms.openlocfilehash: 4b77618e5cd525cf3ad012405f402681aa5bb52c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406659"
---
# <a name="walkthrough-manipulating-files-and-directories-in-visual-basic"></a>演练：在 Visual Basic 中操作文件和目录

本演练简单介绍 Visual Basic 中文件 I/O 的基础知识。 描述如何创建列出并检查目录中文本文件的小型应用程序。 对于所选的每个文本文件，该应用程序都会提供文件属性和内容的第一行。 可以选择将信息写入日志文件中。  
  
 本演练使用 `My.Computer.FileSystem Object` 的成员，这些成员可从 Visual Basic 中获得。 有关更多信息，请参见<xref:Microsoft.VisualBasic.FileIO.FileSystem>。 本演练结尾部分提供等效示例，该示例使用来自 <xref:System.IO> 命名空间的类。  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
### <a name="to-create-the-project"></a>创建项目  
  
1. 在“文件”菜单上，单击“新建项目”。    
  
     此时将出现“新建项目”  对话框。  
  
2. 在“已安装的模板”窗格中，展开“Visual Basic”，然后单击“Windows”。    在中间的“模板”窗格中，单击“Windows 窗体应用程序”。    
  
3. 在“名称”框中，键入 `FileExplorer` 以设置项目名称，然后单击“确定”。  
  
     Visual Studio 会将项目添加到“解决方案资源管理器”  中，此时“Windows 窗体设计器”随即打开。  
  
4. 将下表中的控件添加到窗体，并设置控件属性相应的值。  
  
    |控件|Property|“值”|  
    |-------------|--------------|-----------|  
    |**ListBox**|**Name**|`filesListBox`|  
    |**Button**|**Name**<br /><br /> **文本**|`browseButton`<br /><br /> **浏览**|  
    |**Button**|**Name**<br /><br /> **文本**|`examineButton`<br /><br /> **检查**|  
    |**CheckBox**|**Name**<br /><br /> **文本**|`saveCheckBox`<br /><br /> **保存结果**|  
    |**FolderBrowserDialog**|**Name**|`FolderBrowserDialog1`|  
  
### <a name="to-select-a-folder-and-list-files-in-a-folder"></a>选择一个文件夹，并列出文件夹中的文件  
  
1. 通过双击窗体上的控件，创建 `browseButton` 的 `Click` 事件处理程序。 代码编辑器随即打开。  
  
2. 将以下代码添加到 `Click` 事件处理程序中。  
  
     [!code-vb[VbVbcnMyFileSystem#103](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/class2.vb#103)]  
  
     `FolderBrowserDialog1.ShowDialog` 调用将打开“浏览文件夹”对话框。  用户单击“确定”后  ，<xref:System.Windows.Forms.FolderBrowserDialog.SelectedPath%2A> 属性作为参数发送给在下一步中添加的 `ListFiles` 方法。  
  
3. 添加以下 `ListFiles` 方法。  
  
     [!code-vb[VbVbcnMyFileSystem#104](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/class2.vb#104)]  
  
     此代码首先会清除“ListBox”。   
  
     <xref:Microsoft.VisualBasic.FileIO.FileSystem.GetFiles%2A> 方法然后检索字符串集合，一个字符串对应目录中的一个文件。 `GetFiles` 方法接受一个搜索模式参数来检索与特定模式匹配的文件。 在此示例中，仅返回具有 .txt 扩展名的文件。  
  
     通过 `GetFiles` 方法返回的字符串随后将添加到“ListBox”中。   
  
4. 运行该应用程序。 单击“浏览”  按钮。 在“浏览文件夹”对话框中，浏览到包含 .txt 文件的文件夹，然后选择该文件夹，并单击“确定”。    
  
     `ListBox` 包含所选文件夹中 .txt 文件的列表。  
  
5. 停止运行该应用程序。  
  
### <a name="to-obtain-attributes-of-a-file-and-content-from-a-text-file"></a>从文本文件获取文件的属性和内容  
  
1. 通过双击窗体上的控件，创建 `examineButton` 的 `Click` 事件处理程序。  
  
2. 将以下代码添加到 `Click` 事件处理程序中。  
  
     [!code-vb[VbVbcnMyFileSystem#105](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/class2.vb#105)]  
  
     该代码验证是否在 `ListBox` 中选中某项。 然后，将从 `ListBox` 获取文件路径项。 <xref:Microsoft.VisualBasic.FileIO.FileSystem.FileExists%2A> 方法用于检查文件是否仍然存在。  
  
     该文件路径作为参数发送给下一步中添加的 `GetTextForOutput` 方法。 此方法返回一个包含文件信息字符串。 该文件信息将出现在“MessageBox”中。   
  
3. 添加以下 `GetTextForOutput` 方法。  
  
     [!code-vb[VbVbcnMyFileSystem#107](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/class2.vb#107)]  
  
     该代码使用 <xref:Microsoft.VisualBasic.FileIO.FileSystem.GetFileInfo%2A> 方法来获取文件参数。 文件参数添加到 <xref:System.Text.StringBuilder>。  
  
     <xref:Microsoft.VisualBasic.FileIO.FileSystem.OpenTextFileReader%2A> 方法将文件内容读取到 <xref:System.IO.StreamReader>。 该内容的第一行是从 `StreamReader` 获取的，然后将添加到 `StringBuilder`。  
  
4. 运行该应用程序。 单击“浏览”，然后浏览到包含 .txt 文件的文件夹。  单击“确定”。   
  
     在 `ListBox` 中选择一个文件，然后单击“检查”  。 `MessageBox` 显示文件信息。  
  
5. 停止运行该应用程序。  
  
### <a name="to-add-a-log-entry"></a>添加日志项目  
  
1. 将以下代码添加到 `examineButton_Click` 事件处理程序末尾。  
  
     [!code-vb[VbVbcnMyFileSystem#106](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/class2.vb#106)]  
  
     该代码设置日志文件路径以便将日志文件放在与所选文件相同的目录中。 日志项目的文本设置为当前日期和文件信息后的时间。  
  
     <xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllText%2A> 方法，其 `append` 参数设置为 `True`，用于创建日志条目。  
  
2. 运行该应用程序。 浏览到一个文本文件，在 `ListBox` 中选中它，选择“保存结果”复选框，然后再单击“检查”。   验证日志条目是否写入 `log.txt` 文件。  
  
3. 停止运行该应用程序。  
  
### <a name="to-use-the-current-directory"></a>使用当前目录  
  
1. 通过双击窗体，创建 `Form1_Load` 的事件处理程序。  
  
2. 将以下代码添加到该事件处理程序中。  
  
     [!code-vb[VbVbcnMyFileSystem#102](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/class2.vb#102)]  
  
     此代码将文件夹浏览器的默认目录设置为当前目录。  
  
3. 运行该应用程序。 第一次单击“浏览”时，“浏览文件夹”对话框将打开到当前目录。    
  
4. 停止运行该应用程序。  
  
### <a name="to-selectively-enable-controls"></a>有选择地启用控件  
  
1. 添加以下 `SetEnabled` 方法。  
  
     [!code-vb[VbVbcnMyFileSystem#108](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/class2.vb#108)]  
  
     `SetEnabled` 方法启用还是禁用控件是由是否选中 `ListBox` 中的项决定的。  
  
2. 通过双击窗体上的 `ListBox` 控件，创建 `filesListBox` 的 `SelectedIndexChanged` 事件处理程序。  
  
3. 在新的 `filesListBox_SelectedIndexChanged` 事件处理程序中添加对 `SetEnabled` 的调用。  
  
4. 在 `browseButton_Click` 事件处理程序末尾添加对 `SetEnabled` 的调用。  
  
5. 在 `Form1_Load` 事件处理程序末尾添加对 `SetEnabled` 的调用。  
  
6. 运行该应用程序。 如果在 `ListBox` 中未选中任何项，将禁用“保存结果”复选框和“检查”按钮。  
  
## <a name="full-example-using-mycomputerfilesystem"></a>使用 My.Computer.FileSystem 的完整示例  

 以下是完整示例。  
  
 [!code-vb[VbVbcnMyFileSystem#101](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/class2.vb#101)]  
  
## <a name="full-example-using-systemio"></a>使用 System.IO 的完整示例  

 以下等效示例使用来自 <xref:System.IO> 命名空间的类，而不使用 `My.Computer.FileSystem` 对象。  
  
 [!code-vb[VbVbcnMyFileSystem#111](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/class3.vb#111)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.IO>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.CurrentDirectory%2A>
- [演练：使用 .NET Framework 方法操作文件](walkthrough-manipulating-files-by-using-net-framework-methods.md)
