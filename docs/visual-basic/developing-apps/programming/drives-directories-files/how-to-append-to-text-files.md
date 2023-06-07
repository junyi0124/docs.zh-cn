---
title: 如何：向文本文件追加内容
ms.date: 07/20/2015
helpviewer_keywords:
- I/O [Visual Basic], appending to files
- I/O [Visual Basic], My.Computer.FileSystem.WriteAllText method
- I/O [Visual Basic], WriteAllText method
ms.assetid: bbbd7fb5-f169-41a9-b53f-520ea9613913
ms.openlocfilehash: 53b2e4c9030e9d1dd81a4121ad3f92aad796e3e1
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84359950"
---
# <a name="how-to-append-to-text-files-in-visual-basic"></a>如何：在 Visual Basic 中向文本文件追加内容

通过指定将 `append` 参数设置为 `True`，可使用 <xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllText%2A> 方法向文本文件追加内容。  
  
### <a name="to-append-to-a-text-file"></a>向文本文件追加内容  
  
- 使用 `WriteAllText` 方法，指定目标文件以及要追加的字符串，并将 `append` 参数设置为 `True`。  
  
     此示例将字符串 `"This is a test string."` 写入名为 `Testfile.txt` 的文件中。  
  
     [!code-vb[VbFileIOWrite#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOWrite/VB/Class1.vb#6)]  
  
## <a name="robust-programming"></a>可靠编程  

 以下情况可能会导致异常：  
  
- 路径由于以下原因之一而无效：是零长度字符串；仅为空白；包含无效字符；是一个设备路径（开头字符为 \\\\.\\）(<xref:System.ArgumentException>)。  
  
- 路径无效，因为它是 `Nothing` (<xref:System.ArgumentNullException>)。  
  
- `File` 指向不存在的路径（<xref:System.IO.FileNotFoundException> 或 <xref:System.IO.DirectoryNotFoundException>）。  
  
- 文件正由另一个进程使用，或者出现 I/O 错误 (<xref:System.IO.IOException>)。  
  
- 路径超过了系统定义的最大长度 (<xref:System.IO.PathTooLongException>)。  
  
- 路径中的文件名或目录名包含冒号 (:)，或格式无效 (<xref:System.NotSupportedException>)。  
  
- 该用户缺少查看该路径所必需的权限 (<xref:System.Security.SecurityException>)。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllText%2A>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem>
- [写入文件](writing-to-files.md)
