---
title: 正则表达式中的限定符
description: 了解正则表达式限定符，该类限定符指定输入中必须存在字符、组或字符类的多少实例才能进行匹配。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- regular expressions, quantifiers
- metacharacters, quantifiers
- minimal matching quantifiers
- quantifiers in regular expressions
- .NET regular expressions, quantifiers
- quantifiers
- lazy quantifiers
ms.assetid: 36b81212-6511-49ed-a8f1-ff080415312f
ms.openlocfilehash: 75d37527507b596d6017171279e84b8348489831
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733576"
---
# <a name="quantifiers-in-regular-expressions"></a>正则表达式中的限定符

限定符指定输入中必须存在字符、组或字符类的多少实例才能找到匹配项。  下表列出了 .NET 支持的限定符。  
  
|贪婪限定符|惰性限定符|描述|  
|-----------------------|---------------------|-----------------|  
|`*`|`*?`|匹配零次或多次。|  
|`+`|`+?`|匹配一次或多次。|  
|`?`|`??`|匹配零次或一次。|  
|`{` n `}`|`{` n `}?`|恰好匹配 n 次。|  
|`{` n `,}`|`{` n `,}?`|至少匹配 n 次。|  
|`{` n `,` m `}` |`{` n `,` m `}?` |匹配 n 到 m 次 。|  
  
 数量 `n` 和 `m` 是整数常量。 通常，限定符是贪婪的；它们使正则表达式引擎匹配尽可能多的特定模式实例。 向限定符追加 `?` 字符可使它成为惰性的；会使正则表达式引擎匹配尽可能少的实例。 有关贪婪与惰性限定符之间的差异的完整说明，请参见本主题后面的[贪婪与惰性限定符](#Greedy)部分。  
  
> [!IMPORTANT]
> 嵌套限定符（例如正则表达式模式 `(a*)*` 的行为）可以按输入字符串中的字符数的指数函数形式，来增加正则表达式引擎必须执行的比较次数。 若要详细了解此行为及其解决方法，请参阅[回溯](backtracking-in-regular-expressions.md)。  
  
## <a name="regular-expression-quantifiers"></a>正则表达式限定符  

 以下部分列出了 .NET 正则表达式支持的限定符。  
  
> [!NOTE]
> 如果在正则表达式模式中遇到 *、+、?、{ 和 } 字符，正则表达式引擎会将它们解释为量符或量符构造的一部分，除非它们包含在[字符类](character-classes-in-regular-expressions.md)中。 若要在字符类外部将这些字符解释文本字符，必须通过在它们前面加反斜杠来对它们进行转义。 例如，正则表达式模式中的字符串 `\*` 会被解释为文本星号（“\*”）字符。  
  
### <a name="match-zero-or-more-times-"></a>匹配零次或多次：*  

 `*` 限定符与前面的元素匹配零次或多次。 它相当于 `{0,}` 量符。 `*` 是贪婪量符，相当的惰性量符是 `*?`。  
  
 下面的示例说明此正则表达式。 在输入字符串中的九个数字组中，五个与模式匹配，四个（`95`、`929`、`9219` 和 `9919`）不匹配。  
  
 [!code-csharp[RegularExpressions.Quantifiers#1](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#1)]
 [!code-vb[RegularExpressions.Quantifiers#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#1)]  
  
 正则表达式模式的定义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`\b`|在单词边界处开始。|  
|`91*`|匹配后跟零个或多个“1”字符的“9”。|  
|`9*`|匹配零个或多个“9”字符。|  
|`\b`|在字边界结束。|  
  
### <a name="match-one-or-more-times-"></a>匹配一次或多次：+  

 `+` 量符匹配上一元素一次或多次。 它相当于 `{1,}`。 `+` 是贪婪量符，相当的惰性量符是 `+?`。  
  
 例如，正则表达式 `\ban+\w*?\b` 会尝试匹配以后跟字母 `n` 的一个或多个实例的字母 `a` 开头的完整单词。 下面的示例说明此正则表达式。 正则表达式会匹配单词 `an`、`annual`、`announcement` 和 `antique`，并且正确地无法匹配 `autumn` 和 `all`。  
  
 [!code-csharp[RegularExpressions.Quantifiers#2](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#2)]
 [!code-vb[RegularExpressions.Quantifiers#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#2)]  
  
 正则表达式模式的定义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`\b`|在单词边界处开始。|  
|`an+`|匹配后跟一个或多个“n”字符的“a”。|  
|`\w*?`|匹配单词字符零次或多次，但次数尽可能少。|  
|`\b`|在字边界结束。|  
  
### <a name="match-zero-or-one-time-"></a>匹配零次或一次：?  

 `?` 量符匹配上一元素零次或一次。 它相当于 `{0,1}`。 `?` 是贪婪量符，相当的惰性量符是 `??`。  
  
 例如，正则表达式 `\ban?\b` 会尝试匹配以后跟字母 `n` 的零个或一个实例的字母 `a` 开头的完整单词。 换句话说，它会尝试匹配单词 `a` 和 `an`。 下面的示例说明此正则表达式。  
  
 [!code-csharp[RegularExpressions.Quantifiers#3](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#3)]
 [!code-vb[RegularExpressions.Quantifiers#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#3)]  
  
 正则表达式模式的定义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`\b`|在单词边界处开始。|  
|`an?`|匹配后跟零个或一个“n”字符的“a”。|  
|`\b`|在字边界结束。|  
  
### <a name="match-exactly-n-times-n"></a>恰好匹配 n 次：{n}  

 `{`n`}` 限定符与前面的元素恰好匹配 n 次，其中 n 是任何整数  。 `{`n`}` 是贪婪限定符，其惰性等效项是 `{`n`}?` 。  
  
 例如，正则表达式 `\b\d+\,\d{3}\b` 会尝试匹配依次后跟一个或多个十进制数字、三个十进制数字、一个单词边界的单词边界。 下面的示例说明此正则表达式。  
  
 [!code-csharp[RegularExpressions.Quantifiers#4](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#4)]
 [!code-vb[RegularExpressions.Quantifiers#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#4)]  
  
 正则表达式模式的定义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`\b`|在单词边界处开始。|  
|`\d+`|匹配一个或多个十进制数字。|  
|`\,`|匹配逗号字符。|  
|`\d{3}`|匹配三个十进制数字。|  
|`\b`|在字边界结束。|  
  
### <a name="match-at-least-n-times-n"></a>至少匹配 n 次：{n,}  

 `{`n`,}` 限定符与前面的元素至少匹配 n 次，其中 n 是任何整数  。 `{`n`,}` 是贪婪限定符，其惰性等效项是 `{`n`,}?` 。  
  
 例如，正则表达式 `\b\d{2,}\b\D+` 会尝试匹配依次后跟至少两个数字、一个单词边界和一个非数字字符的单词边界。 下面的示例说明此正则表达式。 正则表达式无法匹配短语 `"7 days"`，因为它只包含一个十进制数字，但可以成功匹配短语 `"10 weeks and 300 years"`。  
  
 [!code-csharp[RegularExpressions.Quantifiers#5](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#5)]
 [!code-vb[RegularExpressions.Quantifiers#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#5)]  
  
 正则表达式模式的定义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`\b`|在单词边界处开始。|  
|`\d{2,}`|匹配至少两个十进制数字。|  
|`\b`|与字边界匹配。|  
|`\D+`|匹配至少一个非十进制数字。|  
  
### <a name="match-between-n-and-m-times-nm"></a>匹配 n 到 m 次：{n,m}  

 `{`n`,`m`}` 限定符与前面的元素至少匹配 n 次，但不超过 m 次，其中 n 和 m 是整数     。 `{`n`,`m`}` 是贪婪限定符，相当的惰性限定符是 `{`n`,`m`}?`   。  
  
 在下面的示例中，正则表达式 `(00\s){2,4}` 尝试与后跟一个空格的两个零数字匹配两到四次。 请注意，输入字符串的最后一部分包含此模式五次，而不是最大值四次。 但是，只有此子字符串的初始部分（到空格和第五对零）与正则表达式模式匹配。  
  
 [!code-csharp[RegularExpressions.Quantifiers#6](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#6)]
 [!code-vb[RegularExpressions.Quantifiers#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#6)]  
  
### <a name="match-zero-or-more-times-lazy-match-"></a>匹配零次或多次（惰性匹配）：*?  

 `*?` 量符匹配上一元素零次或多次，但次数尽可能少。 它是贪婪量符 `*` 对应的惰性量符。  
  
 在下面的示例中，正则表达式 `\b\w*?oo\w*?\b` 匹配包含字符串 `oo` 的所有单词。  
  
 [!code-csharp[RegularExpressions.Quantifiers#7](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#7)]
 [!code-vb[RegularExpressions.Quantifiers#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#7)]  
  
 正则表达式模式的定义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`\b`|在单词边界处开始。|  
|`\w*?`|匹配零个或多个单词字符，但字符要尽可能的少。|  
|`oo`|匹配字符串“oo”。|  
|`\w*?`|匹配零个或多个单词字符，但字符要尽可能的少。|  
|`\b`|在单词边界处结束。|  
  
### <a name="match-one-or-more-times-lazy-match-"></a>匹配一次或多次（惰性匹配）：+?  

 `+?` 量符匹配上一元素一次或多次，但次数尽可能少。 它是贪婪量符 `+` 对应的惰性量符。  
  
 例如，正则表达式 `\b\w+?\b` 匹配由单词边界分隔的一个或多个字符。 下面的示例说明此正则表达式。  
  
 [!code-csharp[RegularExpressions.Quantifiers#8](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#8)]
 [!code-vb[RegularExpressions.Quantifiers#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#8)]  
  
### <a name="match-zero-or-one-time-lazy-match-"></a>匹配零次或一次（惰性匹配）：??  

 `??` 量符匹配上一元素零次或一次，但次数尽可能少。 它是贪婪量符 `?` 对应的惰性量符。  
  
 例如，正则表达式 `^\s*(System.)??Console.Write(Line)??\(??` 尝试匹配字符串“Console.Write”或“Console.WriteLine”。 字符串还可以在“Console”前面包含“System.”， 并且可以后跟左括号。 字符串必须处于行的开头，不过前面可以是空格。 下面的示例说明此正则表达式。  
  
 [!code-csharp[RegularExpressions.Quantifiers#9](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#9)]
 [!code-vb[RegularExpressions.Quantifiers#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#9)]  
  
 正则表达式模式的定义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`^`|匹配输入流的开头。|  
|`\s*`|匹配零个或多个空白字符。|  
|`(System.)??`|匹配字符串“System.”的零个或一个匹配项。|  
|`Console.Write`|匹配字符串“Console.Write”。|  
|`(Line)??`|匹配字符串“Line”的零个或一个匹配项。|  
|`\(??`|匹配左括号的零个或一个匹配项。|  
  
### <a name="match-exactly-n-times-lazy-match-n"></a>恰好匹配 n 次（惰性匹配）：{n}?  

 `{`n`}?` 限定符与前面的元素恰好匹配 `n` 次，其中 n 是任何整数 。 它是贪婪限定符 `{`n`}` 的惰性对应项。  
  
 在下面的示例中，正则表达式 `\b(\w{3,}?\.){2}?\w{3,}?\b` 用于标识网站地址。 请注意，它匹配“www.microsoft.com”和“msdn.microsoft.com”，但不匹配“mywebsite”或“mycompany.com”。  
  
 [!code-csharp[RegularExpressions.Quantifiers#10](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#10)]
 [!code-vb[RegularExpressions.Quantifiers#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#10)]  
  
 正则表达式模式的定义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`\b`|在单词边界处开始。|  
|`(\w{3,}?\.)`|匹配至少 3 个后跟一个点或句点字符的单词字符，但字符数尽可能少。 这是第一个捕获组。|  
|`(\w{3,}?\.){2}?`|匹配第一个组中的模式两次，但次数尽可能少。|  
|`\b`|在单词边界处结束匹配。|  
  
### <a name="match-at-least-n-times-lazy-match-n"></a>至少匹配 n 次（惰性匹配）：{n,}?  

 `{`n`,}?` 限定符与前面的元素至少匹配 `n` 次，其中 n 是任何整数，但次数尽可能少 。 它是贪婪限定符 `{`n`,}` 的惰性对应项。  
  
 有关说明，请参阅上一部分中的 `{`n`}?` 限定符示例。 该示例中的正则表达式使用 `{`n`,}` 限定符匹配包含后跟一个句点的至少三个字符的字符串。  
  
### <a name="match-between-n-and-m-times-lazy-match-nm"></a>匹配 n 到 m 次（惰性匹配）：{n,m}?  

 `{`n`,`m`}?` 限定符匹配上一元素 `n` 次到 `m` 次，其中 n 和 m 是整数，但次数尽可能少   。 它是贪婪限定符 `{`n`,`m`}` 的惰性对应项 。  
  
 在下面的示例中，正则表达式 `\b[A-Z](\w*?\s*?){1,10}[.!?]` 匹配包含一到十个单词的句子。 它可匹配输入字符串中的所有句子（除了包含 18 个单词的一个句子）。  
  
 [!code-csharp[RegularExpressions.Quantifiers#12](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers/cs/Quantifiers1.cs#12)]
 [!code-vb[RegularExpressions.Quantifiers#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers/vb/Quantifiers1.vb#12)]  
  
 正则表达式模式的定义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`\b`|在单词边界处开始。|  
|`[A-Z]`|匹配从 A 到 Z 的大写字符。|  
|`(\w*?\s*?)`|匹配零个或多个后跟一个或多个空白字符的单词字符，但次数应尽可能少。 这是第一个捕获组。|  
|`{1,10}`|与前面的模式匹配 1 到 10 次。|  
|`[.!?]`|匹配标点字符“.”、“!”或“?”中的任何一种。|  
  
<a name="Greedy"></a>

## <a name="greedy-and-lazy-quantifiers"></a>贪婪与惰性限定符  

 一些限定符具有两个版本：  
  
- 贪婪版本。  
  
     贪婪限定符尝试尽可能多地匹配元素。  
  
- 非贪婪（或惰性）版本。  
  
     非贪婪限定符尝试尽可能少地匹配元素。 只需添加 `?`，即可将贪婪量符转换为惰性量符。  
  
 请考虑一个简单的正则表达式，它旨在从数字字符串（如信用卡号）中提取最后四位数。 使用 `*`贪婪量符的正则表达式版本是 `\b.*([0-9]{4})\b`。 但是，如果字符串包含两个数字，则此正则表达式仅匹配第二个数字的最后四位数，如下面的示例所示。  
  
 [!code-csharp[RegularExpressions.Quantifiers.Greedy#1](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers.Greedy/cs/Greedy.cs#1)]
 [!code-vb[RegularExpressions.Quantifiers.Greedy#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers.Greedy/vb/Greedy.vb#1)]  
  
 正则表达式无法匹配第一个数字，因为 `*` 量符尝试在整个字符串中尽可能多地匹配上一元素，所以它会在字符串末尾找到匹配。  
  
 这不是所需行为。 相反，可以使用 `*?` 惰性量符，从这两个数字提取数字，如下面的示例所示。  
  
 [!code-csharp[RegularExpressions.Quantifiers.Greedy#2](../../../samples/snippets/csharp/VS_Snippets_CLR/RegularExpressions.Quantifiers.Greedy/cs/Greedy.cs#2)]
 [!code-vb[RegularExpressions.Quantifiers.Greedy#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/RegularExpressions.Quantifiers.Greedy/vb/Greedy.vb#2)]  
  
 在大多数情况下，具有贪婪和惰性限定符的正则表达式返回相同匹配项。 与匹配任何字符的通配符 (`.`) 元字符一起使用时，它们通常会返回不同的结果。  
  
## <a name="quantifiers-and-empty-matches"></a>限定符和空匹配项  

 如果已找到最小捕获数，限定符 `*`、`+` 和 `{`n`,`m`}` 及对应的惰性限定符绝不会在空匹配项后重复 。 此规则会在最大可能组捕获数是无限或接近无限时，阻止限定符在空的子表达式匹配项上进入无限循环。  
  
 例如，下面的代码展示了使用正则表达式模式 `(a?)*`（匹配零个或一个“a”字符零次或多次）调用 <xref:System.Text.RegularExpressions.Regex.Match%2A?displayProperty=nameWithType> 方法的结果。 请注意，一个捕获组捕获所有“a”以及 <xref:System.String.Empty?displayProperty=nameWithType>，但没有第二个空匹配，因为第一个空匹配导致量符停止重复运行。  
  
 [!code-csharp[RegularExpressions.Quantifiers.EmptyMatch#1](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.quantifiers.emptymatch/cs/emptymatch1.cs#1)]
 [!code-vb[RegularExpressions.Quantifiers.EmptyMatch#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.quantifiers.emptymatch/vb/emptymatch1.vb#1)]  
  
 若要查看定义最小和最大捕获数的捕获组与定义固定捕获数的捕获组之间的实际差异，请考虑正则表达式模式 `(a\1|(?(1)\1)){0,2}` 和 `(a\1|(?(1)\1)){2}`。 这两个正则表达式包含单个捕获组，其定义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`(a\1`|匹配“a”以及第一个捕获组的值...|  
|<code>&#124;(?(1)</code>|… 或测试是否定义了第一个捕获组。 （请注意，`(?(1)` 构造不定义捕获组。）|  
|`\1))`|如果第一个捕获组存在，则匹配其值。 如果组不存在，组会匹配 <xref:System.String.Empty?displayProperty=nameWithType>。|  
  
 第一个正则表达式尝试与此模式匹配零到二次；第二个正则表达式尝试恰好匹配两次。 由于第一个模式在首次捕获 <xref:System.String.Empty?displayProperty=nameWithType> 时达到最小捕获数，因此它绝不会重复尝试匹配 `a\1`；`{0,2}` 量符仅允许在最后一个迭代中有空匹配。 相反，第二个正则表达式匹配“a”，因为它会第二次计算 `a\1`；最小迭代数 2 会强制引擎在空匹配项后面重复。  
  
 [!code-csharp[RegularExpressions.Quantifiers.EmptyMatch#2](../../../samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.quantifiers.emptymatch/cs/emptymatch4.cs#2)]
 [!code-vb[RegularExpressions.Quantifiers.EmptyMatch#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.quantifiers.emptymatch/vb/emptymatch4.vb#2)]  
  
## <a name="see-also"></a>请参阅

- [正则表达式语言 - 快速参考](regular-expression-language-quick-reference.md)
- [回溯](backtracking-in-regular-expressions.md)
