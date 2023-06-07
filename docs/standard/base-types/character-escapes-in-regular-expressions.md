---
title: .NET 正则表达式中的字符转义
description: 了解 .NET 正则表达式中的特殊字符和转义字符。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- unescaped characters
- replacement patterns
- characters, escapes
- regular expressions, character escapes
- escape characters
- .NET regular expressions, character escapes
- constructs, character escapes
ms.assetid: f49cc9cc-db7d-4058-8b8a-422bc08b29b0
ms.openlocfilehash: 820e6cd7fa4a60fa6adfcaf0f0ff4d25fdda0f21
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734382"
---
# <a name="character-escapes-in-regular-expressions"></a>正则表达式中的字符转义

正则表达式中的反斜杠 (\\) 指示以下值之一：  
  
- 后接字符为特殊字符，如下节表中所示。 例如，`\b` 是定位标记，用于指示正则表达式的匹配应从单词边界开始，`\t` 表示制表符，而 `\x020` 表示空格。  
  
- 本应解释为未转义语言构造的字符应按字面意思进行解释。 例如，大括号 (`{`) 开始定义限定符，而反斜杠后接大括号 (`\{`) 表示正则表达式引擎应匹配大括号。 同样，单个反斜杠标记转义的语言构造的开始，而两个反斜杠 (`\\`) 表示正则表达式引擎应匹配反斜杠。  
  
> [!NOTE]
> 字符转义可在正则表达式模式中识别，但无法在替换模式中识别。  
  
## <a name="character-escapes-in-net"></a>.NET 中的字符转义  

 下表列出了 .NET 中正则表达式支持的字符转义。  
  
|字符或序列|描述|  
|---------------------------|-----------------|  
|除以下字符外的所有字符：<br /><br /> 。 $ ^ { [ ( &#124; ) * + ? \ |“字符或序列”  列中未包含的字符在正则表达式中没有特殊含义；此类字符与自身匹配。<br /><br /> “字符或序列”  列中包括的字符均为特殊的正则表达式语言元素。 若要在正则表达式中匹配这些字符，必须将其转义或纳入[正字符组](character-classes-in-regular-expressions.md)。 例如，正则表达式 `\$\d+` 或 `[$]\d+` 匹配“$1200”。|  
|`\a`|匹配响铃（警报）字符，`\u0007`。|  
|`\b`|在 `[`character_group  `]` 字符类中，匹配退格，`\u0008`。  （请参阅[字符类](character-classes-in-regular-expressions.md)。）在字符类之外，`\b` 是匹配字边界的定位点。 （请参阅[定位标记](anchors-in-regular-expressions.md)）|  
|`\t`|匹配制表符，`\u0009`。|  
|`\r`|匹配回车，`\u000D`。 请注意，`\r` 不等同于换行符，`\n`。|  
|`\v`|匹配垂直制表符，`\u000B`。|  
|`\f`|匹配换页，`\u000C`。|  
|`\n`|匹配换行，`\u000A`。|  
|`\e`|匹配转义，`\u001B`。|  
|`\` *nnn*|匹配 ASCII 字符，其中 nnn  包含表示八进制字符代码的两位数或三位数。 例如，`\040` 表示空格字符。 如果此构造仅包含一个数字（如 `\2`）或者它对应捕获组的编号，则将它解释为向后引用。 （请参阅[向后引用构造](backreference-constructs-in-regular-expressions.md)。）|  
|`\x` *nn*|匹配 ASCII 字符，其中 nn  是两位数的十六进制字符代码。|  
|`\c` *X*|匹配 ASCII 控制字符，其中 X 是控制字符的字母。 例如，`\cC` 为 CTRL-C。|  
|`\u` *nnnn*|匹配的 UTF-16 代码单元，单元值是 nnnn  十六进制。 **注意：** .NET 不支持用于指定 Unicode 的 Perl 5 字符转义。 Perl 5 字符转义采用以下格式 `\x{`####  `…}`，其中 ####  `…` 是一系列十六进制数字。 改用 `\u`nnnn  。|  
|`\`|后接字符未识别为转义字符时，将匹配此字符。 例如，`\*` 匹配星号 (*) 并等同于 `\x2A`。|  
  
## <a name="an-example"></a>示例  

 以下示例说明了如何使用正则表达式中的字符转义。 分析了包含 2009 年世界上最大城市的名称及其人口的字符串。 使用制表符 (`\t`) 或垂直条（&#124; 或 `\u007c`）将每个城市名与其人口数量分开。 使用回车符和换行符分隔各个城市及其人口。  
  
 [!code-csharp[RegularExpressions.Language.Escapes#1](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.escapes/cs/escape1.cs#1)]
 [!code-vb[RegularExpressions.Language.Escapes#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.escapes/vb/escape1.vb#1)]  
  
 正则表达式 `\G(.+)[\t\u007c](.+)\r?\n` 可以解释为下表中所示内容。  
  
|模式|描述|  
|-------------|-----------------|  
|`\G`|从上次匹配结束处开始匹配。|  
|`(.+)`|一次或多次匹配任何字符。 这是第一个捕获组。|  
|`[\t\u007c]`|匹配制表符 (`\t`) 或垂直条 (&#124;)。|  
|`(.+)`|一次或多次匹配任何字符。 这是第二个捕获组。|  
|`\r?\n`|匹配零次或一次回车符后接新行的情况。|  
  
## <a name="see-also"></a>请参阅

- [正则表达式语言 - 快速参考](regular-expression-language-quick-reference.md)
