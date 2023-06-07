---
title: 如何：从字符串中剥离无效字符
description: 阅读示例，了解如何使用静态 Regex.Replace 方法剥离字符串中可能有害的字符。
ms.date: 06/30/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- regular expressions, examples
- cleaning input
- user input, examples
- .NET regular expressions, examples
- regular expressions [.NET], examples
- Regex.Replace method
- stripping invalid characters
- Replace method
- validating user input
ms.assetid: b4319c8a-9032-4129-a9d5-6f6fc28e7f32
ms.openlocfilehash: d6422556ab9c7d2100ea66e6b0dae1ee01e0e434
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95683844"
---
# <a name="how-to-strip-invalid-characters-from-a-string"></a>如何：从字符串中剥离无效字符

下面的示例使用静态 <xref:System.Text.RegularExpressions.Regex.Replace%2A?displayProperty=nameWithType> 方法，从字符串中剥离无效字符。  

[!INCLUDE [regex](../../../includes/regex.md)]

## <a name="example"></a>示例  

 可以使用此示例中定义的 `CleanInput` 方法来剥离在接受用户输入的文本字段中输入的可能有害的字符。 在此情况下，`CleanInput` 会剥离所有非字母数字字符（句点 (.)、at 符号 (@) 和连字符 (-) 除外），并返回剩余字符串。 但是，可以修改正则表达式模式，使其剥离不应包含在输入字符串内的所有字符。  
  
 [!code-csharp[RegularExpressions.Examples.StripChars#1](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Examples.StripChars/cs/Example.cs#1)]
 [!code-vb[RegularExpressions.Examples.StripChars#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Examples.StripChars/vb/Example.vb#1)]  
  
 正则表达式模式 `[^\w\.@-]` 与非单词字符、句点、@ 符号或连字符的任何字符相匹配。 单词字符可以是任何字母、十进制数字或标点连接符（如下划线符号）。 与此模式匹配的任何字符被替换为 <xref:System.String.Empty?displayProperty=nameWithType>（即替换模式定义的字符串）。 若要允许用户输入中出现其他字符，请将该字符添加到正则表达式模式中的字符类。 例如，正则表达式模式 `[^\w\.@-\\%]` 还允许输入字符串中包含百分号和反斜杠。  
  
## <a name="see-also"></a>请参阅

- [.NET 正则表达式](regular-expressions.md)
