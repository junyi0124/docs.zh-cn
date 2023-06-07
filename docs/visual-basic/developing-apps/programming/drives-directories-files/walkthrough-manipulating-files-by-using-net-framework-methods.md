---
title: 使用 .NET Framework 方法操作文件
ms.date: 07/20/2015
helpviewer_keywords:
- I/O [Visual Basic], walkthroughs
- text files [Visual Basic], writing to
- reading text files [Visual Basic]
- text, writing to files
- files [Visual Basic], searching
- StreamReader class, walkthroughs
- files [Visual Basic], accessing
- I/O [Visual Basic], writing text to files
- writing to files [Visual Basic], walkthroughs
- StreamWriter class, walkthroughs
- text files [Visual Basic], reading
- I/O [Visual Basic], reading text from files
ms.assetid: 7d2109eb-f98a-4389-b43d-30f384aaa7d5
ms.openlocfilehash: 9abb87f3f6cdefefef29eb37c2c2d4d15155e93d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406646"
---
# <a name="walkthrough-manipulating-files-by-using-net-framework-methods-visual-basic"></a>演练：使用 .NET Framework 方法操作文件 (Visual Basic)

此演练演示了如何使用 <xref:System.IO.StreamReader> 类打开和读取文件，如何查看文件是否正被访问，如何在使用 <xref:System.IO.StreamReader> 类实例读取的文件中搜索字符串，以及如何使用 <xref:System.IO.StreamWriter> 类写入文件。

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

## <a name="creating-the-application"></a>创建应用程序

启动 Visual Studio，并创建一个用户可用于写入指定文件的窗体来开始项目。

### <a name="to-create-the-project"></a>创建项目

1. 在“文件”菜单上，选择“新建项目”。  

2. 在“新建项目”窗格中，单击“Windows 应用程序”。  

3. 在“名称”框中，键入 `MyDiary`，然后单击“确定”。

     Visual Studio 将项目添加到“解决方案资源管理器”中，“Windows 窗体设计器”随即打开   。

4. 将下表中的控件添加到窗体中，并为其属性设置相应的值。

|**对象**|<bpt id="p1">**</bpt>Properties<ept id="p1">**</ept>|**值**|
|---|---|---|
|<xref:System.Windows.Forms.Button>|**Name**<br /><br /> **文本**|`Submit`<br /><br /> **提交项**|
|<xref:System.Windows.Forms.Button>|**Name**<br /><br /> **文本**|`Clear`<br /><br /> **清除项**|
|<xref:System.Windows.Forms.TextBox>|**Name**<br /><br /> **文本**<br /><br /> **多行**|`Entry`<br /><br /> **请输入内容。**<br /><br /> `False`|

## <a name="writing-to-the-file"></a>写入文件

若要通过应用程序添加写入文件的功能，请使用 <xref:System.IO.StreamWriter> 类。 <xref:System.IO.StreamWriter> 设计用于特定编码的字符输出，而 <xref:System.IO.Stream> 类设计用于字节的输入和输出。 使用 <xref:System.IO.StreamWriter> 可将多行信息写入标准的文本文件。 有关 <xref:System.IO.StreamWriter> 类的详细信息，请参阅 <xref:System.IO.StreamWriter>。

### <a name="to-add-writing-functionality"></a>添加写入功能

1. 从“视图”菜单中选择“代码”，以打开代码编辑器。  

