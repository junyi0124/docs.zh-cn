---
title: 如何：读取固定宽度的文本文件
ms.date: 07/20/2015
helpviewer_keywords:
- fixed-width text file
- reading text files [Visual Basic], fixed-width
- files [Visual Basic], parsing
- text files [Visual Basic], tasks
- text files [Visual Basic], reading
ms.assetid: 99be5692-967a-4e85-993e-cd18139a5a69
ms.openlocfilehash: 77b2e0a4ebe36b68501f821ef5731935ee3b16a7
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411624"
---
# <a name="how-to-read-from-fixed-width-text-files-in-visual-basic"></a>如何：在 Visual Basic 中读取定宽文本文件

`TextFieldParser` 对象提供一种可以轻松而高效地分析结构化文本文件（如日志）的方法。  
  
 `TextFieldType` 属性定义分析的文件是带分隔符的文件还是具有定宽文本字段的文件。 在定宽文本文件中，结尾处的字段可以具有可变宽度。 若要指定结尾处的字段具有可变宽度，请将它定义为宽度小于或等于零。  
  
### <a name="to-parse-a-fixed-width-text-file"></a>分析定宽文本文件  
  
1. 创建一个新的 `TextFieldParser`。 下面的代码创建名为 `Reader` 的 `TextFieldParser`，并打开 `test.log` 文件。  
  
     [!code-vb[VbFileIORead#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#9)]  
  
2. 将 `TextFieldType` 属性定义为 `FixedWidth`（定义宽度和格式）。 下面的代码定义文本的各列；第一列宽度为 5 个字符，第二列宽度为 10 个字符，第三列宽度为 11 个字符，而第四列宽度可变。  
  
     [!code-vb[VbFileIORead#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#10)]  
  
3. 循环访问文件中的各个字段。 如果有任何行损坏，则报告错误，然后继续分析。  
  
     [!code-vb[VbFileIORead#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#11)]  
  
4. 用 `End While` 和 `End Using` 结束 `While` 和 `Using` 块。  
  
     [!code-vb[VbFileIORead#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#12)]  
  
## <a name="example"></a>示例  

 此示例读取文件 `test.log`。  
  
 [!code-vb[VbFileIORead#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#13)]  
  
## <a name="robust-programming"></a>可靠编程  

 以下情况可能会导致异常：  
  
- 无法使用指定的格式 (<xref:Microsoft.VisualBasic.FileIO.MalformedLineException>) 分析行。 此异常消息指定导致发生异常的行，同时将 <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.ErrorLine%2A> 属性分配给该行中包含的文本。  
  
- 指定的文件不存在 (<xref:System.IO.FileNotFoundException>)。  
  
- 在部分信任的情况下，用户没有足够的权限访问文件。 (<xref:System.Security.SecurityException>).  
  
- 路径过长 (<xref:System.IO.PathTooLongException>)。  
  
- 用户没有足够的权限访问文件 (<xref:System.UnauthorizedAccessException>)。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser?displayProperty=nameWithType>
- [如何：读取逗号分隔的文本文件](how-to-read-from-comma-delimited-text-files.md)
- [如何：读取具有多种格式的文本文件](how-to-read-from-text-files-with-multiple-formats.md)
- [使用 TextFieldParser 对象分析文本文件](parsing-text-files-with-the-textfieldparser-object.md)
- [演练：在 Visual Basic 中操作文件和目录](walkthrough-manipulating-files-and-directories.md)
- [疑难解答：读取和写入文本文件](troubleshooting-reading-from-and-writing-to-text-files.md)
