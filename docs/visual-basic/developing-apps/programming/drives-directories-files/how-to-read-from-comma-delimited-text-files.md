---
title: 如何：读取逗号分隔的文本文件
ms.date: 07/20/2015
helpviewer_keywords:
- files [Visual Basic], parsing
- text files [Visual Basic], tasks
- reading text files [Visual Basic], comma-delimited
- text files [Visual Basic], reading
ms.assetid: a8413fe4-0dba-49c8-8692-44fb67a9ec4f
ms.openlocfilehash: ba62a0226a1a83326cbc7ab393d135763a7c7cb2
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84411637"
---
# <a name="how-to-read-from-comma-delimited-text-files-in-visual-basic"></a>如何：在 Visual Basic 中读取逗号分隔的文本文件

`TextFieldParser` 对象提供一种可以轻松而高效地分析结构化文本文件（如日志）的方法。 `TextFieldType` 属性用于定义文件是带分隔符的文件还是具有固定宽度文本字段的文件。  
  
### <a name="to-parse-a-comma-delimited-text-file"></a>分析逗号分隔的文本文件  
  
1. 创建一个新的 `TextFieldParser`。 下面的代码创建名为 `MyReader` 的 `TextFieldParser`，并打开 `test.txt` 文件。  
  
     [!code-vb[VbFileIORead#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#15)]  
  
2. 定义 `TextField` 类型和分隔符。 下面的代码将 `TextFieldType` 属性定义为 `Delimited`，并将分隔符定义为“,”。  
  
     [!code-vb[VbFileIORead#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#16)]  
  
3. 循环访问文件中的各个字段。 如果遇到任何损坏的行，则报告错误，然后继续分析。 下面的代码循环访问该文件，依次显示各字段并报告所有格式不正确的字段。  
  
     [!code-vb[VbFileIORead#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#17)]  
  
4. 用 `End While` 和 `End Using` 结束 `While` 和 `Using` 块。  
  
     [!code-vb[VbFileIORead#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#18)]  
  
## <a name="example"></a>示例  

 此示例读取文件 `test.txt`。  
  
 [!code-vb[VbFileIORead#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#19)]  
  
## <a name="robust-programming"></a>可靠编程  

 以下情况可能会导致异常：  
  
- 无法使用指定的格式 (<xref:Microsoft.VisualBasic.FileIO.MalformedLineException>) 分析行。 此异常消息指定导致发生异常的行，同时将 <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.ErrorLine%2A> 属性分配给该行中包含的文本。  
  
- 指定的文件不存在 (<xref:System.IO.FileNotFoundException>)。  
  
- 在部分信任的情况下，用户没有足够的权限访问文件。 (<xref:System.Security.SecurityException>).  
  
- 路径过长 (<xref:System.IO.PathTooLongException>)。  
  
- 用户没有足够的权限访问文件 (<xref:System.UnauthorizedAccessException>)。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser?displayProperty=nameWithType>
- [如何：读取固定宽度的文本文件](how-to-read-from-fixed-width-text-files.md)
- [如何：读取具有多种格式的文本文件](how-to-read-from-text-files-with-multiple-formats.md)
- [使用 TextFieldParser 对象分析文本文件](parsing-text-files-with-the-textfieldparser-object.md)
- [演练：在 Visual Basic 中操作文件和目录](walkthrough-manipulating-files-and-directories.md)
- [疑难解答：读取和写入文本文件](troubleshooting-reading-from-and-writing-to-text-files.md)
