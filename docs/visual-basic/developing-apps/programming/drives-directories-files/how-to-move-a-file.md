---
title: 如何：移动文件
ms.date: 07/20/2015
helpviewer_keywords:
- files [Visual Basic], moving
ms.assetid: 53a7457b-5815-41ad-b37d-28537c1fb77a
ms.openlocfilehash: 2dafeb3b5f8b8c3a8976b25c1a57f405aebb32b9
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401597"
---
# <a name="how-to-move-a-file-in-visual-basic"></a>如何：在 Visual Basic 中移动文件

`My.Computer.FileSystem.MoveFile` 方法可以用于将文件移到另一个文件夹。 如果目标结构不存在，则将创建它。  
  
### <a name="to-move-a-file"></a>移动文件  
  
- 使用 `MoveFile` 方法来移动文件，并指定源文件和目标文件的文件名和位置。 本示例将名为 `test.txt` 的文件从 `TestDir1` 移动到 `TestDir2`。 请注意，即使目标文件名与源文件名相同，也要指定目标文件名。  
  
     [!code-vb[VbVbcnMyFileSystem#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#24)]  
  
### <a name="to-move-a-file-and-rename-it"></a>移动文件并对其重命名  
  
- 使用 `MoveFile` 方法来移动文件，指定源文件名和位置、目标位置以及在目标位置中的新名称。 本示例将名为 `test.txt` 的文件从 `TestDir1` 移动到 `TestDir2` 并将其重命名为 `nexttest.txt`。  
  
     [!code-vb[VbVbcnMyFileSystem#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#25)]  
  
## <a name="robust-programming"></a>可靠编程  

 以下情况可能会导致异常：  
  
- 路径由于以下原因之一而无效：是零长度字符串；仅为空白；包含无效字符；是一个设备路径（开头字符为 \\\\.\\）(<xref:System.ArgumentException>)。  
  
- 路径无效，因为它是 `Nothing` (<xref:System.ArgumentNullException>)。  
  
- `destinationFileName` 为 `Nothing` 或空字符串 (<xref:System.ArgumentNullException>)。  
  
- 源文件无效或不存在 (<xref:System.IO.FileNotFoundException>)。  
  
- 组合路径指向现有目录、目标文件已存在并且 `overwrite` 已设置为 `False`、具有相同名称的目标目录中的文件正在使用，或用户没有足够的权限访问该文件 (<xref:System.IO.IOException>)。  
  
- 路径中的文件名或目录名包含冒号 (:)，或格式无效 (<xref:System.NotSupportedException>)。  
  
- `showUI` 已设置为 `True`、 `onUserCancel` 已设置为 `ThrowException`，并且用户已取消该操作或发生了未指定的 I/O 错误 (<xref:System.OperationCanceledException>)。  
  
- 路径超过了系统定义的最大长度 (<xref:System.IO.PathTooLongException>)。  
  
- 该用户缺少查看该路径所必需的权限 (<xref:System.Security.SecurityException>)。  
  
- 用户没有必需的权限 (<xref:System.UnauthorizedAccessException>)。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.MoveFile%2A>
- [如何：重命名文件](how-to-rename-a-file.md)
- [如何：在不同的目录中创建文件的副本](how-to-create-a-copy-of-a-file-in-a-different-directory.md)
- [如何：分析文件路径](how-to-parse-file-paths.md)
