---
title: 如何：将文本写入“我的文档”目录中的文件
ms.date: 07/20/2015
helpviewer_keywords:
- files [Visual Basic], writing to
- text, writing to files
- examples [Visual Basic], text files
- writing to files [Visual Basic], in My Documents
ms.assetid: 1c726124-781d-4976-9baa-ed46814ff3fe
ms.openlocfilehash: bb3a9bdc44f86fbcdb3c56ee088740efdfebe95d
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90546453"
---
# <a name="how-to-write-text-to-files-in-the-my-documents-directory-in-visual-basic"></a>如何：在 Visual Basic 中将文本写入“我的文档”目录中的文件

`My.Computer.FileSystem.SpecialDirectories` 对象允许用户访问特殊目录，如 MyDocuments  目录。  
  
## <a name="procedure"></a>过程  
  
#### <a name="to-write-new-text-files-in-the-my-documents-directory"></a>在“我的文档”目录中写入新的文本文件  
  
1. 使用 `My.Computer.FileSystem.SpecialDirectories.MyDocuments` 属性提供路径。  
  
     [!code-vb[VbFileIOWrite#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOWrite/VB/Class1.vb#1)]  
  
2. 使用 `WriteAllText` 方法将文本写入指定文件。  
  
     [!code-vb[VbVbcnMyFileSystem#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#14)]  
  
## <a name="example"></a>示例  

 [!code-vb[VbFileIOWrite#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOWrite/VB/Class1.vb#2)]  
  
## <a name="compiling-the-code"></a>编译代码  

 将 `test.txt` 替换为要写入的文件的名称。  
  
## <a name="robust-programming"></a>可靠编程  

 此代码会重新引发向文件写入文本时可能发生的所有异常。 可以通过使用 Windows 窗体控件（如限制用户选择有效文件名的 [OpenFileDialog](/dotnet/desktop/winforms/controls/openfiledialog-component-windows-forms) 和 [SaveFileDialog](/dotnet/desktop/winforms/controls/savefiledialog-component-windows-forms) 组件）减少引发异常的可能性。 但是，使用这些控件并不能做到万无一失。 文件系统可以在用户选择文件和代码执行的间隙时间内更改。 因此，处理文件时，几乎总是需要处理异常。  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  

 如果在部分信任上下文中运行，该代码可能会因特权不足而引发异常。 有关详细信息，请参阅[代码访问安全性基础知识](../../../../framework/misc/code-access-security-basics.md)。  
  
 此示例创建一个新文件。 如果某个应用程序需要创建文件，则该应用程序需要文件夹的“创建”权限。 可使用访问控制列表设置权限。 如果文件已存在，则该应用程序只需要“写入”权限（这是较弱的权限）。 如有可能，在部署过程中创建文件，并且仅授予针对单个文件的“读取”权限（而不是针对文件夹授予“创建”权限）会更加安全。 此外，较安全的做法是将数据写入用户文件夹，而不是根文件夹或“Program Files”  文件夹。 有关详细信息，请参阅 [ACL 技术概述](/previous-versions/dotnet/netframework-4.0/ms229742(v=vs.100))。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.IO.Path.Combine%2A?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Devices.Computer>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllText%2A>
- <xref:Microsoft.VisualBasic.FileIO.SpecialDirectories>
