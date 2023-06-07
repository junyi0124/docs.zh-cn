---
title: 如何：删除文件
ms.date: 07/20/2015
helpviewer_keywords:
- Delete method [Visual Basic]
- files [Visual Basic], deleting
- files [Visual Basic], manipulating
- File object
ms.assetid: 4b721769-3e45-4be7-b7fe-b08dc4141b44
ms.openlocfilehash: 0c8213786b8073d784f1f3ea51417741d5ad4cba
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401649"
---
# <a name="how-to-delete-a-file-in-visual-basic"></a>如何：在 Visual Basic 中删除文件

通过 `My.Computer.FileSystem` 对象的 `DeleteFile` 方法，可以删除文件。 它提供的选项包括：是否将已删除的文件发送到“回收站”  、是否要求用户确认应删除该文件，以及当用户取消操作时要执行的操作。  
  
### <a name="to-delete-a-text-file"></a>删除文本文件  
  
- 使用 `DeleteFile` 方法删除文件。 以下代码演示如何删除名为 `test.txt` 的文件。  
  
     [!code-vb[VbVbcnMyFileSystem#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#22)]  
  
### <a name="to-delete-a-text-file-and-ask-the-user-to-confirm-that-the-file-should-be-deleted"></a>删除文本文件，并要求用户确认应删除该文件  
  
- 使用 `DeleteFile` 方法删除文件，同时将 `showUI` 设置为 `AllDialogs`。 以下代码演示如何删除名为 `test.txt` 的文件，并允许用户确认是否应删除该文件。  
  
     [!code-vb[VbFileIOMisc#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOMisc/VB/Class1.vb#9)]  
  
### <a name="to-delete-a-text-file-and-send-it-to-the-recycle-bin"></a>删除文本文件并将其发送到“回收站”  
  
- 使用 `DeleteFile` 方法删除文件，并为 `recycle` 参数指定 `SendToRecycleBin`。 以下代码演示如何删除名为 `test.txt` 的文件并将其发送到“回收站”  。  
  
     [!code-vb[VbFileIOMisc#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOMisc/VB/Class1.vb#10)]  
  
## <a name="robust-programming"></a>可靠编程  

 以下情况可能会导致异常：  
  
- 路径由于以下原因之一而无效：是零长度字符串；仅为空白；包含无效字符；是一个设备路径（开头字符为 \\\\.\\）(<xref:System.ArgumentException>)。  
  
- 路径无效，因为它是 `Nothing` (<xref:System.ArgumentNullException>)。  
  
- 路径超过了系统定义的最大长度 (<xref:System.IO.PathTooLongException>)。  
  
- 路径中的文件名或文件夹名包含冒号 (:)，或其格式无效 (<xref:System.NotSupportedException>)。  
  
- 该文件正在使用中 (<xref:System.IO.IOException>)。  
  
- 该用户缺少查看该路径所必需的权限 (<xref:System.Security.SecurityException>)。  
  
- 该文件不存在 (<xref:System.IO.FileNotFoundException>)。  
  
- 用户无权删除文件，或文件为只读 (<xref:System.UnauthorizedAccessException>)。  
  
- 存在部分信任的情况，在此情况下用户没有足够的权限 (<xref:System.Security.SecurityException>)。  
  
- 用户取消了该操作，并且 `onUserCancel` 设置为 `ThrowException` (<xref:System.OperationCanceledException>)。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileIO.UICancelOption>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem>
- <xref:Microsoft.VisualBasic.FileIO.UIOption>
- <xref:Microsoft.VisualBasic.FileIO.RecycleOption>
- [如何：获取目录中的文件集合](how-to-get-the-collection-of-files-in-a-directory.md)
