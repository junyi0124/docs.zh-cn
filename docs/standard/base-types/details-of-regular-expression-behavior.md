---
title: 正则表达式行为
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- regular expressions, behavior
- .NET regular expressions, behavior
ms.assetid: 0ee1a6b8-caac-41d2-917f-d35570021b10
ms.openlocfilehash: 1e5d2d40f52220a8fff40eb19a24d8b2efd3cab5
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94829695"
---
# <a name="details-of-regular-expression-behavior"></a>正则表达式行为的详细信息

.NET 正则表达式引擎是回溯正则表达式匹配程序，其中包含传统的非确定性有限自动机 (NFA) 引擎（如 Perl、Python、Emacs 和 Tcl 所使用的引擎）。 这使它有别于速度更快、但是限制更多的纯正则表达式确定性有限自动机 (DFA) 引擎（如 awk、egrep 或 lex 中的引擎）。 这也使它有别于标准化、但速度较慢的 POSIX NFA。 以下部分介绍正则表达式引擎的三种类型，并说明为何使用传统 NFA 引擎实现 .NET 中的正则表达式。

## <a name="benefits-of-the-nfa-engine"></a>NFA 引擎的优势

 DFA 引擎执行模式匹配时，其处理顺序由输入字符串驱动。 该引擎从输入字符串的开头处开始，按顺序继续进行以确定下一个字符是否与正则表达式模式匹配。 它们可以保证匹配可能最长的字符串。 因为它们绝不会对相同字符测试两次，所以 DFA 引擎不支持回溯。 但是，由于 DFA 引擎只包含有限状态，因此它无法匹配具有反向引用的模式，并且因为它不构造显式扩展，所以无法捕获子表达式。

 与 DFA 引擎不同，传统 NFA 引擎执行模式匹配时，其处理顺序由正则表达式模式驱动。 处理特定语言元素时，该引擎使用贪婪匹配；也就是说，它尽可能多地匹配输入字符串。 但是，它还会在成功匹配子表达式之后保存其状态。 如果匹配最终失败，则该引擎可以返回到已保存状态，以便可以尝试其他匹配项。 这样的过程称为“回溯”  ，即放弃成功子表达式匹配，以便正则表达式中后面的语言元素也可以进行匹配。 NFA 引擎使用回溯按特定顺序测试正则表达式的所有可能扩展，并接受第一个匹配项。 因为传统 NFA 引擎针对成功匹配构造正则表达式的特定扩展，所以它可以捕获子表达式匹配项以及匹配反向引用。 但是，由于传统 NFA 会进行回溯，因此如果它通过不同路径到达相同状态，则可能会多次访问该状态。 因此，其运行速度在最糟糕的情况下可能会极端缓慢。 因为传统 NFA 引擎接受它找到的第一个匹配项，所以它还可能会使其他（可能更长）的匹配项保持未发现状态。

 POSIX NFA 引擎类似于传统 NFA 引擎，只不过它们会继续回溯，直到可以保证已找到最长的可能匹配项。 因此，POSIX NFA 引擎速度低于传统 NFA 引擎，而且使用 POSIX NFA 引擎时，无法通过更改回溯搜索的顺序，使较短匹配项优先于较长匹配项。

 程序员更喜欢传统 NFA 引擎，因为与 DFA 或 POSIX NFA 引擎相比，通过它们可更好地控制字符串匹配。 不过在最糟糕的情况下，它们可能会运行缓慢，你可以使用减少歧义和限制回溯的模式，控制它们以线性方式或在多项式时间内找到匹配项。 换句话说，虽然 NFA 引擎以牺牲性能为代价来实现强大功能和灵活性，不过在大多数情况下，如果正则表达式编写良好并避免回溯呈指数级降低性能的情况，它们可提供可接受的良好性能。

> [!NOTE]
> 若要了解过度回溯导致的性能损失，以及如何生成正则表达式来解决此问题，请参阅[回溯](backtracking-in-regular-expressions.md)。

## <a name="net-engine-capabilities"></a>.NET 引擎功能

 为了利用传统 NFA 引擎的优势，.NET 正则表达式引擎包括一组全面的构造，使程序员可以控制回溯引擎。 这些构造可以用于更快地找到匹配项或使特定扩展优先于其他扩展。

 .NET 正则表达式引擎的其他功能包括以下这些：

