---
title: .NET 正则表达式中的回溯
description: 了解如何在正则表达式模式匹配中控制回溯的使用。
ms.date: 11/12/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- .NET regular expressions, backtracking
- alternative matching patterns
- optional matching patterns
- searching with regular expressions, backtracking
- pattern-matching with regular expressions, backtracking
- backtracking
- regular expressions [.NET], backtracking
- strings [.NET], regular expressions
- parsing text with regular expressions, backtracking
ms.assetid: 34df1152-0b22-4a1c-a76c-3c28c47b70d8
ms.openlocfilehash: 5c6d9d2e048c2dd89cf18ff7148050ddb6813f40
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95699698"
---
# <a name="backtracking-in-regular-expressions"></a>正则表达式中的回溯

当正则表达式模式包含可选[限定符](quantifiers-in-regular-expressions.md)或[备用构造](alternation-constructs-in-regular-expressions.md)时，会发生回溯，并且正则表达式引擎会返回以前保存的状态，以继续搜索匹配项。 回溯是正则表达式的强大功能的中心；它使得表达式强大、灵活，可以匹配非常复杂的模式。 同时，这种强大功能需要付出一定代价。 通常，回溯是影响正则表达式引擎性能的单个最重要的因素。 幸运的是，开发人员可以控制正则表达式引擎的行为及其使用回溯的方式。 本主题说明回溯的工作方式以及如何对其进行控制。  
  
> [!NOTE]
> 通常情况下，非确定性有限自动机 (NFA) 引擎（如 .NET 正则表达式引擎）会将构造快速高效的正则表达式的职责交给开发人员。  