2. 由于该应用程序引用 <xref:System.IO> 命名空间，因此，请在代码的最开头处，在窗体的类声明（以 `Public Class Form1` 开始）之前，添加以下语句。

     [!code-vb[VbVbcnMyFileSystem#35](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#35)]

     写入文件前，必须创建一个 <xref:System.IO.StreamWriter> 类的实例。

3. 从“视图”菜单中选择“设计器”，以返回“Windows 窗体设计器”。    双击 `Submit` 按钮，为该按钮创建一个 <xref:System.Windows.Forms.Control.Click> 事件处理程序，然后添加以下代码。

     [!code-vb[VbVbcnMyFileSystem#36](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#36)]

> [!NOTE]
> Visual Studio 集成开发环境 (IDE) 将返回代码编辑器，并将插入点放在应在其中添加代码的事件处理程序内。

1. 若要写入该文件，请使用 <xref:System.IO.StreamWriter> 类的 <xref:System.IO.StreamWriter.Write%2A> 方法。 在 `Dim fw As StreamWriter` 后直接添加以下代码。 不需要担心如果找不到该文件会引发异常，因为如果它不存在，将创建该文件。

     [!code-vb[VbVbcnMyFileSystem#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#37)]

2. 在 `Dim ReadString As String` 后直接添加以下代码，确保用户无法提交空项。

     [!code-vb[VbVbcnMyFileSystem#38](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#38)]

3. 因为这是日记，所以用户需要为每一项指定一个日期。 在 `fw = New StreamWriter("C:\MyDiary.txt", True)` 之后插入以下代码，以将变量 `Today` 设置为当前日期。

     [!code-vb[VbVbcnMyFileSystem#39](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#39)]

4. 最后，附加代码以清除 <xref:System.Windows.Forms.TextBox>。 将以下代码添加到 `Clear` 按钮的 <xref:System.Windows.Forms.Control.Click> 事件中。

     [!code-vb[VbVbcnMyFileSystem#40](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#40)]

## <a name="adding-display-features-to-the-diary"></a>将显示功能添加到日记

在本节中，将添加一项功能，用于显示 `DisplayEntry`<xref:System.Windows.Forms.TextBox> 中的最新项。 还可以添加 <xref:System.Windows.Forms.ComboBox> 以显示各种项，同时用户可从中选择一个显示在 `DisplayEntry`<xref:System.Windows.Forms.TextBox> 中的条目。 <xref:System.IO.StreamReader> 类的实例从 `MyDiary.txt` 中读取。 与 <xref:System.IO.StreamWriter> 类一样，<xref:System.IO.StreamReader> 可与文本文件一起使用。

在此演练的这一节中，请将下表中的控件添加到窗体中，并为其属性设置相应的值。

|控件|属性|值|
|-------------|----------------|------------|
|<xref:System.Windows.Forms.TextBox>|**Name**<br /><br /> **可见**<br /><br /> **Size**<br /><br /> **多行**|`DisplayEntry`<br /><br /> `False`<br /><br /> `120,60`<br /><br /> `True`|
|<xref:System.Windows.Forms.Button>|**Name**<br /><br /> **文本**|`Display`<br /><br /> **显示**|
|<xref:System.Windows.Forms.Button>|**Name**<br /><br /> **文本**|`GetEntries`<br /><br /> **获取项**|
|<xref:System.Windows.Forms.ComboBox>|**Name**<br /><br /> **文本**<br /><br /> **启用**|`PickEntries`<br /><br /> **选择一项**<br /><br /> `False`|

### <a name="to-populate-the-combo-box"></a>填充组合框

1. `PickEntries`<xref:System.Windows.Forms.ComboBox> 用于显示用户提交每一项的日期，这样，用户就可以选择特定日期的项。 创建 `GetEntries` 按钮的 <xref:System.Windows.Forms.Control.Click> 事件处理程序并添加以下代码。

     [!code-vb[VbVbcnMyFileSystem#41](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#41)]

2. 若要测试代码，请按 F5 编译该应用程序，然后单击“获取项”。  单击 <xref:System.Windows.Forms.ComboBox> 中的下拉箭头，以显示条目日期。

### <a name="to-choose-and-display-individual-entries"></a>选择并显示个别项

1. 创建 `Display` 按钮的 <xref:System.Windows.Forms.Control.Click> 事件处理程序并添加以下代码。

     [!code-vb[VbVbcnMyFileSystem#42](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#42)]

2. 若要测试代码，请按 F5 编译该应用程序，然后提交一项。 单击“获取项”，从 <xref:System.Windows.Forms.ComboBox> 中选择一项，然后单击“显示”。 所选条目的内容显示在 `DisplayEntry`<xref:System.Windows.Forms.TextBox> 中。

## <a name="enabling-users-to-delete-or-modify-entries"></a>允许用户删除或修改项

最后，可以包括其他功能，以允许用户使用 `DeleteEntry` 和 `EditEntry` 按钮来删除或修改项。 除非显示有项，否则这两个按钮都保持禁用状态。

将下表中的控件添加到窗体中，并为其属性设置相应的值。

|控件|属性|值|
|-------------|----------------|------------|
|<xref:System.Windows.Forms.Button>|**Name**<br /><br /> **文本**<br /><br /> **启用**|`DeleteEntry`<br /><br /> **删除项**<br /><br /> `False`|
|<xref:System.Windows.Forms.Button>|**Name**<br /><br /> **文本**<br /><br /> **启用**|`EditEntry`<br /><br /> **编辑项**<br /><br /> `False`|
|<xref:System.Windows.Forms.Button>|**Name**<br /><br /> **文本**<br /><br /> **启用**|`SubmitEdit`<br /><br /> **提交编辑**<br /><br /> `False`|

### <a name="to-enable-deletion-and-modification-of-entries"></a>允许删除和修改项

1. 在 `DisplayEntry.Text = ReadString` 之后，将以下代码添加到 `Display` 按钮的 <xref:System.Windows.Forms.Control.Click> 事件。

     [!code-vb[VbVbcnMyFileSystem#43](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#43)]

2. 创建 `DeleteEntry` 按钮的 <xref:System.Windows.Forms.Control.Click> 事件处理程序并添加以下代码。

     [!code-vb[VbVbcnMyFileSystem#44](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#44)]

3. 用户显示某一项时，`EditEntry` 按钮将变为启用状态。 在 `DisplayEntry.Text = ReadString` 之后，将以下代码添加到 `Display` 按钮的 <xref:System.Windows.Forms.Control.Click> 事件。

     [!code-vb[VbVbcnMyFileSystem#45](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#45)]

4. 创建 `EditEntry` 按钮的 <xref:System.Windows.Forms.Control.Click> 事件处理程序并添加以下代码。

     [!code-vb[VbVbcnMyFileSystem#46](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#46)]

5. 创建 `SubmitEdit` 按钮的 <xref:System.Windows.Forms.Control.Click> 事件处理程序并添加以下代码

     [!code-vb[VbVbcnMyFileSystem#47](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#47)]

若要测试代码，请按 F5 编译该应用程序。 单击“获取项”，选择一项，然后单击“显示”。   条目将出现在 `DisplayEntry`<xref:System.Windows.Forms.TextBox> 中。 单击“编辑项”  。 条目将出现在 `Entry`<xref:System.Windows.Forms.TextBox> 中。 编辑 `Entry`<xref:System.Windows.Forms.TextBox> 中的项，然后单击“提交编辑”  。 打开 `MyDiary.txt` 文件以确认所做的更正。 现在，选择一项，然后单击“删除项”  。 当 <xref:System.Windows.Forms.MessageBox> 请求确认时，请单击“确定”  。 关闭该应用程序，然后打开 `MyDiary.txt`，以确认该项已删除。

## <a name="see-also"></a>另请参阅

- <xref:System.IO.StreamReader>
- <xref:System.IO.StreamWriter>
- [演练](../../../walkthroughs.md)
