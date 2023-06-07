---
title: 如何：向文件内写入文本
ms.date: 07/20/2015
helpviewer_keywords:
- files [Visual Basic], writing to
- text, writing to files
- writing to files [Visual Basic]
- examples [Visual Basic], text files
ms.assetid: 304956eb-530d-4df7-b48f-9b4d1f2581a0
ms.openlocfilehash: f95a0c4df4a2eab0069a6dab0d4c3fa338d1d8ef
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411533"
---
# <a name="how-to-write-text-to-files-in-visual-basic"></a>如何：在 Visual Basic 中向文件内写入文本

可使用 <xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllText%2A> 方法向文件写入文本。 如果指定的文件不存在，则会创建一个。  
  
## <a name="procedure"></a>过程  
  
#### <a name="to-write-text-to-a-file"></a>向文件写入文本  
  
- 可以使用 `WriteAllText` 方法向文件写入文本，并指定要写入的文件和文本。 此示例将行 `"This is new text."` 写入名为 `test.txt` 的文件，同时将文本追加到文件中的任何现有文本。  
  
     [!code-vb[VbFileIOWrite#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOWrite/VB/Class1.vb#3)]  
  
#### <a name="to-write-a-series-of-strings-to-a-file"></a>向文件写入一系列字符串  
  
- 通过字符串集合循环。 可以使用 `WriteAllText` 方法向文件写入文本，同时指定目标文件以及要添加的字符串，并将 `append` 设置为 `True`。  
  
     此示例将 `Documents and Settings` 目录中的文件名写入 `FileList.txt`，并在每个文件之间插入回车符，以增强可读性。  
  
     [!code-vb[VbFileIOWrite#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOWrite/VB/Class1.vb#4)]  
  
## <a name="robust-programming"></a>可靠编程  

 以下情况可能会导致异常：  
  
- 路径由于以下原因之一而无效：是零长度字符串；仅为空白；包含无效字符；是一个设备路径（开头字符为 \\\\.\\）(<xref:System.ArgumentException>)。  
  
- 路径无效，因为它是 `Nothing` (<xref:System.ArgumentNullException>)。  
  
- `File` 指向不存在的路径（<xref:System.IO.FileNotFoundException> 或 <xref:System.IO.DirectoryNotFoundException>）。  
  
- 文件正由另一个进程使用，或者出现 I/O 错误 (<xref:System.IO.IOException>)。  
  
- 路径超过了系统定义的最大长度 (<xref:System.IO.PathTooLongException>)。  
  
- 路径中的文件名或目录名包含冒号 (:)，或格式无效 (<xref:System.NotSupportedException>)。  
  
- 该用户缺少查看该路径所必需的权限 (<xref:System.Security.SecurityException>)。  
  
- 磁盘已满，且对 `WriteAllText` 的调用失败 (<xref:System.IO.IOException>)。  
  
 如果在部分信任上下文中运行，该代码可能会因特权不足而引发异常。 有关详细信息，请参阅[代码访问安全性基础知识](../../../../framework/misc/code-access-security-basics.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileIO.FileSystem>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllText%2A>
- [如何：读取文本文件](how-to-read-from-text-files.md)
