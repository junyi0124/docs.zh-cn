---
title: .NET 正则表达式中的替换构造
description: 了解如何使用替换构造在正则表达式中进行条件匹配。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- either/or matching
- alternative matching patterns
- regular expressions, alternation constructs
- alternation constructs
- optional matching patterns
- constructs, alternation
- .NET regular expressions, alternation constructs
ms.assetid: 071e22e9-fbb0-4ecf-add1-8d2424f9f2d1
ms.openlocfilehash: 0a1b46443d5cd9f8a19f4cfb9f9b07a5e33e719e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95714687"
---
# <a name="alternation-constructs-in-regular-expressions"></a>正则表达式中的替换构造

替换构造可修改正则表达式以启用 either/or 或条件匹配。 .NET 支持三种替换构造：

- [利用 &#124; 的模式匹配](#Either_Or)
- [利用 (?(expression)yes&#124;no) 的条件匹配](#Conditional_Expr)
- [基于有效的捕获组的条件匹配](#Conditional_Group)

<a name="Either_Or"></a>

## <a name="pattern-matching-with-124"></a>利用 &#124; 的模式匹配

可以使用竖线 (`|`) 字符匹配一系列模式中的任何一种模式，其中 `|` 字符用于分隔每个模式。

与正向字符集一样， `|` 字符可用于匹配多个字符中的任意一个字符。 以下示例使用正向字符集和 either/or 模式匹配（使用 `|` 字符）查找字符串中单词“gray”或“grey”的匹配项。 在该示例中， `|` 字符生成了更为详细的正则表达式。

[!code-csharp[RegularExpressions.Language.Alternation#1](~/samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.alternation/cs/alternation1.cs#1)]
[!code-vb[RegularExpressions.Language.Alternation#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.alternation/vb/alternation1.vb#1)]

使用 `|` 字符的正则表达式 `\bgr(a|e)y\b` 的解释如下表所示：

|模式|描述|  
|-------------|-----------------|  
|`\b`|在单词边界处开始。|  
|`gr`|匹配字符“gr”。|  
|<code>(a&#124;e)</code>|匹配“a”或“e”。|  
|`y\b`|匹配单词边界中的“y”。|  

还可以使用 `|` 字符执行具有多个字符或子表达式（包含任意组合的字符常量和正则表达式语言元素）的 either/or 匹配。 （字符类不提供此功能。）下面的示例使用 `|` 字符提取美国社会安全号码 (SSN)（格式为 ddd-dd-dddd  的 9 位数字），或美国雇主标识号 (EIN)（格式为 dd-ddddddd  的 9 位数字）   。

[!code-csharp[RegularExpressions.Language.Alternation#2](~/samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.alternation/cs/alternation2.cs#2)]
[!code-vb[RegularExpressions.Language.Alternation#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.alternation/vb/alternation2.vb#2)]  

正则表达式 `\b(\d{2}-\d{7}|\d{3}-\d{2}-\d{4})\b` 可以解释为下表中所示内容：
  
|模式|描述|  
|-------------|-----------------|  
|`\b`|在单词边界处开始。|  
|<code>(\d{2}-\d{7}&#124;\d{3}-\d{2}-\d{4})</code>|匹配以下其中一个内容：连字符连接的两个十进制数字和七个十进制数字；或三个十进制数字后接连字符，后接两个十进制数字，后接另一个连字符，然后再接四个十进制数字。|  
|`\b`|在单词边界处结束匹配。|  
  
<a name="Conditional_Expr"></a>

## <a name="conditional-matching-with-an-expression"></a>条件匹配的表达式

此语言元素尝试根据是否可以匹配初始模式来匹配两种模式之一。 语法为：  

`(?(` *expression* `)` *yes* `|` *no* `)`

其中， *expression* 是要匹配的初始模式， *yes* 是当匹配 *expression* 时要匹配的模式，而 *no* 是未匹配 *expression* 时要匹配的可选模式。 正则表达式引擎将 *expression* 视为一个宽度为零的断言；也就是说，正则表达式引擎在计算 *expression* 之后，不再处理输入流的后续数据。 因此，该构造是等效于以下语法：

`(?(?=` *expression* `)` *yes* `|` *no* `)`

其中 `(?=`expression  `)` 是宽度为零的断言构造。 （有关详细信息，请参阅[分组构造](grouping-constructs-in-regular-expressions.md)。）由于正则表达式引擎将 expression  解释为定位点（零宽断言），因此 expression  必须是零宽断言（有关详细信息，请参阅[定位标记](anchors-in-regular-expressions.md)），或者是也包含在 yes  中的子表达式。 否则，无法匹配 *yes* 模式。  
  
> [!NOTE]
> 如果 expression  是命名捕获组或带编号的捕获组，则备用构造将被解释为捕获测试；有关详细信息，请参阅下一部分[基于有效捕获组的条件匹配](#Conditional_Group)。 换而言之，正则表达式引擎不会尝试匹配捕获的子字符串，而是测试该组是否存在。  
  
下面的示例是[利用 &#124; 的 Either/Or 模式匹配](#Either_Or)一节中的示例变体。 它使用条件匹配来确定单词边界之后的前三个字符是否是后接一个连字符的两个数字。 如果是，则将尝试匹配美国雇主标识号 (EIN)。 如果不是，则将尝试匹配美国社会保障号 (SSN)。

[!code-csharp[RegularExpressions.Language.Alternation#3](~/samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.alternation/cs/alternation3.cs#3)]
[!code-vb[RegularExpressions.Language.Alternation#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.alternation/vb/alternation3.vb#3)]

正则表达式模式 `\b(?(\d{2}-)\d{2}-\d{7}|\d{3}-\d{2}-\d{4})\b` 的释义如下表所示：

|模式|描述|  
|-------------|-----------------|  
|`\b`|在单词边界处开始。|  
|`(?(\d{2}-)`|确定接下来的三个字符是否由两个数字后接一个连字符组成。|  
|`\d{2}-\d{7}`|如果前面的模式匹配，则匹配后接一个连字符和七个数字的两个数字。|  
|`\d{3}-\d{2}-\d{4}`|如果前面的模式不匹配，则匹配三个十进制数字，后接一个连字符，再接两个十进制数字，再接另一个连字符，再接四个十进制数字。|  
|`\b`|与字边界匹配。|  

<a name="Conditional_Group"></a>

## <a name="conditional-matching-based-on-a-valid-captured-group"></a>基于有效的捕获组的条件匹配

此语言元素尝试根据是否已经匹配指定的捕获组来匹配两种模式之一。 语法为：

`(?(` *name* `)` *yes* `|` *no* `)`

or

`(?(` *number* `)` *yes* `|` *no* `)`

其中， *name* 是捕获组的名称， *number* 是捕获组的编号； *yes* 是当 *name* 或 *number* 具有匹配项时要匹配的表达式； *no* 是当不具有匹配项时要匹配的可选表达式。

如果 *name* 与正则表达式模式中所用捕获组的名称不对应，则替换构造将解释为表达式测试，如上一节中所述。 通常，这意味着 *expression* 的计算结果为 `false`。 如果 *number* 与正则表达式模式中所用带编号的捕获组不对应，则正则表达式引擎将引发 <xref:System.ArgumentException>。

下面的示例是[利用 &#124; 的 Either/Or 模式匹配](#Either_Or)一节中的示例变体。 它使用一个名为 `n2` 的捕获组，其中包含两个数字，后接一个连字符。 替换构造测试此捕获组是否在输入字符串中找到匹配项。 如果有匹配项，则替换构造会尝试匹配九位数的美国雇主标识号 (EIN)。 如果没有匹配项，则将尝试匹配九位数的美国社会保障号 (SSN)。

[!code-csharp[RegularExpressions.Language.Alternation#4](~/samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.alternation/cs/alternation4.cs#4)]
[!code-vb[RegularExpressions.Language.Alternation#4](~/samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.alternation/vb/alternation4.vb#4)]

正则表达式模式 `\b(?<n2>\d{2}-)?(?(n2)\d{7}|\d{3}-\d{2}-\d{4})\b` 的释义如下表所示：

|模式|描述|  
|-------------|-----------------|  
|`\b`|在单词边界处开始。|  
|`(?<n2>\d{2}-)?`|匹配两个数字后接一个连字符的零或一个匹配项。 命名此捕获组 `n2`。|  
|`(?(n2)`|测试输入字符串中是否有 `n2` 的匹配项。|  
|`\d{7}`|如果找到 `n2` 的匹配项，则匹配 7 个十进制数字。|  
|<code>&#124;\d{3}-\d{2}-\d{4}</code>|如果未找到 `n2` 的匹配项，则匹配 3 个十进制数字，后接一个连字符，再接 2 个十进制数字，再接另一个连字符，再接 4 个十进制数字。|  
|`\b`|与字边界匹配。|  

下面示例中显示此示例变体使用编号组而非命名组。 正则表达式模式为 `\b(\d{2}-)?(?(1)\d{7}|\d{3}-\d{2}-\d{4})\b`。

[!code-csharp[RegularExpressions.Language.Alternation#5](~/samples/snippets/csharp/VS_Snippets_CLR/regularexpressions.language.alternation/cs/alternation5.cs#5)]
[!code-vb[RegularExpressions.Language.Alternation#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/regularexpressions.language.alternation/vb/alternation5.vb#5)]

## <a name="see-also"></a>请参阅

- [正则表达式语言 - 快速参考](regular-expression-language-quick-reference.md)
