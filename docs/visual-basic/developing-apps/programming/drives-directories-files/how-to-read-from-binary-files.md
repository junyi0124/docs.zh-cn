---
title: 如何：读取二进制文件
ms.date: 07/20/2015
helpviewer_keywords:
- binary files [Visual Basic], reading from
- I/O [Visual Basic], reading from binary files
- ReadAllBytes method [Visual Basic], reading from binary files
- My.Computer.FileSystem object, reading from binary files
ms.assetid: d2b1269e-24b6-42e0-9414-ae708db282d8
ms.openlocfilehash: 3b4108034b86d99143fff6943e68ca0077ebd21b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84359924"
---
# <a name="how-to-read-from-binary-files-in-visual-basic"></a>如何：在 Visual Basic 中读取二进制文件

`My.Computer.FileSystem` 对象提供用于读取二进制文件的 `ReadAllBytes` 方法。  
  
### <a name="to-read-from-a-binary-file"></a>读取二进制文件  
  
- 使用 `ReadAllBytes` 方法可将文件的内容作为字节数组返回。 此示例读取文件 `C:/Documents and Settings/selfportrait.jpg`。  
  
     [!code-vb[VbVbcnMyFileSystem#78](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#78)]  
  
- 对于大型二进制文件，可以使用 <xref:System.IO.FileStream.Read%2A> 对象的 <xref:System.IO.FileStream> 方法从文件中一次只读取指定数量的内容。 然后，可以限制每次读取操作将文件加载进内存的数量。 以下代码示例将复制文件，并允许调用方指定每次读取操作将文件读入内存的数量。  
  
     [!code-vb[VbVbcnMyFileSystem#91](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#91)]  
  
## <a name="robust-programming"></a>可靠编程  

 以下情况可能会导致异常：  
  
- 路径由于以下原因之一而无效：它是零长度字符串；它仅包含空白；它包含无效字符；或者它是一个设备路径 (<xref:System.ArgumentException>)。  
  
- 路径无效，因为它是 `Nothing` (<xref:System.ArgumentNullException>)。  
  
- 该文件不存在 (<xref:System.IO.FileNotFoundException>)。  
  
- 文件正由另一个进程使用，或者出现 I/O 错误 (<xref:System.IO.IOException>)。  
  
- 路径超过了系统定义的最大长度 (<xref:System.IO.PathTooLongException>)。  
  
- 路径中的文件名或目录名包含冒号 (:)，或格式无效 (<xref:System.NotSupportedException>)。  
  
- 内存不足，无法将字符串写入缓冲区 (<xref:System.OutOfMemoryException>)。  
  
- 该用户缺少查看该路径所必需的权限 (<xref:System.Security.SecurityException>)。  
  
 不要根据文件的名称来判断文件的内容。 例如，文件 Form1.vb 可能不是 Visual Basic 源文件。  
  
 在应用程序中使用输入的数据之前，需验证所有的输入内容。 文件的内容可能不是预期内容，并且用来读取该文件的方法可能失败。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.ReadAllBytes%2A>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllBytes%2A>
- [从文件读取](reading-from-files.md)
- [如何：读取具有多种格式的文本文件](how-to-read-from-text-files-with-multiple-formats.md)
- [将数据存储到剪贴板以及从剪贴板读取数据](../computer-resources/storing-data-to-and-reading-from-the-clipboard.md)