- 惰性限定符：`??`、`*?`、`+?`、`{`n`,`m`}?`   。 这些构造会指示回溯引擎首先搜索最小数量的重复项。 相反，普通贪婪限定符会尝试首先匹配最大数量的重复项。 以下示例演示了两者之间的差异。 正则表达式匹配以数字结尾的句子，捕获组旨在提取该数字。 正则表达式 `.+(\d+)\.` 包含贪婪限定符 `.+`，这使正则表达式引擎仅捕获数字的最后一位数。 相反，正则表达式 `.+?(\d+)\.` 包含惰性限定符 `.+?`，这使正则表达式引擎捕获整个数字。

     [!code-csharp[Conceptual.RegularExpressions.Design#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/lazy1.cs#1)]
     [!code-vb[Conceptual.RegularExpressions.Design#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/lazy1.vb#1)]

     下表定义了此正则表达式的贪婪和惰性版本：

    |模式|描述|
    |-------------|-----------------|
    |`.+`（贪婪限定符）|匹配任何字符的至少一个匹配项。 这会导致正则表达式引擎匹配整个字符串，然后根据需要进行回溯以匹配模式的其余部分。|
    |`.+?`（惰性限定符）|匹配任何字符的至少一个匹配项，但匹配尽可能少。|
    |`(\d+)`|匹配至少一个数字字符，并将其分配给第一个捕获组。|
    |`\.`|匹配句点。|

     若要详细了解惰性量符，请参阅[量符](quantifiers-in-regular-expressions.md)。

- 正预测先行断言：`(?=`subexpression  `)`。 此功能允许回溯引擎在匹配子表达式之后返回到文本中的相同位置。 它可用于通过验证从相同位置开始的多个模式来搜索整个文本。 它还允许引擎验证匹配项末尾是否存在某个子字符串，而无需在匹配的文本中包含该子字符串。 下面的示例使用正预测先行提取句子中后面不是标点符号的单词。

     [!code-csharp[Conceptual.RegularExpressions.Design#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/lookahead1.cs#2)]
     [!code-vb[Conceptual.RegularExpressions.Design#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/lookahead1.vb#2)]

     正则表达式 `\b[A-Z]+\b(?=\P{P})` 的定义如下表所示。

    |模式|描述|
    |-------------|-----------------|
    |`\b`|在单词边界处开始匹配。|
    |`[A-Z]+`|匹配任何字母字符一次或多次。 由于 <xref:System.Text.RegularExpressions.Regex.Matches%2A?displayProperty=nameWithType> 方法是使用 <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> 选项进行调用，因此比较不区分大小写。|
    |`\b`|在单词边界处结束匹配。|
    |`(?=\P{P})`|预测先行以确定下一个字符是否为标点符号。 如果不是，则匹配成功。|

     若要详细了解正预测先行断言，请参阅[分组构造](grouping-constructs-in-regular-expressions.md)。

- 负预测先行断言：`(?!`subexpression  `)`。 通过此功能可以仅当子表达式未能匹配时才匹配表达式。 这对于修剪搜索十分有用，因为针对应消除的情况提供表达式通常比针对必须包括的情况提供表达式要更简单。 例如，难以为不以“non”开头的单词编写表达式。 下面的示例使用负预测先行排除它们。

     [!code-csharp[Conceptual.RegularExpressions.Design#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/lookahead2.cs#3)]
     [!code-vb[Conceptual.RegularExpressions.Design#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/lookahead2.vb#3)]

     正则表达式模式 `\b(?!non)\w+\b` 的定义如下表所示。

    |模式|描述|
    |-------------|-----------------|
    |`\b`|在单词边界处开始匹配。|
    |`(?!non)`|预测先行以确保当前字符串不以“non”开头。 如果以“non”开头，则匹配失败。|
    |`(\w+)`|匹配一个或多个单词字符。|
    |`\b`|在单词边界处结束匹配。|

     若要详细了解负预测先行断言，请参阅[分组构造](grouping-constructs-in-regular-expressions.md)。

- 条件求值：`(?(`expression  `)`yes  `|`no  `)` 和 `(?(`name  `)`yes  `|`no  `)`，其中 expression  是要匹配的子表达式，name  是捕获组的名称，yes  是在 expression  匹配或 name  是有效的非空捕获组时要匹配的字符串，no  是在 expression  不匹配或 name  不是有效的非空捕获组时要匹配的子表达式。 此功能允许引擎使用多个备用模式进行搜索（具体取决于上一个子表达式匹配的结果或零宽度断言的结果）。 这样可实现功能更强大的反向引用形式，例如，它允许基于上一个子表达式是否匹配来匹配子表达式。 下面示例中的正则表达式匹配旨在供公共和内部使用的段落。 仅供内部使用的段落以 `<PRIVATE>` 标记开头。 正则表达式模式 `^(?<Pvt>\<PRIVATE\>\s)?(?(Pvt)((\w+\p{P}?\s)+)|((\w+\p{P}?\s)+))\r?$` 使用条件评估将旨在供公共使用和内部使用的段落内容分配给不同的捕获组。 这些段落随后可以按不同方式进行处理。

     [!code-csharp[Conceptual.RegularExpressions.Design#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/conditional1.cs#4)]
     [!code-vb[Conceptual.RegularExpressions.Design#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/conditional1.vb#4)]

     正则表达式模式的定义如下表所示。

    |模式|描述|
    |-------------|-----------------|
    |`^`|从行的开头开始匹配。|
    |`(?<Pvt>\<PRIVATE\>\s)?`|匹配后跟一个空白字符的字符串 `<PRIVATE>` 的零个或一个匹配项。 将匹配项分配给 `Pvt` 捕获组。|
    |`(?(Pvt)((\w+\p{P}?\s)+)`|如果 `Pvt` 捕获组存在，则匹配后跟零个或一个标点分隔符、再后跟一个空白字符的一个或多个单词字符的一个或多个匹配项。 将子字符串分配给第一个捕获组。|
    |<code>&#124;((\w+\p{P}?\s)+))</code>|如果 `Pvt` 捕获组不存在，则匹配后跟零个或一个标点分隔符、再后跟一个空白字符的一个或多个单词字符的一个或多个匹配项。 将子字符串分配给第三个捕获组。|
    |`\r?$`|匹配行尾或字符串末尾。|

     若要详细了解条件求值，请参阅[替换构造](alternation-constructs-in-regular-expressions.md)。

- 平衡组定义：`(?<`*name1*`-`*name2*`>` *subexpression*`)`。 此功能允许正则表达式引擎跟踪嵌套构造（如圆括号或者左方括号和右方括号）。 有关示例，请参阅[分组构造](grouping-constructs-in-regular-expressions.md)。

- 原子组：`(?>`subexpression`)`  。 此功能允许回溯引擎保证子表达式仅匹配为该子表达式找到的第一个匹配项，就如同该表达式独立于其包含表达式运行一样。 如果不使用此构造，则来自较大表达式的回溯搜索可能会更改子表达式的行为。 例如，正则表达式 `(a+)\w` 除了一系列“a”字符后面的单词字符匹配，还与一个或多个“a”字符相匹配，并且它将该系列“a”字符分配给第一个捕获组。 但是，如果输入字符串的最后一个字符也是“a“，则它由 `\w` 语言元素匹配且不包含在捕获的组中。

     [!code-csharp[Conceptual.RegularExpressions.Design#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/nonbacktracking2.cs#7)]
     [!code-vb[Conceptual.RegularExpressions.Design#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/nonbacktracking2.vb#7)]

     正则表达式 `((?>a+))\w` 会阻止此行为。 因为所有连续“a”字符会在不进行回溯的情况下匹配，所以第一个捕获组包含所有连续“a”字符。 如果“a”字符后面不是至少一个“a”之外的字符，则匹配会失败。

     [!code-csharp[Conceptual.RegularExpressions.Design#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/nonbacktracking1.cs#8)]
     [!code-vb[Conceptual.RegularExpressions.Design#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/nonbacktracking1.vb#8)]

     要详细了解原子组，请参阅[分组构造](grouping-constructs-in-regular-expressions.md)。

- 从右到左匹配：指定方式为向 <xref:System.Text.RegularExpressions.Regex> 类构造函数或静态实例匹配方法提供 <xref:System.Text.RegularExpressions.RegexOptions.RightToLeft?displayProperty=nameWithType> 选项。 当从右到左（而不是从左到右）进行搜索时，或是在从模式右侧部分（而不是左侧部分）开始匹配效率更高的情况下，此功能非常有用。 如下面的示例所示，使用从右到左匹配可以更改贪婪限定符的行为。 该示例对以数字结尾的句子执行两个搜索。 使用贪婪限定符 `+` 的从左到右搜索匹配句子中六个数字之一，而从右到左搜索匹配所有六个数字。 有关正则表达式模式的介绍，请参见此部分前面说明惰性限定符的示例。

     [!code-csharp[Conceptual.RegularExpressions.Design#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/rtl1.cs#6)]
     [!code-vb[Conceptual.RegularExpressions.Design#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/rtl1.vb#6)]

     有关从右到左匹配的更多信息，请参见[正则表达式选项](regular-expression-options.md)。

- 正负回顾后发断言：`(?<=`subexpression  `)`（正回顾后发断言）和 `(?<!`subexpression  `)`（负回顾后发断言）。 此功能非常类似于本主题前面讨论的预测先行。 由于正则表达式引擎允许完全的从右到左匹配，因此正则表达式允许无限制回顾。 当嵌套子表达式是外部表达式的超集时，正回顾和负回顾还可以用于避免嵌套限定符。 具有此类嵌套限定符的正则表达式通常性能不佳。 例如，下面的示例验证字符串是否以字母数字字符开头和结尾，以及字符串中的任何其他字符是否为更大子集之一。 它形成用于验证电子邮件地址的正则表达式的一部分内容；有关详细信息，请参阅[如何：确认字符串是有效的电子邮件格式](how-to-verify-that-strings-are-in-valid-email-format.md)。

     [!code-csharp[Conceptual.RegularExpressions.Design#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.design/cs/lookbehind1.cs#5)]
     [!code-vb[Conceptual.RegularExpressions.Design#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.design/vb/lookbehind1.vb#5)]

     正则表达式 ``^[A-Z0-9]([-!#$%&'.*+/=?^`{}|~\w])*(?<=[A-Z0-9])$`` 的定义如下表所示。

    |模式|描述|
    |-------------|-----------------|
    |`^`|从字符串开头开始匹配。|
    |`[A-Z0-9]`|匹配任意数字或字母数字字符。 （比较不区分大小写。）|
    |<code>([-!#$%&'.*+/=?^\`{}&#124;~\w])\*</code>|匹配零个或多个任意单词字符或下列任意字符：-、!、#、$、%、&、'、.、\*、+、/、=、?、^、&#96;、{, }、&#124; 或 ~。|
    |`(?<=[A-Z0-9])`|回顾上一个字符（必须是数字或字母数字）。 （比较不区分大小写。）|
    |`$`|在字符串的结尾结束匹配。|

     若要详细了解正负向后行断言，请参阅[分组构造](grouping-constructs-in-regular-expressions.md)。

## <a name="related-articles"></a>相关文章

|Title|描述|
|-----------|-----------------|
|[回溯](backtracking-in-regular-expressions.md)|提供有关正则表达式回溯如何进行分支以查找替代匹配的信息。|
|[编译和重用](compilation-and-reuse-in-regular-expressions.md)|提供有关编译和重复使用正则表达式以提高性能的信息。|
|[线程安全性](thread-safety-in-regular-expressions.md)|提供有关正则表达式线程安全的信息，并说明何时应同步对正则表达式对象进行的访问。|
|[.NET 正则表达式](regular-expressions.md)|提供正则表达式的编程语言方面的概述。|
|[正则表达式对象模型](the-regular-expression-object-model.md)|提供演示如何使用正则表达式类的信息和代码示例。|
|[正则表达式语言 - 快速参考](regular-expression-language-quick-reference.md)|提供有关可用来定义正则表达式的字符集、运算符和构造的信息。|

## <a name="reference"></a>参考

- <xref:System.Text.RegularExpressions?displayProperty=nameWithType>