## <a name="linear-comparison-without-backtracking"></a>不使用回溯的线性比较  

 如果正则表达式模式没有可选限定符或替换构造，正则表达式引擎将以线性时间执行。 也就是说，在正则表达式引擎将模式中的第一个语言元素与输入字符串中的文本匹配后，它尝试将模式中的下一个语言元素与输入字符串中的下一个字符或字符组匹配。 此操作将继续，直至匹配成功或失败。 在任何一种情况下，在同一时间，正则表达式引擎都比输入字符串中提前一个字符。  
  
 下面的示例进行了这方面的演示。 正则表达式 `e{2}\w\b` 查找字母 "e" 后跟任意单词字符再后跟单词边界的两个匹配项。  
  
 [!code-csharp[Conceptual.RegularExpressions.Backtracking#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/cs/backtracking1.cs#1)]
 [!code-vb[Conceptual.RegularExpressions.Backtracking#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/vb/backtracking1.vb#1)]  
  
 尽管此正则表达式包括限定符 `{2}`，但它仍以线性方式进行计算。 由于 `{2}` 不是可选限定符，因此该正则表达式引擎不回溯；它指定确切数字，而不是前一个子表达式必须匹配的可变次数。 因此，正则表达式引擎尝试使正则表达式模式与输入字符串匹配，如下表所示。  
  
|操作|在模式中的位置|在字符串中的位置|结果|  
|---------------|-------------------------|------------------------|------------|  
|1|e|“needing a reed”（索引 0）|无匹配。|  
|2|e|“eeding a reed”（索引 1）|可能匹配。|  
|3|e{2}|“eding a reed”（索引 2）|可能匹配。|  
|4|\w|“ding a reed”（索引 3）|可能匹配。|  
|5|\b|“ing a reed”（索引 4）|可能的匹配失败。|  
|6|e|“eding a reed”（索引 2）|可能匹配。|  
|7|e{2}|“ding a reed”（索引 3）|可能的匹配失败。|  
|8|e|“ding a reed”（索引 3）|匹配失败。|  
|9|e|“ing a reed”（索引 4）|无匹配。|  
|10|e|“ng a reed”（索引 5）|无匹配。|  
|11|e|“g a reed”（索引 6）|无匹配。|  
|12|e|“a reed”（索引 7）|无匹配。|  
|13|e|“a reed”（索引 8）|无匹配。|  
|14|e|“reed”（索引 9）|无匹配。|  
|15|e|“reed”（索引 10）|无匹配|  
|16|e|“eed”（索引 11）|可能匹配。|  
|17|e{2}|“ed”（索引 12）|可能匹配。|  
|18|\w|“d”（索引 13）|可能匹配。|  
|19|\b|“”(索引 14)|匹配。|  
  
 如果正则表达式模式中不包括可选限定符或替换构造，则将正则表达式模式与输入字符串匹配所需要的最大比较数大致等于输入字符串中的字符数。 在这种情况下，正则表达式引擎通过 19 次比较来标识该 13 个字符的字符串中可能的匹配项。  换句话说，如果正则表达式引擎不包含可选限定符或替换构造，则正则表达式引擎将以近线性时间运行。

## <a name="backtracking-with-optional-quantifiers-or-alternation-constructs"></a>使用可选限定符或替换构造的回溯  

 当正则表达式模式包含可选限定符或替换构造时，输入字符串的计算将不再为线性。 使用 NFA 引擎的模式匹配由正则表达式中的语言元素驱动，而不是由输入字符串中要匹配的字符驱动。 因此，正则表达式引擎将尝试完全匹配可选或可替换的子表达式。 当它前进到子表达式中的下一个语言元素并且匹配不成功时，正则表达式引擎可放弃其成功匹配的一部分，并返回以前保存的与将正则表达式作为一个整体与输入字符串匹配有关的状态。 返回到以前保存状态以查找匹配的这一过程称为回溯。  
  
 例如，考虑正则表达式模式 `.*(es)`，它匹配字符“es”以及它前面的所有字符。 如下面的示例所示，如果输入字符串为“Essential services are provided by regular expressions.”，模式将匹配“expressions”之前且包括“es”在内的整个字符串。  
  
 [!code-csharp[Conceptual.RegularExpressions.Backtracking#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/cs/backtracking2.cs#2)]
 [!code-vb[Conceptual.RegularExpressions.Backtracking#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/vb/backtracking2.vb#2)]  
  
 为此，正则表达式引擎按如下所示使用回溯：  
  
- 它将 `.*` （它对应于出现零次、一次或多次任意字符）与整个输入字符串匹配。  
  
- 它尝试在正则表达式模式中匹配“e”。 但是，输入字符串没有剩余的可用字符来匹配。  
  
- 它回溯到上一次成功的匹配“Essential services are provided by regular expressions”，并尝试将“e”与句尾的句号匹配。 匹配失败。  
  
- 它继续回溯到上一个成功匹配，一次一个字符，直至临时匹配的子字符串为“Essential services are provided by regular expr”。 然后，它将模式中的“e”与“expressions”中的第二个“e”进行比较，并找到匹配。  
  
- 它将模式中的“s”与匹配的“e”字符之后的“s”（“expressions”中的第一个“s”）进行比较。 匹配成功。  
  
 当您使用回溯将正则表达式模式与输入字符串（长度为 55 个字符）匹配时，需要执行 67 次比较操作。 通常，如果正则表达式模式包括单个替换构造或单个可选限定符，则匹配模式所需要的比较操作数大于输入字符串中字符数的两倍。

## <a name="backtracking-with-nested-optional-quantifiers"></a>使用嵌套的可选限定符的回溯  

 如果模式中包括大量替换构造、嵌套的替换构造（或最常见的是嵌套的可选限定符），则匹配正则表达式模式所需要的比较操作数会成指数增加。 例如，正则表达式模式 `^(a+)+$` 用于匹配包含一个或多个“a”字符的完整字符串。 该示例提供了两个长度相同的输入字符串，但只有第一个字符串与模式匹配。 <xref:System.Diagnostics.Stopwatch?displayProperty=nameWithType> 类用于确定匹配操作所需的时间。  
  
 [!code-csharp[Conceptual.RegularExpressions.Backtracking#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/cs/backtracking3.cs#3)]
 [!code-vb[Conceptual.RegularExpressions.Backtracking#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/vb/backtracking3.vb#3)]  
  
 正如示例输出所示，正则表达式引擎查找输入字符串与模式不匹配所需的时间大约为标识匹配字符串所需时间的两倍。 这是因为，不成功的匹配始终表示最糟糕的情况。 正则表达式引擎必须使用正则表达式来遵循通过数据的所有可能路径，然后才能得出匹配不成功的结论，嵌套的括号会创建通过数据的许多其他路径。 正则表达式引擎通过执行以下操作来确定第二个字符串与模式不匹配：  
  
- 它检查到正位于字符串开头，然后将字符串中的前五个字符与模式 `a+`匹配。 然后确定字符串中没有其他成组的“a”字符。 最后，它测试是否位于字符串结尾。 由于还有一个附加字符保留在字符串中，所以匹配失败。 这一失败的匹配需要进行 9 次比较。 正则表达式引擎也从其“a”（我们将其称为匹配 1）、“aa”（匹配 2）、“aaa”（匹配 3）和“aaaa”（匹配 4）的匹配中保存状态信息。  
  
- 它返回到以前保存的匹配 4。 它确定没有一个附加的“a”字符可分配给其他捕获的组。 最后，它测试是否位于字符串结尾。 由于还有一个附加字符保留在字符串中，所以匹配失败。 该失败的匹配需要进行 4 次比较。 到目前为止，总共执行了 13 次比较。  
  
- 它返回到以前保存的匹配 3。 它确定有两个附加的“a”字符可分配给其他捕获的组。 但是，字符串末尾测试失败。 然后，它返回 match3 并尝试在两个附加的捕获组中匹配两个附加的“a”字符。 字符串末尾测试仍失败。 这些失败的匹配需要进行 12 次比较。 到目前为止，总共执行了 25 次比较。  
  
 输入字符串与正则表达式的比较将以此方式继续，直到正则表达式引擎已尝试所有可能的匹配组合然后得出无匹配的结论。 因为存在嵌套的限定符，所以此比较为 O(2<sup>n</sup>) 或指数操作，其中 n 是输入字符串中的字符数  。 这意味着在最糟糕的情况下，包含 30 个字符的输入字符串大约需要进行 1,073,741,824 次比较，包含 40 个字符的输入字符串大约需要进行 1,099,511,627,776 次比较。 如果使用上述长度甚至更长的字符串，则正则表达式方法在处理与正则表达式模式不匹配的输入时，会需要超长的时间来完成。

## <a name="controlling-backtracking"></a>控制回溯  

 通过回溯可以创建强大、灵活的正则表达式。 但如上一节所示，回溯在提供这些优点的同时，可能也会使性能差的无法接受。 若要防止过度回溯，则应在实例化 <xref:System.Text.RegularExpressions.Regex> 对象或调用静态正则表达式匹配方法时定义超时间隔。 下一节中将对此进行讨论。 此外，.NET 支持下面三个正则表达式语言元素，它们限制或禁止回溯、支持复杂的正则表达式，且不或几乎不损害性能：[原子组](#atomic-groups)、[回顾后发断言](#lookbehind-assertions)和[先行断言](#lookahead-assertions)。 有关每个语言元素的详细信息，请参见 [分组构造](grouping-constructs-in-regular-expressions.md)。  

### <a name="defining-a-time-out-interval"></a>定义超时间隔  

 从 .NET Framework 4.5 开始，可以设置超时值，该值表示正则表达式引擎在放弃尝试并引发 <xref:System.Text.RegularExpressions.RegexMatchTimeoutException> 异常之前将搜索单个匹配项的最长间隔。 你可以通过向实例正则表达式的 <xref:System.TimeSpan> 构造函数提供 <xref:System.Text.RegularExpressions.Regex.%23ctor%28System.String%2CSystem.Text.RegularExpressions.RegexOptions%2CSystem.TimeSpan%29> 值来指定超时间隔。 此外，每种静态模式匹配方法都具有带 <xref:System.TimeSpan> 参数的重载，该参数允许你指定超时值。 默认情况下，超时间隔设置为 <xref:System.Text.RegularExpressions.Regex.InfiniteMatchTimeout?displayProperty=nameWithType> 且正则表达式引擎不会超时。  
  
> [!IMPORTANT]
> 如果正则表达式依赖回溯，建议你始终设置超时间隔。  
  
 <xref:System.Text.RegularExpressions.RegexMatchTimeoutException> 异常指示正则表达式引擎无法在指定的超时间隔内找到匹配项，但不指示引发异常的原因。 原因可能是过度回溯，但也可能是超时间隔设置得过小（在引发异常时产生系统负载）。 在处理异常时，你可以选择放弃与输入字符串的进一步匹配或增大超时间隔，然后重试匹配操作。  
  
 例如，下面的代码调用 <xref:System.Text.RegularExpressions.Regex.%23ctor%28System.String%2CSystem.Text.RegularExpressions.RegexOptions%2CSystem.TimeSpan%29> 构造函数来实例化超时值为 1 秒的 <xref:System.Text.RegularExpressions.Regex> 对象。 正则表达式模式 `(a+)+$`（与行尾的一个或多个“a”字符的一个或多个序列匹配）受过度回溯的约束。 如果引发 <xref:System.Text.RegularExpressions.RegexMatchTimeoutException> 异常，该示例会将超时值增大到三秒最长间隔。 之后，它放弃尝试匹配模式。  
  
 [!code-csharp[System.Text.RegularExpressions.Regex.ctor#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.text.regularexpressions.regex.ctor/cs/ctor1.cs#1)]
 [!code-vb[System.Text.RegularExpressions.Regex.ctor#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.text.regularexpressions.regex.ctor/vb/ctor1.vb#1)]  

### <a name="atomic-groups"></a>原子组

 `(?>` subexpression`)` 语言元素禁止在子表达式中使用回溯  。 成功匹配后，它不会将其匹配项的任何部分提供给后续回溯。 例如，在模式 `(?>\w*\d*)1` 中，如果无法匹配 `1`，则即使意味着会允许 `1` 成功匹配，`\d*` 也不会放弃其任何匹配项。 原子组可帮助防止与失败匹配关联的性能问题。
  
 下面的示例演示在使用嵌套的限定符时禁止回溯如何改进性能。 它测量正则表达式引擎确定输入字符串与两个正则表达式不匹配所需要的时间。 第一个正则表达式使用回溯尝试匹配一个字符串，在该字符串中，一个或多个十六进制数出现了一次或多次，然后依次为冒号、一个或多个十六进制数、两个冒号。 第二个正则表达式与第一个相同，不同之处是它禁用了回溯。 如该示例输出所示，禁用回溯对性能的改进非常显著。  
  
 [!code-csharp[Conceptual.RegularExpressions.Backtracking#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/cs/backtracking4.cs#4)]
 [!code-vb[Conceptual.RegularExpressions.Backtracking#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/vb/backtracking4.vb#4)]  

### <a name="lookbehind-assertions"></a>回顾断言  

 .NET 包括两个语言元素（`(?<=`subexpression  `)` 和 `(?<!`subexpression  `)`），它们与输入字符串中之前的一个或多个字符匹配。 这两个语言元素都是零宽度断言；也就是说，它们通过 *subexpression* 而不是前移或回溯来确定当前字符之前紧挨着的一个或多个字符是否匹配。  
  
 `(?<=` subexpression`)` 是正回顾断言；也就是说，当前位置之前的一个或多个字符必须与 subexpression 匹配   。 `(?<!`*subexpression*`)` 是负回顾断言；也就是说，当前位置之前的一个或多个字符不得与 *subexpression* 匹配。 当 *subexpression* 为前一个子表达式的子集时，正回顾断言和负回顾断言都最为有用。  
  
 下面的示例使用两个相当的正则表达式模式，验证电子邮件地址中的用户名。 第一个模式由于过多使用回溯，性能极差。 第二个模式通过将嵌套的限定符替换为正回顾断言来修改第一个正则表达式。 该示例的输出显示 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> 方法的执行时间。  
  
 [!code-csharp[Conceptual.RegularExpressions.Backtracking#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/cs/backtracking5.cs#5)]
 [!code-vb[Conceptual.RegularExpressions.Backtracking#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/vb/backtracking5.vb#5)]  
  
 第一个正则表达式模式 `^[0-9A-Z]([-.\w]*[0-9A-Z])*@`的定义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`^`|从字符串开头开始匹配。|  
|`[0-9A-Z]`|匹配字母数字字符。 因为 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> 方法是使用 <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> 选项调用的，所以此比较不区分大小写。|  
|`[-.\w]*`|匹配零个、一个或多个连字符、句号或单词字符。|  
|`[0-9A-Z]`|匹配字母数字字符。|  
|`([-.\w]*[0-9A-Z])*`|匹配以下零个或多个事例：即零个或多个连字符、句号或单词字符后跟一个字母数字字符的组合。 这是第一个捕获组。|  
|`@`|匹配 at 符号（“\@”）。|  
  
 第二个正则表达式模式 `^[0-9A-Z][-.\w]*(?<=[0-9A-Z])@`使用正回顾断言。 其定义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`^`|从字符串开头开始匹配。|  
|`[0-9A-Z]`|匹配字母数字字符。 因为 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> 方法是使用 <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> 选项调用的，所以此比较不区分大小写。|  
|`[-.\w]*`|匹配零个或多个连字符、句号或单词字符。|  
|`(?<=[0-9A-Z])`|回顾最后一个匹配的字符，如果该字符是字母数字字符，则继续匹配。 请注意，字母数字字符是由句号、连字符和所有单词字符构成的集合的子集。|  
|`@`|匹配 at 符号（“\@”）。|  

### <a name="lookahead-assertions"></a>预测先行断言  

 .NET 包括两个语言元素（`(?=`subexpression  `)` 和 `(?!`subexpression  `)`），它们与输入字符串中接下来的一个或多个字符匹配。 这两个语言元素都是零宽度断言；也就是说，它们通过 *subexpression* 而不是前移或回溯来确定当前字符之后紧挨着的一个或多个字符是否匹配。  
  
 `(?=` subexpression`)` 是正预测先行断言；也就是说，当前位置之后的一个或多个字符必须与 subexpression 匹配   。 `(?!`*subexpression*`)` 是负预测先行断言；也就是说，当前位置之后的一个或多个字符不得与 *subexpression* 匹配。 当 *subexpression* 为下一个子表达式的子集时，正预测先行断言和负预测先行断言都最为有用。  
  
 下面的示例使用两个可验证完全限定的类型名称的等效正则表达式模式。 第一个模式由于过多使用回溯，性能极差。 第二个模式通过将嵌套的限定符替换为正预测先行断言来修改第一个正则表达式。 该示例的输出显示 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> 方法的执行时间。  
  
 [!code-csharp[Conceptual.RegularExpressions.Backtracking#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/cs/backtracking6.cs#6)]
 [!code-vb[Conceptual.RegularExpressions.Backtracking#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regularexpressions.backtracking/vb/backtracking6.vb#6)]  
  
 第一个正则表达式模式 `^(([A-Z]\w*)+\.)*[A-Z]\w*$`的定义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`^`|从字符串开头开始匹配。|  
|`([A-Z]\w*)+\.`|对后跟零个或多个单词字符、句点的字母字符 (A-Z) 匹配一次或多次。 因为 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> 方法是使用 <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> 选项调用的，所以此比较不区分大小写。|  
|`(([A-Z]\w*)+\.)*`|对前一个模式匹配零次或多次。|  
|`[A-Z]\w*`|匹配后跟零个或多个单词字符的字母字符。|  
|`$`|在输入字符串末尾结束匹配。|  
  
 第二个正则表达式模式 `^((?=[A-Z])\w+\.)*[A-Z]\w*$`使用正预测先行断言。 其定义如下表所示。  
  
|模式|描述|  
|-------------|-----------------|  
|`^`|从字符串开头开始匹配。|  
|`(?=[A-Z])`|预测先行到第一个字符，如果它是字母 (A-Z)，则继续匹配。 因为 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> 方法是使用 <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> 选项调用的，所以此比较不区分大小写。|  
|`\w+\.`|匹配后跟句号的一个或多个单词字符。|  
|`((?=[A-Z])\w+\.)*`|对一个或多个单词字符后跟句号的模式进行一次或多次匹配。 初始单词字符必须为字母字符。|  
|`[A-Z]\w*`|匹配后跟零个或多个单词字符的字母字符。|  
|`$`|在输入字符串末尾结束匹配。|  
  
## <a name="see-also"></a>请参阅

- [.NET 正则表达式](regular-expressions.md)
- [正则表达式语言 - 快速参考](regular-expression-language-quick-reference.md)
- [数量词](quantifiers-in-regular-expressions.md)
- [替换构造](alternation-constructs-in-regular-expressions.md)
- [分组构造](grouping-constructs-in-regular-expressions.md)
