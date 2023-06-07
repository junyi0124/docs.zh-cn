---
title: 正则表达式中的其他构造
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- constructs, miscellaneous
- .NET regular expressions, miscellaneous constructs
- regular expressions, miscellaneous constructs
ms.assetid: 7d10d11f-680f-4721-b047-fb136316b4cd
ms.openlocfilehash: ef8f126559b51aa9b918f04e27f5599fba519b43
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95683792"
---
# <a name="miscellaneous-constructs-in-regular-expressions"></a>正则表达式中的其他构造

.NET 中的正则表达式包括三个其他语言构造。 其中一个使你可以在正则表达式模式中间启用或禁用特定匹配选项。 其余两个使你可以在正则表达式中包含注释。  
  
## <a name="inline-options"></a>内联选项  

 可以使用语法为正则表达式的一部分设置或禁用特定模式匹配选项  
  
`(?imnsx-imnsx)`  
  
 在问号后列出要启用的选项，在负号后列出要禁用的选项。 下表对每个选项进行了描述。 有关每个选项的更多信息，请参见[正则表达式选项](regular-expression-options.md)。  
  
|选项|描述|  
|------------|-----------------|  
|`i`|不区分大小写的匹配。|  
|`m`|多行模式。|  
|`n`|仅显式捕获。 （圆括号不充当捕获组。）|  
|`s`|单行模式。|  
|`x`|忽略未转义空格，并允许 x 模式注释。|  
  
 如果 `(?imnsx-imnsx)` 构造定义的正则表达式选项有任何更改，更改在封闭组结束前一直有效。  
  
> [!NOTE]
> `(?imnsx-imnsx:`subexpression  `)` 分组构造为子表达式提供了完全相同的功能。 有关详细信息，请参阅 [分组构造](grouping-constructs-in-regular-expressions.md)。  
  
 下面的示例使用 `i`、`n` 和 `x` 选项，启用不区分大小写和显式捕获，并在正则表达式中间忽略正则表达式模式中的空格。  
  
 [!code-csharp[RegularExpressions.Language.Miscellaneous#1](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.miscellaneous/cs/miscellaneous1.cs#1)]
 [!code-vb[RegularExpressions.Language.Miscellaneous#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.miscellaneous/vb/miscellaneous1.vb#1)]  
  
 该示例定义两个正则表达式。 第一个 `\b(D\w+)\s(d\w+)\b` 匹配以一个大写“D”和一个小写“d”开头的两个连续单词。 第二个正则表达式 `\b(D\w+)(?ixn) \s (d\w+) \b` 使用内联选项修改此模式，如下表所述。 结果的比较会确认 `(?ixn)` 构造的效果。  
  
|模式|描述|  
|-------------|-----------------|  
|`\b`|在单词边界处开始。|  
|`(D\w+)`|匹配后跟一个或多个单词字符的大写“D”。 这是第一个捕获组。|  
|`(?ixn)`|从此处起，使比较不区分大小写，仅进行显式捕获，以及忽略正则表达式模式中的空格。|  
|`\s`|与空白字符匹配。|  
|`(d\w+)`|匹配后跟一个或多个单词字符的大写或小写“d”。 因为 `n`（显式捕获）选项已启用，所以不会捕获此组。|  
|`\b`|与字边界匹配。|  
  
## <a name="inline-comment"></a>内联注释  

 `(?#` comment`)` 构造可用于在正则表达式中添加内联注释  。 正则表达式引擎在模式匹配中不使用注释的任何部分，尽管注释仍包含在 <xref:System.Text.RegularExpressions.Regex.ToString%2A?displayProperty=nameWithType> 方法返回的字符串中。 该注释在第一个右括号处终止。  
  
 下面的示例重复了上一部分的示例中的第一个正则表达式模式。 它将两个内联注释添加到该正则表达式，以指示比较是否区分大小写。 正则表达式模式 `\b((?# case-sensitive comparison)D\w+)\s(?ixn)((?#case-insensitive comparison)d\w+)\b` 按以下方式定义。  
  
|模式|描述|  
|-------------|-----------------|  
|`\b`|在单词边界处开始。|  
|`(?# case-sensitive comparison)`|注释。 它不影响模式匹配行为。|  
|`(D\w+)`|匹配后跟一个或多个单词字符的大写“D”。 这是第一个捕获组。|  
|`\s`|与空白字符匹配。|  
|`(?ixn)`|从此处起，使比较不区分大小写，仅进行显式捕获，以及忽略正则表达式模式中的空格。|  
|`(?#case-insensitive comparison)`|注释。 它不影响模式匹配行为。|  
|`(d\w+)`|匹配后跟一个或多个单词字符的大写或小写“d”。 这是第二个捕获组。|  
|`\b`|与字边界匹配。|  
  
 [!code-csharp[RegularExpressions.Language.Miscellaneous#2](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.miscellaneous/cs/miscellaneous2.cs#2)]
 [!code-vb[RegularExpressions.Language.Miscellaneous#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.miscellaneous/vb/miscellaneous2.vb#2)]  
  
## <a name="end-of-line-comment"></a>行尾注释  

 数字符号 (`#`) 标记 x 模式注释，即从正则表达式模式末尾的未转义 # 字符开始一直延续到行末。 若要使用此构造，必须启用 `x` 选项（通过内联选项），或在实例化 <xref:System.Text.RegularExpressions.Regex> 对象或调用静态 <xref:System.Text.RegularExpressions.Regex> 方法时向 `option` 参数提供 <xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace?displayProperty=nameWithType> 值。  
  
 下面的示例说明行尾注释构造。 它确定字符串是否为包含至少一个格式项的复合格式字符串。 下表描述了正则表达式模式中的构造：  
  
 `\{\d+(,-*\d+)*(\:\w{1,4}?)*\}(?x) # Looks for a composite format item.`  
  
|模式|描述|  
|-------------|-----------------|  
|`\{`|匹配左大括号。|  
|`\d+`|匹配一个或多个十进制数字。|  
|`(,-*\d+)*`|与零个或一个后跟一个可选负号、再后跟一个或多个十进制数字的逗号匹配。|  
|`(\:\w{1,4}?)*`|与零个或一个后跟一到四个（但尽可能少）空白字符的冒号匹配。|  
|`\}`|匹配右大括号。|  
|`(?x)`|启用忽略模式空格选项，以便识别行尾注释。|  
|`# Looks for a composite format item.`|行尾注释。|  
  
 [!code-csharp[RegularExpressions.Language.Miscellaneous#3](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.miscellaneous/cs/miscellaneous3.cs#3)]
 [!code-vb[RegularExpressions.Language.Miscellaneous#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.miscellaneous/vb/miscellaneous3.vb#3)]  
  
 请注意，还可以调用 <xref:System.Text.RegularExpressions.Regex.IsMatch%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> 方法并向它传递 <xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace?displayProperty=nameWithType> 枚举值，从而识别注释，而不用在正则表达式中提供 `(?x)` 构造。  
  
## <a name="see-also"></a>请参阅

- [正则表达式语言 - 快速参考](regular-expression-language-quick-reference.md)
