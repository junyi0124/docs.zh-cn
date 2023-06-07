---
title: 如何：重命名文件
ms.date: 07/20/2015
helpviewer_keywords:
- I/O [Visual Basic], renaming files
- files [Visual Basic], renaming
ms.assetid: 0ea7e0c8-2cb2-4bf5-a00d-7b6e3c08a3bc
ms.openlocfilehash: 3de41ee6627315f0e26964b75f564ff98fe472ec
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411585"
---
# <a name="how-to-rename-a-file-in-visual-basic"></a>如何：在 Visual Basic 中重命名文件

使用 `My.Computer.FileSystem` 对象的 `RenameFile` 方法可通过提供当前位置、文件名和新文件名来重命名文件。 此方法不能用于移动文件；使用 `MoveFile` 方法可移动并重命名文件。  
  
### <a name="to-rename-a-file"></a>重命名文件  
  
- 使用 `My.Computer.FileSystem.RenameFile` 方法可重命名文件。 此示例将名为 `Test.txt` 的文件重命名为 `SecondTest.txt`。  
  
     [!code-vb[VbVbcnMyFileSystem#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#9)]  
  
 此代码示例也可作为 IntelliSense 代码片段。 在代码片段选取器中，该代码段位于“文件系统 - 处理驱动器、文件夹和文件”。  有关详细信息，请参阅[代码片段](/visualstudio/ide/code-snippets)。  
  
## <a name="robust-programming"></a>可靠编程  

 以下情况可能会导致异常：  
  
- 路径由于以下原因之一而无效：是零长度字符串；仅为空白；包含无效字符；是一个设备路径（开头字符为 \\\\.\\）(<xref:System.ArgumentException>)。  
  
- `newName` 包含路径信息 (<xref:System.ArgumentException>)。  
  
- 路径无效，因为它是 `Nothing` (<xref:System.ArgumentNullException>)。  
  
- `newName` 为 `Nothing` 或空字符串 (<xref:System.ArgumentNullException>)。  
  
- 源文件无效或不存在 (<xref:System.IO.FileNotFoundException>)。  
  
- 不存在具有 `newName` (<xref:System.IO.IOException>) 中指定名称的现有文件或目录。  
  
- 路径超过了系统定义的最大长度 (<xref:System.IO.PathTooLongException>)。  
  
- 路径中的文件名或目录名包含冒号 (:)，或格式无效 (<xref:System.NotSupportedException>)。  
  
- 该用户缺少查看该路径所必需的权限 (<xref:System.Security.SecurityException>)。  
  
- 用户没有所必需的权限 (<xref:System.UnauthorizedAccessException>)。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.RenameFile%2A>
- [如何：移动文件](how-to-move-a-file.md)
- [创建、删除和移动文件和目录](creating-deleting-and-moving-files-and-directories.md)
- [如何：在同一目录中创建文件副本](how-to-create-a-copy-of-a-file-in-the-same-directory.md)
- [如何：在不同的目录中创建文件的副本](how-to-create-a-copy-of-a-file-in-a-different-directory.md)
