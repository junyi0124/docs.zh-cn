---
title: 如何：查找具有特定模式的文件
ms.date: 07/20/2015
helpviewer_keywords:
- files [Visual Basic], finding
- pattern matching
- patterns, matching
ms.assetid: 25e3b71d-b844-4293-9e4e-f06c5836b5cc
ms.openlocfilehash: 71073672ed14cb2d5df5b5365266b718c59cb18f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401636"
---
# <a name="how-to-find-files-with-a-specific-pattern-in-visual-basic"></a>如何：在 Visual Basic 中查找具有特定模式的文件

<xref:Microsoft.VisualBasic.MyServices.FileSystemProxy.GetFiles%2A> 方法返回表示文件的路径名的只读字符串集合。 可以使用 `wildCards` 参数来指定特定模式。 若要在搜索中包括子目录，请将 `searchType` 参数设置为 `SearchOption.SearchAllSubDirectories`。  
  
 如果没有找到与指定模式匹配的文件，则返回一个空集合。  
  
> [!NOTE]
> 有关使用 `System.IO` 命名空间的 `DirectoryInfo` 类返回文件列表的信息，请参阅 <xref:System.IO.DirectoryInfo.GetFiles%2A>。  
  
### <a name="to-find-files-with-a-specified-pattern"></a>查找具有指定模式的文件  
  
- 使用 `GetFiles` 方法，同时提供要搜索的目录的名称和路径并指定模式。 以下示例返回目录中扩展名为 `.dll` 的所有文件，并将其添加到 `ListBox1`。  
  
     [!code-vb[VbFileIOMisc#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOMisc/VB/Class1.vb#4)]  
  
## <a name="net-framework-security"></a>.NET Framework 安全性  

 以下情况可能会导致异常：  
  
- 路径由于以下原因之一而无效：是零长度字符串；仅为空白；包含无效字符；是一个设备路径（开头字符为 \\\\.\\）(<xref:System.ArgumentException>)。  
  
- 路径无效，因为它是 `Nothing` (<xref:System.ArgumentNullException>)。  
  
- `directory` 不存在 (<xref:System.IO.DirectoryNotFoundException>)。  
  
- `directory` 指向某个现有文件 (<xref:System.IO.IOException>)。  
  
- 路径超过了系统定义的最大长度 (<xref:System.IO.PathTooLongException>)。  
  
- 路径中的文件名或文件夹名包含冒号 (:)，或其格式无效 (<xref:System.NotSupportedException>)。  
  
- 该用户缺少查看该路径所必需的权限 (<xref:System.Security.SecurityException>)。  
  
- 该用户缺少必要的权限 (<xref:System.UnauthorizedAccessException>)。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.MyServices.FileSystemProxy.GetFiles%2A>
- [如何：查找具有特定模式的子目录](how-to-find-subdirectories-with-a-specific-pattern.md)
- [疑难解答：读取和写入文本文件](troubleshooting-reading-from-and-writing-to-text-files.md)
- [如何：获取目录中的文件集合](how-to-get-the-collection-of-files-in-a-directory.md)
