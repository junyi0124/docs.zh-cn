---
title: 如何：将具有特定模式的文件复制到目录中
ms.date: 07/20/2015
helpviewer_keywords:
- My.Computer.FileSystem.CopyFile method, copying files [Visual Basic]
- files [Visual Basic], copying
- CopyFile method [Visual Basic], copying files in Visual Basic
- I/O [Visual Basic], copying files
ms.assetid: f205d2ad-bbe5-4d55-8a40-acda21aa82dd
ms.openlocfilehash: 7e263e2c9035db54dbb58c6c78c0647d5442504e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401701"
---
# <a name="how-to-copy-files-with-a-specific-pattern-to-a-directory-in-visual-basic"></a>如何：在 Visual Basic 中将具有特定模式的文件复制到目录中

<xref:Microsoft.VisualBasic.MyServices.FileSystemProxy.GetFiles%2A> 方法返回表示文件的路径名的只读字符串集合。 可以使用 `wildCards` 参数来指定特定模式。  
  
 如果找不到匹配的文件，则返回空集合。  
  
 可以使用 <xref:Microsoft.VisualBasic.FileIO.FileSystem.CopyFile%2A> 方法将文件复制到目录。  
  
### <a name="to-copy-files-with-a-specific-pattern-to-a-directory"></a>将具有特定模式的文件复制到目录中  
  
1. 使用 `GetFiles` 方法返回文件的列表。 此示例在指定的目录中返回所有 .rtf 文件。  
  
     [!code-vb[VbFileIOMisc#36](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOMisc/VB/Class1.vb#36)]  
  
2. 使用 `CopyFile` 方法复制文件。 此示例将文件复制到名为 `testdirectory`的目录中。  
  
     [!code-vb[VbVbcnMyFileSystem#88](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#88)]  
  
3. 使用 `For` 语句关闭 `Next` 语句。  
  
     [!code-vb[VbVbcnMyFileSystem#89](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#89)]  
  
## <a name="example"></a>示例  

 下面的示例完整地显示了上述代码片段，该示例将指定目录中的所有 .rtf 文件复制到名为 `testdirectory`的目录。  
  
 [!code-vb[VbFileIOMisc#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOMisc/VB/Class1.vb#37)]  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  

 以下情况可能会导致异常：  
  
- 路径由于以下原因之一而无效：是零长度字符串；仅为空白；包含无效字符；是一个设备路径（开头字符为 \\\\.\\）(<xref:System.ArgumentException>)。  
  
- 路径无效，因为它是 `Nothing` (<xref:System.ArgumentNullException>)。  
  
- 目录不存在 (<xref:System.IO.DirectoryNotFoundException>)。  
  
- 指向某个现有文件的目录 (<xref:System.IO.IOException>)。  
  
- 路径超过了系统定义的最大长度 (<xref:System.IO.PathTooLongException>)。  
  
- 路径中的文件名或目录名包含冒号 (:)，或格式无效 (<xref:System.NotSupportedException>)。  
  
- 该用户缺少查看该路径所必需的权限 (<xref:System.Security.SecurityException>)。 该用户缺少必要的权限 (<xref:System.UnauthorizedAccessException>)。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.CopyFile%2A>
- <xref:Microsoft.VisualBasic.MyServices.FileSystemProxy.GetFiles%2A>
- [如何：查找具有特定模式的子目录](how-to-find-subdirectories-with-a-specific-pattern.md)
- [疑难解答：读取和写入文本文件](troubleshooting-reading-from-and-writing-to-text-files.md)
- [如何：获取目录中的文件集合](how-to-get-the-collection-of-files-in-a-directory.md)
