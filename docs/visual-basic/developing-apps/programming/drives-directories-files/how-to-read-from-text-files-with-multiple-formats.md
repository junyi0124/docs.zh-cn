---
title: 如何：读取具有多种格式的文本文件
ms.date: 07/20/2015
helpviewer_keywords:
- TextFieldParser object, reading from a file
- TextFieldType enumeration
- My.Computer.FileSystem.WriteAllText method, parsing structured text files
- WriteAllText method [Visual Basic], parsing structured text files
- PeekChars method [Visual Basic], determining format of text
- reading text files [Visual Basic], multiple formats
- I/O [Visual Basic], reading text files
- text files [Visual Basic], reading
ms.assetid: 8d185eb2-79ca-42cd-95a7-d3ff44a5a0f8
ms.openlocfilehash: b36c781d5f8333749d346bb8f19540f0d1bd1692
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "74334570"
---
# <a name="how-to-read-from-fext-files-with-multiple-formats-in-visual-basic"></a>如何：在 Visual Basic 中读取具有多种格式的文本文件

<xref:Microsoft.VisualBasic.FileIO.TextFieldParser> 对象提供一种可以轻松而高效地分析结构化文本文件（如日志）的方法。 可以使用 `PeekChars` 方法处理具有多种格式的文件，以便在分析整个文件时确定每行的格式。
  
### <a name="to-parse-a-text-file-with-multiple-formats"></a>分析具有多种格式的文本文件

1. 将名为 testfile.txt  的文本文件添加到你的项目中。 向文本文件添加以下内容：

    ```text
    Err  1001 Cannot access resource.
    Err  2014 Resource not found.
    Acc  10/03/2009User1      Administrator.
    Err  0323 Warning: Invalid access attempt.
    Acc  10/03/2009User2      Standard user.
    Acc  10/04/2009User2      Standard user.
    ```

2. 定义所需格式和报告错误时使用的格式。 每个数组中的最后一项为 -1，因此假定最后一个字段为可变宽度。 数组中的最后一项小于或等于 0 时，会出现这种情况。

     [!code-vb[VbFileIORead#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#4)]

3. 创建新的 <xref:Microsoft.VisualBasic.FileIO.TextFieldParser> 对象，定义宽度和格式。

     [!code-vb[VbFileIORead#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#5)]

4. 依次通过各行，并在读取之前测试格式。

     [!code-vb[VbFileIORead#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#6)]

5. 将错误写入控制台。

     [!code-vb[VbFileIORead#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#7)]

## <a name="example"></a>示例

以下是从文件 `testfile.txt` 中读取的完整示例：

 [!code-vb[VbFileIORead#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#8)]

## <a name="robust-programming"></a>可靠编程

以下情况可能会导致异常：  
  
- 无法使用指定的格式分析行 (<xref:Microsoft.VisualBasic.FileIO.MalformedLineException>)。 此异常消息指定导致发生异常的行，同时将 <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.ErrorLine%2A> 属性分配给该行中包含的文本。
- 指定的文件不存在 (<xref:System.IO.FileNotFoundException>)。
- 在部分信任的情况下，用户没有足够的权限访问文件。 (<xref:System.Security.SecurityException>).
- 路径过长 (<xref:System.IO.PathTooLongException>)。
- 用户没有足够的权限访问文件 (<xref:System.UnauthorizedAccessException>)。

## <a name="see-also"></a>请参阅

- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.PeekChars%2A>
- <xref:Microsoft.VisualBasic.FileIO.MalformedLineException>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllText%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.EndOfData%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.TextFieldType%2A>
- [如何：读取逗号分隔的文本文件](how-to-read-from-comma-delimited-text-files.md)
- [如何：读取固定宽度的文本文件](how-to-read-from-fixed-width-text-files.md)
- [使用 TextFieldParser 对象分析文本文件](parsing-text-files-with-the-textfieldparser-object.md)
