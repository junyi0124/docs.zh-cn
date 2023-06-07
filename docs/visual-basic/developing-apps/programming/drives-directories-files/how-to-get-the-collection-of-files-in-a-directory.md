---
title: 如何：获取目录中的文件集合
ms.date: 07/20/2015
helpviewer_keywords:
- folders, working with
- files [Visual Basic], accessing
ms.assetid: 6c8ba7e8-dd37-4853-92bf-762b67c98160
ms.openlocfilehash: 055151d4b3e3aba6be9b9b03ac9abffc6eb7b734
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401610"
---
# <a name="how-to-get-the-collection-of-files-in-a-directory-in-visual-basic"></a>如何：在 Visual Basic 中获取目录中的文件集合

<xref:Microsoft.VisualBasic.FileIO.FileSystem.GetFiles%2A?displayProperty=nameWithType> 方法的重载返回字符串的只读集合，这些字符串表示目录内文件的名称：  
  
- 将 <xref:Microsoft.VisualBasic.FileIO.FileSystem.GetFiles%28System.String%29> 重载用于指定目录中的简单文件搜索，而无需搜索子目录。  
  
- 使用 <xref:Microsoft.VisualBasic.FileIO.FileSystem.GetFiles(System.String,Microsoft.VisualBasic.FileIO.SearchOption,System.String[])> 重载指定搜索的其他选项。 可以使用 `wildCards` 参数来指定搜索模式。 若要在搜索中包括子目录，请将 `searchType` 参数设置为 <xref:Microsoft.VisualBasic.FileIO.SearchOption.SearchAllSubDirectories?displayProperty=nameWithType>。  
  
 如果没有找到与指定模式匹配的文件，则返回一个空集合。  
  
### <a name="to-list-files-in-a-directory"></a>列出目录中的文件  
  
- 使用其中一个 <xref:Microsoft.VisualBasic.FileIO.FileSystem.GetFiles%2A?displayProperty=nameWithType> 方法重载，向 `directory` 参数中的搜索提供目录的名称和路径。 下面的示例返回目录中的所有文件，并将它们添加到 `ListBox1`。  
  
     [!code-vb[VbVbcnMyFileSystem#32](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#32)]  
  
## <a name="robust-programming"></a>可靠编程  

 以下情况可能会导致异常：  
  
- 路径由于以下原因之一而无效：是零长度字符串；仅为空白；包含无效字符；是一个设备路径（开头字符为 \\\\.\\）(<xref:System.ArgumentException>)。  
  
- 路径无效，因为它是 `Nothing` (<xref:System.ArgumentNullException>)。  
  
- `directory` 不存在 (<xref:System.IO.DirectoryNotFoundException>)。  
  
- `directory` 指向某个现有文件 (<xref:System.IO.IOException>)。  
  
- 路径超过了系统定义的最大长度 (<xref:System.IO.PathTooLongException>)。  
  
- 路径中的文件名或目录名包含冒号 (:)，或格式无效 (<xref:System.NotSupportedException>)。  
  
- 该用户缺少查看该路径所必需的权限 (<xref:System.Security.SecurityException>)。  
  
- 该用户缺少必要的权限 (<xref:System.UnauthorizedAccessException>)。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.GetFiles%2A>
- [如何：查找具有特定模式的文件](how-to-find-files-with-a-specific-pattern.md)
- [如何：查找具有特定模式的子目录](how-to-find-subdirectories-with-a-specific-pattern.md)
