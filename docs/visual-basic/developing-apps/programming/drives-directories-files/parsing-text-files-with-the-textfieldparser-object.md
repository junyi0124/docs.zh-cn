---
title: 使用 TextFieldParser 对象分析文本文件
ms.date: 07/20/2015
helpviewer_keywords:
- TextFieldParser object, using
- I/O [Visual Basic], parsing files
- files [Visual Basic], parsing
ms.assetid: fc31d6e6-af0c-403f-8a00-d556b2c57567
ms.openlocfilehash: 7b2cb0dd39d14dd94db661cc9cbacf99edf0e778
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406672"
---
# <a name="parsing-text-files-with-the-textfieldparser-object-visual-basic"></a>使用 TextFieldParser 对象分析文本文件 (Visual Basic)

使用 `TextFieldParser` 对象可以分析和处理非常大的文件，这些文件的结构是以宽度分隔的文本列，如日志文件或旧版数据库信息。 使用 `TextFieldParser` 分析文本文件与循环访问文本文件相似，而提取文本字段的分析方法则与将分隔字符串标记化所使用的字符串操作方法相似。  
  
## <a name="parsing-different-types-of-text-files"></a>分析不同类型的文本文件  

 文本文件包含的字段可能有多种宽度，也可能使用字符（如逗号或制表符）分隔。 如以下示例所示，定义 `TextFieldType` 和分隔符，该示例使用 `SetDelimiters` 方法定义制表符分隔的文本文件：  
  
 [!code-vb[VbVbalrTextFieldParser#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTextFieldParser/VB/Class1.vb#21)]  
  
 其他文本文件可能包含固定宽度的字段。 在这种情况下，需要将 `TextFieldType` 定义为 `FixedWidth`，并定义各字段的宽度，如以下示例所示。 此示例使用 `SetFieldWidths` 方法定义文本列：第一列的宽度为 5 个字符，第二列的宽度为 10 个字符，第三列的宽度为 11 个字符，第四列的宽度是可变的。  
  
 [!code-vb[VbVbalrTextFieldParser#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrTextFieldParser/VB/Class1.vb#22)]  
  
 定义格式之后，即可使用 `ReadFields` 方法依次处理各行来循环访问文件。  
  
 如果字段与指定的格式不符，则会引发 <xref:Microsoft.VisualBasic.FileIO.MalformedLineException> 异常。 引发此类异常时，`ErrorLine` 和 `ErrorLineNumber` 属性将分别包含导致此异常的文本以及该文本所在的行号。  
  
## <a name="parsing-files-with-multiple-formats"></a>分析具有多种格式的文件  

 在读取各字段之前，可以使用 `TextFieldParser` 对象的 `PeekChars`方法来检查字段，这样可以为字段定义多种格式并采取相应的操作。 有关详细信息，请参阅[如何：读取具有多种格式的文本文件](how-to-read-from-text-files-with-multiple-formats.md)。  
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.FileIO.FileSystem.OpenTextFieldParser%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.PeekChars%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.ReadFields%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.CommentTokens%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.Delimiters%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.ErrorLine%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.ErrorLineNumber%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.FieldWidths%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.HasFieldsEnclosedInQuotes%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.LineNumber%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.TextFieldType%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.TrimWhiteSpace%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.SetDelimiters%2A>
- <xref:Microsoft.VisualBasic.FileIO.TextFieldParser.SetFieldWidths%2A>
