---
title: 如何：将目录复制到另一个目录
ms.date: 07/20/2015
helpviewer_keywords:
- I/O [Visual Basic], copying directories
- I/O [Visual Basic], copying folders
- folders [Visual Basic], copying
- directories [Visual Basic], copying
ms.assetid: 2a370bd7-10ba-4219-afc4-4519d031eb6c
ms.openlocfilehash: e28f50f6a812188ac7af801cea691818488bd6cd
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401714"
---
# <a name="how-to-copy-a-directory-to-another-directory-in-visual-basic"></a>如何：在 Visual Basic 中将一个目录复制到另一个目录

使用 <xref:Microsoft.VisualBasic.FileIO.FileSystem.CopyDirectory%2A> 方法将一个目录复制到另一个目录。 此方法复制目录的内容以及目录本身。 如果目标目录不存在，则将创建它。 如果目标位置中存在具有相同名称的目录，并且 `overwrite` 设置为 `False`，则将合并这两个目录的内容。 操作期间可为此目录指定新名称。

复制目录中的文件时，可能会因特定文件引发异常，例如将 `overwrite` 设为 `False`，在合并期间存在的文件。 如果引发此类异常，那么这些异常将合并为一个异常，其 `Data` 属性保存的条目中文件或目录路径为键，特定的异常消息包含在对应的值中。

## <a name="to-copy-a-directory-to-another-directory"></a>将目录复制到另一个目录

- 使用 `CopyDirectory` 方法指定源和目标目录名称。 下面的示例将名为 `TestDirectory1` 的目录复制到 `TestDirectory2`，并覆盖现有文件。

    [!code-vb[VbVbcnMyFileSystem#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#16)]

    此代码示例也可作为 IntelliSense 代码片段。 在代码片段选取器中，该代码段位于“文件系统 - 处理驱动器、文件夹和文件”。  有关详细信息，请参阅[代码片段](/visualstudio/ide/code-snippets)。

## <a name="robust-programming"></a>可靠编程

以下情况可能会导致异常：

- 为目录指定的新名称包含冒号 (:) 和斜杠（\ 或 /）(<xref:System.ArgumentException>)。

- 路径由于以下原因之一而无效：是零长度字符串；仅为空白；包含无效字符；是一个设备路径（开头字符为 \\\\.\\）(<xref:System.ArgumentException>)。

- 路径无效，因为它是 `Nothing` (<xref:System.ArgumentNullException>)。

- `destinationDirectoryName` 为 `Nothing` 或空字符串 (<xref:System.ArgumentNullException>)

- 源目录不存在 (<xref:System.IO.DirectoryNotFoundException>)。

- 源目录是一个根目录 (<xref:System.IO.IOException>)。

- 合并路径指向现有文件 (<xref:System.IO.IOException>)。

- 源路径和目标路径相同 (<xref:System.IO.IOException>)。

- `ShowUI` 设为 `UIOption.AllDialogs`，并且用户取消了操作，或不能复制目录中的一个或多个文件 (<xref:System.OperationCanceledException>)。

- 操作是循环的 (<xref:System.InvalidOperationException>)。

- 路径包含一个冒号 (:) (<xref:System.NotSupportedException>)。

- 路径超过了系统定义的最大长度 (<xref:System.IO.PathTooLongException>)。

- 路径中的文件名或文件夹名包含冒号 (:)，或其格式无效 (<xref:System.NotSupportedException>)。

- 该用户缺少查看该路径所必需的权限 (<xref:System.Security.SecurityException>)。

- 目标文件存在，但不能访问 (<xref:System.UnauthorizedAccessException>)。

## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.CopyDirectory%2A>
- [如何：查找具有特定模式的子目录](how-to-find-subdirectories-with-a-specific-pattern.md)
- [如何：获取目录中的文件集合](how-to-get-the-collection-of-files-in-a-directory.md)
