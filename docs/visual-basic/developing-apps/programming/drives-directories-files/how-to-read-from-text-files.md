---
title: 如何：读取文本文件
ms.date: 07/20/2015
helpviewer_keywords:
- extended characters [Visual Basic], reading
- reading text files [Visual Basic]
- reading data, text files
- examples [Visual Basic], reading text files
- text files [Visual Basic], reading
ms.assetid: 735fe9d7-0f7a-4185-ba02-f35e580ec4b8
ms.openlocfilehash: 0d99209ed123686355e8d49c82ba23f94084f895
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411611"
---
# <a name="how-to-read-from-text-files-in-visual-basic"></a>如何：在 Visual Basic 中读取文本文件

通过 <xref:Microsoft.VisualBasic.MyServices.FileSystemProxy.ReadAllText%2A> 对象的 `My.Computer.FileSystem` 方法，可以读取文本文件。 如果文件的内容使用类似 ASCII 或 UTF-8 的编码，则可以指定文件编码。

若要读取包含扩展字符的文件，则需要指定文件编码。

> [!NOTE]
> 若要以一次读取一行文本的方式读取文件，请使用 <xref:Microsoft.VisualBasic.MyServices.FileSystemProxy.OpenTextFileReader%2A> 对象的 `My.Computer.FileSystem` 方法。 `OpenTextFileReader` 方法返回 <xref:System.IO.StreamReader> 对象。 可以使用 <xref:System.IO.StreamReader.ReadLine%2A> 对象的 `StreamReader` 方法以一次读取一行的方式读取文件。 可以使用 <xref:System.IO.StreamReader.EndOfStream%2A> 对象的 `StreamReader` 方法测试文件的结尾。

## <a name="to-read-from-a-text-file"></a>读取文本文件

使用 `ReadAllText` 对象的 `My.Computer.FileSystem` 方法并提供路径，可以将文本文件的内容读入字符串中。 下面的示例将 test.txt 的内容读入字符串中，然后在消息框中显示内容。

[!code-vb[VbFileIORead#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#2)]

### <a name="to-read-from-a-text-file-that-is-encoded"></a>读取已编码的文本文件

使用 `ReadAllText` 对象的 `My.Computer.FileSystem` 方法并提供路径和文件编码类型，可以将文本文件的内容读入字符串中。 下面的示例将 UTF32 文件 test.txt 的内容读入字符串中，然后在消息框中显示内容。

[!code-vb[VbFileIORead#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#3)]

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

## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.FileIO.FileSystem>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.ReadAllText%2A>
- [从文件读取](reading-from-files.md)
- [如何：读取逗号分隔的文本文件](how-to-read-from-comma-delimited-text-files.md)
- [如何：读取固定宽度的文本文件](how-to-read-from-fixed-width-text-files.md)
- [如何：读取具有多种格式的文本文件](how-to-read-from-text-files-with-multiple-formats.md)
- [排除故障：读取和写入文本文件](troubleshooting-reading-from-and-writing-to-text-files.md)
- [演练：在 Visual Basic 中操作文件和目录](walkthrough-manipulating-files-and-directories.md)
- [文件编码](file-encodings.md)
