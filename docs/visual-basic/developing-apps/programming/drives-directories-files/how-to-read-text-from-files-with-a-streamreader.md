---
title: 如何：使用 StreamReader 读取文件中的文本
ms.date: 07/20/2015
helpviewer_keywords:
- reading files [Visual Basic], text
- text, reading from files
- reading text from files [Visual Basic]
- files [Visual Basic], reading
ms.assetid: 384033c6-18f9-4d59-9610-36371226558f
ms.openlocfilehash: 5ec161f5146d5bea7f34d4a5b6c154f6c45b1cf4
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90546492"
---
# <a name="how-to-read-text-from-files-with-a-streamreader-visual-basic"></a>如何：使用 StreamReader 读取文件中的文本 (Visual Basic)

`My.Computer.FileSystem` 对象提供打开 <xref:System.IO.TextReader> 和 <xref:System.IO.TextWriter> 的方法。 这些方法（`OpenTextFileWriter` 和 `OpenTextFileReader`）是高级方法，除非选择“全部”  选项卡，否则它们不会出现在 IntelliSense 中。  
  
### <a name="to-read-a-line-from-a-file-with-a-text-reader"></a>使用文本读取器从文件读取一行  
  
- 使用 `OpenTextFileReader` 方法打开 <xref:System.IO.TextReader> 并指定文件。 此示例打开名为 `testfile.txt` 的文件、从中读取一行，然后在消息框中显示该行。  
  
     [!code-vb[VbFileIORead#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#1)]  
  
## <a name="robust-programming"></a>可靠编程  

 读取的文件必须是文本文件。  
  
 不要根据文件的名称来判断文件的内容。 例如，文件 Form1.vb 可能不是 Visual Basic 源文件。  
  
 在应用程序中使用输入的数据之前，需验证所有的输入内容。 文件的内容可能不是预期内容，并且用来读取该文件的方法可能失败。  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  

 若要从文件中读取，程序集需要 <xref:System.Security.Permissions.FileIOPermission> 类授予的特权等级。 如果在部分信任上下文中运行，该代码可能会因特权不足而引发异常。 有关详细信息，请参阅[代码访问安全性基础知识](../../../../framework/misc/code-access-security-basics.md)。 用户还需要具有对文件的访问权限。 有关详细信息，请参阅 [ACL 技术概述](/previous-versions/dotnet/netframework-4.0/ms229742(v=vs.100))。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileIO.FileSystem>
- <xref:System.Windows.Forms.OpenFileDialog>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.OpenTextFileWriter%2A>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.OpenTextFileReader%2A>
- [SaveFileDialog 组件](/dotnet/desktop/winforms/controls/savefiledialog-component-windows-forms)
- [从文件读取](reading-from-files.md)
