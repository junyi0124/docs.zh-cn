---
title: 正则表达式选项
description: 了解如何使用 .NET 中的正则表达式方法，例如不区分大小写的匹配、多行模式和从右到左模式。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- regular expressions, options
- constructs, options
- .NET regular expressions, options
- inline option constructs
- options parameter
ms.assetid: c82dc689-7e82-4767-a18d-cd24ce5f05e9
ms.openlocfilehash: a77409476c8b2c1b32429118edbe6ad6542a7b09
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94828941"
---
# <a name="regular-expression-options"></a>正则表达式选项

默认情况下，正则表达式模式中带有任意文本字符的输入字符串比较区分大小写，正则表达式模式中的空白将被解释为文本空白字符且正则表达式中的捕获组通过隐式和显式命名。 可通过指定正则表达式选项修改默认正则表达式行为的这些和其他数个方面。 列于下表的这些选项，可将内联作为正则表达式的一部分包含，或者可将它们作为 <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType> 枚举值提供给 <xref:System.Text.RegularExpressions.RegexOptions?displayProperty=nameWithType> 类构造函数或静态模式匹配方法。

|RegexOptions 成员|内联字符|效果|
|-------------------------|----------------------|------------|
|<xref:System.Text.RegularExpressions.RegexOptions.None>|不可用|使用默认行为。 有关更多信息，请参见[默认选项](#default-options)。|
|<xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase>|`i`|使用不区分大小写的匹配。 有关更多信息，请参见[不区分大小写的匹配](#case-insensitive-matching)。|
|<xref:System.Text.RegularExpressions.RegexOptions.Multiline>|`m`|使用多线模式，其中 `^` 和 `$` 匹配每行的开头和末尾（不是输入字符串的开头和末尾）。 有关更多信息，请参见[多行模式](#multiline-mode)。|
|<xref:System.Text.RegularExpressions.RegexOptions.Singleline>|`s`|使用单行模式，其中的句号 (.) 匹配每个字符（而不是除了 `\n` 以外的每个字符)。 有关详细信息，请参阅[单行模式](#single-line-mode)。|
|<xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture>|`n`|不捕获未命名的组。 唯一有效的捕获是显式命名或编号的 `(?<`name`>` subexpression`)` 形式的组 。 有关更多信息，请参见[仅显式捕获](#explicit-captures-only)。|
|<xref:System.Text.RegularExpressions.RegexOptions.Compiled>|不可用|将正则表达式编译为程序集。 有关更多信息，请参见[已编译的正则表达式](#compiled-regular-expressions)。|
|<xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace>|`x`|从模式中排除保留的空白并启用数字符号 (`#`) 后的注释。 有关更多信息，请参见[忽略空白](#ignore-white-space)。|
|<xref:System.Text.RegularExpressions.RegexOptions.RightToLeft>|不可用|更改搜索方向。 搜索是从右向左而不是从左向右进行。 有关更多信息，请参见[从右向左模式](#right-to-left-mode)。|
|<xref:System.Text.RegularExpressions.RegexOptions.ECMAScript>|不可用|为表达式启用符合 ECMAScript 的行为。 有关更多信息，请参见 [ECMAScript 匹配行为](#ecmascript-matching-behavior)。|
|<xref:System.Text.RegularExpressions.RegexOptions.CultureInvariant>|不可用|忽略语言的区域性差异。 有关更多信息，请参见[使用固定区域性的比较](#comparison-using-the-invariant-culture)。|

## <a name="specifying-the-options"></a>指定选项

可以用下面三种方法之一指定正则表达式的选项：

- 在 `options` 类构造函数或静态（在 Visual Basic 中为 <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType>）模式匹配方法的 `Shared` 参数中，如 <xref:System.Text.RegularExpressions.Regex.%23ctor%28System.String%2CSystem.Text.RegularExpressions.RegexOptions%29> 或 <xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType>。 `options` 参数是 <xref:System.Text.RegularExpressions.RegexOptions?displayProperty=nameWithType> 枚举值的按位“或”组合。

  当通过使用类构造函数的 `options` 参数，将选项提供给 <xref:System.Text.RegularExpressions.Regex> 实例时，这些选项将分配给 <xref:System.Text.RegularExpressions.RegexOptions?displayProperty=nameWithType> 属性。 然而，<xref:System.Text.RegularExpressions.RegexOptions?displayProperty=nameWithType> 属性不会在正则表达式模式本身中反映内联选项。

  下面的示例进行了这方面的演示。 在标识以字母“d”开头的单词时，它使用 `options` 方法的 <xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> 参数来启用不区分大小写匹配和忽略模式空白。

  [!code-csharp[Conceptual.Regex.Language.Options#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/example1.cs#6)]
  [!code-vb[Conceptual.Regex.Language.Options#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/example1.vb#6)]

- 通过在包含语法 `(?imnsx-imnsx)` 的正则表达式模式中应用内联选项。 该选项从选项定义为模式末尾的点应用于该模式，或应用于另一内联选项未定义选项的点。 请注意，<xref:System.Text.RegularExpressions.Regex> 实例的 <xref:System.Text.RegularExpressions.RegexOptions?displayProperty=nameWithType> 属性不会反映这些内联选项。 有关详细信息，请参阅[其他构造](miscellaneous-constructs-in-regular-expressions.md)主题。

  下面的示例进行了这方面的演示。 在标识以字母“d”开头的单词时，它使用内联选项来启用不区分大小写匹配和忽略模式空白。

  [!code-csharp[Conceptual.Regex.Language.Options#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/example1.cs#7)]
  [!code-vb[Conceptual.Regex.Language.Options#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/example1.vb#7)]

- 通过在包含语法 `(?imnsx-imnsx:`subexpression`)` 的正则表达式模式的特定分组构造中，应用内联选项。 一组选项前面没有符号用于打开该设置；一组选项前面的减号用于关闭该设置。 （无论选项是启用还是禁用，`?` 都是所需的语言构造语法的固定部分。）选项只应用于该组。 有关详细信息，请参阅[分组构造](grouping-constructs-in-regular-expressions.md)。

  下面的示例进行了这方面的演示。 在标识以字母“d”开头的单词时，它使用分组构造中的内联选项来启用不区分大小写匹配和忽略模式空白。

  [!code-csharp[Conceptual.Regex.Language.Options#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/example1.cs#8)]
  [!code-vb[Conceptual.Regex.Language.Options#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/example1.vb#8)]

如果选项指定为内联，一个选项或一组选项前面的减号 (`-`) 用于关闭这些选项。 例如，内联构造 `(?ix-ms)` 将打开 <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> 和 <xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace?displayProperty=nameWithType> 选项而关闭 <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> 和 <xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType> 选项。 默认情况下，关闭所有正则表达式选项。

> [!NOTE]
> 如果构造函数或方法调用的 `options` 形参中指定的正则表达式选项与正则表达式模式中的内联指定的选项冲突，那么将使用该内联选项。

可为下面的五个正则表达式选项同时设置选项形参和内联：

- <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace?displayProperty=nameWithType>

可为下面的五个正则表达式选项设置使用 `options` 形参，但不能为其设置内联：

- <xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.Compiled?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.RightToLeft?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.CultureInvariant?displayProperty=nameWithType>

- <xref:System.Text.RegularExpressions.RegexOptions.ECMAScript?displayProperty=nameWithType>

## <a name="determining-the-options"></a>确定选项

可以确定向 <xref:System.Text.RegularExpressions.Regex> 对象提供哪些选项，在通过检索只读 <xref:System.Text.RegularExpressions.Regex.Options%2A?displayProperty=nameWithType> 属性的值将其实例化时。 该属性尤其可用于确定为编译的正则表达式定义的选项，该正则表达式由 <xref:System.Text.RegularExpressions.Regex.CompileToAssembly%2A?displayProperty=nameWithType> 方法创建。

要测试除 <xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType> 之外的任何选项的存在，使用 <xref:System.Text.RegularExpressions.Regex.Options%2A?displayProperty=nameWithType> 属性的值和需要的 <xref:System.Text.RegularExpressions.RegexOptions> 值执行 AND 运算。 然后测试结果是否等于该 <xref:System.Text.RegularExpressions.RegexOptions> 值。 下面的示例测试是否设置了 <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> 选项。

[!code-csharp[Conceptual.Regex.Language.Options#19](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/determine1.cs#19)]
[!code-vb[Conceptual.Regex.Language.Options#19](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/determine1.vb#19)]

要测试 <xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType>，确定 <xref:System.Text.RegularExpressions.Regex.Options%2A?displayProperty=nameWithType> 属性的值是否等于 <xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType>，如以下示例所示。

[!code-csharp[Conceptual.Regex.Language.Options#20](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/determine1.cs#20)]
[!code-vb[Conceptual.Regex.Language.Options#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/determine1.vb#20)]

下面各部分列出了 .NET 正则表达式支持的选项。

## <a name="default-options"></a>默认选项

<xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType> 选项指示尚未指定任何选项，正则表达式引擎使用其默认行为。 这包括：

- 该模式将被解释为一个规范而非 ECMAScript 正则表达式。

- 从左到右在输入字符串中匹配的正则表达式模式。

- 比较区分大小写。

- `^` 和 `$` 语言元素与输入字符串的开头和结尾匹配。

- `.` 语言元素与除 `\n` 之外的每个字符匹配。

- 正则表达式模式中的任意空白均解释为文本空白字符。

- 将模式与输入字符串进行比较时将使用当前区域性的约定。

- 正则表达式模式中的捕获组可以是隐式的，也可以是显式的。

> [!NOTE]
> <xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType> 选项没有内联等效项。 当内联应用正则表达式选项时，默认行为通过关闭特定选项以逐个选项方式存储。 例如， `(?i)` 打开不区分大小写的比较，`(?-i)` 还原默认区分大小写的比较。

因为 <xref:System.Text.RegularExpressions.RegexOptions.None?displayProperty=nameWithType> 选项表示正则表达式引擎的默认行为，因此它很少显式地在方法调用中指定。 而改为调用构造函数或静态模式匹配的方法，其中不包含 `options` 参数。

## <a name="case-insensitive-matching"></a>不区分大小写的匹配

<xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase> 选项或 `i` 内联选项提供了不区分大小写匹配。 默认情况下，使用当前区域性的大小写约定。

下面的示例定义与以“the”开头的所有单词匹配的正则表达式模式 `\bthe\w*\b`。 因为对 <xref:System.Text.RegularExpressions.Regex.Match%2A> 方法的第一次调用使用默认区分大小写的比较，因此输出会指示以字符串“The”开头的句子不匹配。 通过将选项设置为 <xref:System.Text.RegularExpressions.Regex.Match%2A>，调用 <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase> 方法时对其进行匹配。

[!code-csharp[Conceptual.Regex.Language.Options#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/case1.cs#1)]
[!code-vb[Conceptual.Regex.Language.Options#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/case1.vb#1)]

下面的示例修改了上一示例中的正则表达式模式，以使用内联选项而不是 `options` 参数来提供不区分大小写的比较。 第一个模式定义只应用于字符串“the”中的字母“t”的分组构造中的不区分大小写的选项。 因为选项构造在模式的开始处出现，所以第二个模式将不区分大小写的选项应用于整个正则表达式。

[!code-csharp[Conceptual.Regex.Language.Options#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/case2.cs#2)]
[!code-vb[Conceptual.Regex.Language.Options#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/case2.vb#2)]

## <a name="multiline-mode"></a>多行模式

<xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> 选项或 `m` 内联选项使正则表达式引擎能够处理由多个行组成的输入字符串。 它更改了 `^` 和 `$` 语言元素的解释，以使它们分别与行的开头和结尾匹配，而不是与输入字符串的开头和结尾匹配。

默认情况下，`$` 仅与输入字符串的末尾匹配。 如果指定了 <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> 选项，它将与换行符 (`\n`) 或输入字符串的末尾匹配。 但是，它并不与回车符/换行符的组合匹配。 若要成功匹配它们，使用子表达式 `\r?$` 只替代 `$`。

下面的示例提取投手的姓名和分数，并将它们添加到 <xref:System.Collections.Generic.SortedList%602> 集合中，该集合将按降序顺序对它们进行排序。 调用了两次 <xref:System.Text.RegularExpressions.Regex.Matches%2A> 方法。 在第一个方法调用中，正则表达式是 `^(\w+)\s(\d+)$`，且没有设置任何选项。 如输出所示，因为正则表达式引擎与输入模式及输入字符串的开头和结尾均不匹配，因此没有找到匹配。 在第二个方法调用中，正则表达式更改为 `^(\w+)\s(\d+)\r?$`，选项设置为 <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType>。 如输出所示，姓名和分数成功匹配，且分数按降序顺序显示。

[!code-csharp[Conceptual.Regex.Language.Options#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/multiline1.cs#3)]
[!code-vb[Conceptual.Regex.Language.Options#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/multiline1.vb#3)]

正则表达式模式 `^(\w+)\s(\d+)\r*$` 的定义如下表所示。

|模式|描述|
|-------------|-----------------|
|`^`|从行首开始。|
|`(\w+)`|匹配一个或多个单词字符。 这是第一个捕获组。|
|`\s`|与空白字符匹配。|
|`(\d+)`|匹配一个或多个十进制数字。 这是第二个捕获组。|
|`\r?`|与零个或一个回车符匹配。|
|`$`|在行尾结束。|

下面的示例与上一示例等效，不同之处是下面的示例使用内联选项 `(?m)` 来设置多行选项。

[!code-csharp[Conceptual.Regex.Language.Options#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/multiline2.cs#4)]
[!code-vb[Conceptual.Regex.Language.Options#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/multiline2.vb#4)]

## <a name="single-line-mode"></a>单行模式

<xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType> 选项或 `s` 内联选项导致正则表达式引擎将输入字符串视为由单行组成。 它通过更改句号 (`.`) 语言元素的行为，使其与每个字符匹配，而不是与除换行符 `\n` 或 \u000A 之外的每个字符匹配来执行此操作。

下面的示例演示了在使用 `.` 选项时如何更改 <xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType> 语言元素的行为。 正则表达式 `^.+` 在字符串开头开始并匹配每个字符。 默认情况下，匹配在第一行的结尾结束；正则表达式模式匹配回车符、`\r` 或 \u000D，但不匹配 `\n`。 由于 <xref:System.Text.RegularExpressions.RegexOptions.Singleline?displayProperty=nameWithType> 选项将整个输入字符串解释为单行，因此它匹配输入字符串中的每个字符，包括 `\n`。

[!code-csharp[Conceptual.Regex.Language.CharacterClasses#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.characterclasses/cs/any2.cs#5)]
[!code-vb[Conceptual.Regex.Language.CharacterClasses#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.characterclasses/vb/any2.vb#5)]

下面的示例与上一示例等效，不同之处是下面的示例使用内联选项 `(?s)` 来启用单行模式。

[!code-csharp[Conceptual.Regex.Language.Options#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/singleline1.cs#5)]
[!code-vb[Conceptual.Regex.Language.Options#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/singleline1.vb#5)]

## <a name="explicit-captures-only"></a>仅显式捕获

默认情况下，通过在正则表达式模式中使用括号来定义捕获组。 通过 `(?<`name`>`subexpression`)` 语言选项为命名组指定名称或编号，而未命名组按索引进行访问。 在 <xref:System.Text.RegularExpressions.GroupCollection> 对象中，未命名的组先于已命名的组。

分组构造通常仅用于将限定符应用于多个语言元素，而非应用于捕获的子字符串。 例如，如果下面的正则表达式：

`\b\(?((\w+),?\s?)+[\.!?]\)?`

旨在仅从文档提取末尾有句号、感叹点或问号的句子，仅产生的句子（这由 <xref:System.Text.RegularExpressions.Match> 对象表示）有意义。 集合中的各单词不是。

随后未使用的捕获组可能很昂贵，因为正则表达式引擎必须填充 <xref:System.Text.RegularExpressions.GroupCollection> 和 <xref:System.Text.RegularExpressions.CaptureCollection> 集合对象。 作为替换方法，也可以使用 <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType> 选项或 `n` 内联选项，指定显式命名的唯一有效捕获，或由 `(?<`名称`>` 子表达式`)` 构造指定的编号组 。

以下示例显示 `\b\(?((\w+),?\s?)+[\.!?]\)?` 正则表达式模式在 <xref:System.Text.RegularExpressions.Regex.Match%2A> 方法被调用且没有 <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType> 选项时返回的匹配信息。 如第一个方法调用输出所示，正则表达式引擎使用有关已捕获的子字符串的信息完全填充 <xref:System.Text.RegularExpressions.GroupCollection> 和 <xref:System.Text.RegularExpressions.CaptureCollection> 集合对象。 因为第二个方法使用设置为 `options` 的 <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType> 进行调用，所以它不会捕获有关组的信息。

[!code-csharp[Conceptual.Regex.Language.Options#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/explicit1.cs#9)]
[!code-vb[Conceptual.Regex.Language.Options#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/explicit1.vb#9)]

正则表达式模式 `\b\(?((?>\w+),?\s?)+[\.!?]\)?` 的定义如下表所示。

|模式|描述|
|-------------|-----------------|
|`\b`|在单词边界处开始。|
|`\(?`|匹配左括号（“(”）的零或一个匹配项。|
|`(?>\w+),?`|匹配一个或多个单词字符，后跟零或一个逗号。 当匹配单词字符请不要回溯。|
|`\s?`|匹配零个或一个空白字符。|
|`((\w+),?\s?)+`|一次或多次匹配一个或多个单词字符、零或一个逗号以及零或一个空白字符的组合。|
|`[\.!?]\)?`|与后无右括号或后跟一个右括号（“)”）的三个标点符号匹配。|

还可以使用 `(?n)` 内联元素来禁止自动捕获。 以下示例修改了上一示例中的正则表达式模式，使用的是内联元素 `(?n)` 而非 <xref:System.Text.RegularExpressions.RegexOptions.ExplicitCapture?displayProperty=nameWithType> 选项。

[!code-csharp[Conceptual.Regex.Language.Options#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/explicit2.cs#10)]
[!code-vb[Conceptual.Regex.Language.Options#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/explicit2.vb#10)]

最后，可以使用内联组元素 `(?n:)` 禁止逐组进行自动捕获。 下面的示例修改了之前的模式，以取消外部组 `((?>\w+),?\s?)` 中的非命名捕获。 请注意，这也取消了内部组中的非命名捕获。

[!code-csharp[Conceptual.Regex.Language.Options#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/explicit3.cs#11)]
[!code-vb[Conceptual.Regex.Language.Options#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/explicit3.vb#11)]

## <a name="compiled-regular-expressions"></a>已编译的正则表达式

默认情况下，.NET 中的正则表达式会有解释。 当实例化 <xref:System.Text.RegularExpressions.Regex> 对象或者调用静态 <xref:System.Text.RegularExpressions.Regex> 方法时，将把正则表达式模式解析为一组自定义操作代码，并且解释器使用这些操作代码来运行正则表达式。 这涉及一个权衡：初始化正则表达式引擎的成本通过运行时性能的消耗而最小化。

通过使用 <xref:System.Text.RegularExpressions.RegexOptions.Compiled?displayProperty=nameWithType> 选项可以使用编译的而非解释的正则表达式。 在此情况下，当模式传递给正则表达式引擎时，它将分析为一组操作码，然后转换为 Microsoft 中间语言 (MSIL)，该语言可以被直接传递到公共语言运行时。 已编译的正则表达式最大限度地提高运行时性能，代价是会影响初始化时间。

> [!NOTE]
> 仅可以通过将 <xref:System.Text.RegularExpressions.RegexOptions.Compiled?displayProperty=nameWithType> 值提供给 `options` 类构造函数或静态模式匹配方法的 <xref:System.Text.RegularExpressions.Regex> 参数来编译正则表达式。 它不可作为内联选项使用。

在调用静态和实例正则表达式时，可使用编译的正则表达式。 在静态正则表达式中，<xref:System.Text.RegularExpressions.RegexOptions.Compiled?displayProperty=nameWithType> 选项将传递到正则表达式模式匹配方法的 `options` 参数。 在实例正则表达式中，将它传递到 `options` 类构造函数的 <xref:System.Text.RegularExpressions.Regex> 参数。 在这两种情况中它将导致性能增强。

但是，这种性能改进只有在以下情况下才发生：

- 表示特定正则表达式的 <xref:System.Text.RegularExpressions.Regex> 对象可用于多个正则表达式模式匹配方法调用。

- 不允许 <xref:System.Text.RegularExpressions.Regex> 对象超出范围，以便可以重用它。

- 静态正则表达式在对正则表达式模式匹配方法的多个调用中使用。 （之所以能够提高性能，是因为静态方法调用中使用的正则表达式由正则表达式引擎缓存。）

> [!NOTE]
> <xref:System.Text.RegularExpressions.RegexOptions.Compiled?displayProperty=nameWithType> 选项与 <xref:System.Text.RegularExpressions.Regex.CompileToAssembly%2A?displayProperty=nameWithType> 方法无关，该方法创建一个特殊用途的程序集，其中包含预定义的已编译的正则表达式。

## <a name="ignore-white-space"></a>忽略空白

默认情况下，正则表达式模式中的空白非常重要；它会强制正则表达式引擎与输入字符串中的空白字符相匹配。 因此，正则表达式“`\b\w+\s`”和“`\b\w+`”是大致等效的正则表达式。 此外，正则表达式模式中出现数字符号 (#) 时，它被解释为要进行匹配的原义字符。

<xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace?displayProperty=nameWithType> 选项或 `x` 内联选项更改此默认行为，如下所示：

- 正则表达式模式中的非转义的空白将被忽略。 作为正则表达式模式的部分，必须避开空白字符（例如 `\s` 或“`\`”）。

- 数字符号 (#) 被解释为注释的开头，而不是原义字符。 正则表达式模式中的所有文本，从 # 字符到字符串的结尾都解释为注释。

但是，在下列情况下，不会忽略正则表达式中的空白字符，即使使用 <xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace?displayProperty=nameWithType> 选项也是如此：

- 始终按原义解释字符内的空格。 例如，正则表达式模式 `[ .,;:]` 匹配任意单个空白字符、句号、逗号、分号或冒号。

- 加括号的限定符内不允许有空格，如 `{`n`}`、`{`n`,}` 和 `{`n`,`m`}`。 例如，因为它包含一个空白字符，所以正则表达式模式 `\d{1, 3}` 与任何从 1 到 3 位数的数字序列不匹配。

- 引入语言元素的字符序列内不允许有空格。 例如：

  - 语言元素 `(?:`subexpression`)` 表示非捕获组，并且该元素的 `(?:` 部分不能有嵌入空格。 模式 `(? :`子表达式`)` 在运行时抛出 <xref:System.ArgumentException>，因为正则表达式引擎无法分析此模式，且模式 `( ?:`子表达式`)` 与子表达式不匹配。

  - 语言元素 `\p{`name`}` 表示一个 Unicode 类别或命名块，它不能在元素的 `\p{` 部分中包括嵌入空格。 如果你包括了空格，则该元素会在运行时引发 <xref:System.ArgumentException> 异常。

启用此选项有助于简化通常很难分析和理解的正则表达式。 它提高了可读性，并可以记录正则表达式。

下面的示例定义以下正则表达式模式：

`\b \(? ( (?>\w+) ,?\s? )+  [\.!?] \)? # Matches an entire sentence.`

此模式与[仅显式捕获](#explicit-captures-only)部分中定义的模式相似，不同之处在于它使用 <xref:System.Text.RegularExpressions.RegexOptions.IgnorePatternWhitespace?displayProperty=nameWithType> 选项忽略模式空格。

[!code-csharp[Conceptual.Regex.Language.Options#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/whitespace1.cs#12)]
[!code-vb[Conceptual.Regex.Language.Options#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/whitespace1.vb#12)]

下面的示例使用内联选项 `(?x)` 来忽略模式空白。

[!code-csharp[Conceptual.Regex.Language.Options#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/whitespace2.cs#13)]
[!code-vb[Conceptual.Regex.Language.Options#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/whitespace2.vb#13)]

## <a name="right-to-left-mode"></a>从右到左模式

默认情况下，正则表达式引擎从左向右进行搜索。 可通过使用 <xref:System.Text.RegularExpressions.RegexOptions.RightToLeft?displayProperty=nameWithType> 选项反转搜索方向。 搜索在字符串的最后一个字符位置自动开始。 对于包括起始位置参数的模式匹配方法，例如 <xref:System.Text.RegularExpressions.Regex.Match%28System.String%2CSystem.Int32%29?displayProperty=nameWithType>，起始位置是最右边字符位置（即搜索开始位置）的索引。

> [!NOTE]
> 仅能通过将 <xref:System.Text.RegularExpressions.RegexOptions.RightToLeft?displayProperty=nameWithType> 值提供给 `options` 类构造函数或静态模式匹配方法的 <xref:System.Text.RegularExpressions.Regex> 参数来提供从右到左模式。 它不可作为内联选项使用。

<xref:System.Text.RegularExpressions.RegexOptions.RightToLeft?displayProperty=nameWithType> 选项仅更改搜索方向；它不解释正则表达式模式是从右到左。 例如，正则表达式 `\bb\w+\s` 匹配以字母“b”开头的单词,且后跟一个空白字符。 在下面的示例中，输入字符串由其中包括一个或多个“b”字符的三个单词组成。 第一个单词以“b”开头，第二个单词以“b”结尾，第三个单词的中间包括两个“b”字符。 如示例输出所示，只有第一个词与正则表达式模式匹配。

[!code-csharp[Conceptual.Regex.Language.Options#17](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/righttoleft1.cs#17)]
[!code-vb[Conceptual.Regex.Language.Options#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/righttoleft1.vb#17)]

另请注意，预测先行断言（`(?=`subexpression`)` 语言元素）和回顾后发断言（`(?<=`subexpression`)` 语言元素）不会更改方向。 预测先行断言向右搜索；回顾后发断言向左搜索。 例如，正则表达式 `(?<=\d{1,2}\s)\w+,?\s\d{4}` 使用回顾后发断言测试月份名称前面的日期。 然后该正则表达式匹配月份和年份。 有关预测先行和回顾后发断言的信息，请参阅[分组构造](grouping-constructs-in-regular-expressions.md)。

[!code-csharp[Conceptual.Regex.Language.Options#18](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/righttoleft2.cs#18)]
[!code-vb[Conceptual.Regex.Language.Options#18](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/righttoleft2.vb#18)]

正则表达式模式的定义如下表所示。

|模式|描述|
|-------------|-----------------|
|`(?<=\d{1,2}\s)`|匹配项的开头必须有后跟一个空格的一个或两个十进制数字。|
|`\w+`|匹配一个或多个单词字符。|
|`,?`|匹配零个或一个逗号字符。|
|`\s`|与空白字符匹配。|
|`\d{4}`|匹配四个十进制数字。|

## <a name="ecmascript-matching-behavior"></a>ECMAScript 匹配行为

默认情况下，当正则表达式模式与输入文本匹配时，正则表达式引擎会采用规范行为。 但是，可以指示正则表达式引擎通过指定 <xref:System.Text.RegularExpressions.RegexOptions.ECMAScript?displayProperty=nameWithType> 选项使用 ECMAScript 匹配行为。

> [!NOTE]
> 仅在通过将 <xref:System.Text.RegularExpressions.RegexOptions.ECMAScript?displayProperty=nameWithType> 值提供给 `options` 类构造函数造函数或静态模式匹配方法的 <xref:System.Text.RegularExpressions.Regex> 参数后，符合 ECMAScript 的行为才可用。 它不可作为内联选项使用。

<xref:System.Text.RegularExpressions.RegexOptions.ECMAScript?displayProperty=nameWithType> 选项只能与 <xref:System.Text.RegularExpressions.RegexOptions.IgnoreCase?displayProperty=nameWithType> 和 <xref:System.Text.RegularExpressions.RegexOptions.Multiline?displayProperty=nameWithType> 选项结合使用。 在正则表达式中使用其他选项会导致 <xref:System.ArgumentOutOfRangeException>。

ECMAScript 和规范化正则表达式的行为在三个方面不同：字符类语法、自引用捕获组和八进制与反向引用的解释。

- 字符类语法。 因为规范的正则表达式支持 Unicode，却不支持 ECMAScript，ECMAScript 中的字符类具有一个受限更多的语法且某些字符类语言元素具有不同的含义。 例如，ECMAScript 不支持语言元素（例如 Unicode 类别或块元素 `\p` 和 `\P`）。 同样，使用 ECMAScript 时，与单词字符匹配的 `\w` 元素等效于 `[a-zA-Z_0-9]` 字符类，使用规范化行为时，该元素等效于 `[\p{Ll}\p{Lu}\p{Lt}\p{Lo}\p{Nd}\p{Pc}\p{Lm}]`。 有关更多信息，请参见 [字符类](character-classes-in-regular-expressions.md)。

  下面的示例阐释了规范化与 ECMAScript 模式匹配之间的差异。 它定义了正则表达式 `\b(\w+\s*)+`，该表达式与后跟空白字符的单词匹配。 由两个字符串组成的输入，其中一个字符串使用拉丁字符集，另一个则使用西里尔字符集。 如输出所示，对使用 ECMAScript 匹配的 <xref:System.Text.RegularExpressions.Regex.IsMatch%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> 方法的调用无法与西里尔文的单词匹配，而使用规范化匹配的方法调用与这些单词匹配。

  [!code-csharp[Conceptual.Regex.Language.Options#16](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/ecmascript1.cs#16)]
  [!code-vb[Conceptual.Regex.Language.Options#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/ecmascript1.vb#16)]

- 自引用捕获组。 自身具有后向引用的正则表达式捕获类必须在每次捕获迭代时得到更新。 如以下示例所示，此功能将在使用 ECMAScript 时使正则表达式 `((a+)(\1) ?)+` 与输入字符串“aa aaaa aaaaaa”匹配，但在使用规范化匹配时则不会匹配。

  [!code-csharp[Conceptual.Regex.Language.Options#21](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/ecmascript2.cs#21)]
  [!code-vb[Conceptual.Regex.Language.Options#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/ecmascript2.vb#21)]

  该正则表达式的定义如下表所示。

  |模式|描述|
  |-------------|-----------------|
  |(a+)|与字母“a”匹配一次或多次。 这是第二个捕获组。|
  |(\1)|与第一个捕获组捕获的子字符串匹配。 这是第三个捕获组。|
  |?|匹配零个或一个空白字符。|
  |((a+)(\1) ?)+|与某个模式匹配一次或多次，该模式有一个或多个“a”字符，后跟与第一个捕获组（后无空白字符或后跟一个空白字符）匹配的字符串。 这是第一个捕获组。|

- 八进制转义和反向引用间的多义性的解析。 下表总结了规范化和 ECMAScript 正则表达式在八进制与后向引用解释中的区别。

  |正则表达式|规范行为|ECMAScript 行为|
  |------------------------|------------------------|-------------------------|
  |`\0` 后跟 0 到 2 个八进制数字|解释为八进制。 例如，`\044` 总是解释为八进制值并表示“$”。|行为相同。|
  |`\` 后跟一个从 1 到 9 的数字，后面再没有任何其他十进制数字，|解释为反向引用。 例如，`\9` 始终表示后向引用 9，即使第九捕获组不存在。 如果捕获组不存在，则正则表达式分析器将引发 <xref:System.ArgumentException>。|如果存在单个十进制数字捕获组，则后向引用该数字。 否则将该值解释为文本。|
  |`\` 后跟一个从 1 到 9 的数字，后跟其他十进制数字|将数字解释为十进制值。 如果存在该捕获组，则将该表达式解释为后向引用。<br /><br /> 否则，将前导的八进制数字解释为不超过八进制值 377 的八进制数字；也就是说，仅考虑该值的后八位。 将其余数字解释为文本。 例如，如果表达式 `\3000` 中存在捕获组 300，则解释为后向引用 300；如果捕获组 300 不存在，则解释为后跟 0 的八进制数字 300。|通过将尽可能多的数字转换为可引用捕获的十进制值解释为反向引用。 如果任何数字都不能转换，则解释为使用其值不超过八进制值 377 的前导八进制数字的八进制数字；将其余数字解释为文本。|

## <a name="comparison-using-the-invariant-culture"></a>使用固定区域性的比较

默认情况下，当正则表达式引擎执行不区分大小写的比较时，它使用当前区域性的大小写约定来确定等效的大写和小写字符。

但是，此行为不需要某些类型的比较，尤其是在比较用户输入与系统资源名称时（如密码、文件或 URL）。 下面的示例阐释此类方案。 该代码旨在阻止对 URL 开头为 **FILE://** 的所有资源的访问。 正则表达式通过使用正则表达式 `$FILE://` 尝试与字符串的不区分大小写的匹配。 但是，在当前系统区域性为 tr-TR（土耳其语-土耳其）时，“I”不是“i”的大写等效项。 因此，对 <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> 方法的调用返回 `false`，并允许访问该文件。

[!code-csharp[Conceptual.Regex.Language.Options#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/culture1.cs#14)]
[!code-vb[Conceptual.Regex.Language.Options#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/culture1.vb#14)]

> [!NOTE]
> 有关区分大小写和使用固定区域性的字符串比较的更多信息，请参见[针对使用字符串的最佳做法](best-practices-strings.md)。

不使用当前区域性的不区分大小写比较，可以指定 <xref:System.Text.RegularExpressions.RegexOptions.CultureInvariant?displayProperty=nameWithType> 选项忽略语言的区域性差异，并使用固定区域性的约定。

> [!NOTE]
> 仅能通过将 <xref:System.Text.RegularExpressions.RegexOptions.CultureInvariant?displayProperty=nameWithType> 值提供给 `options` 类构造函数或静态模式匹配方法的 <xref:System.Text.RegularExpressions.Regex> 参数来提供使用固定区域性的比较。 它不可作为内联选项使用。

下面的示例与上一示例相等，不同之处是下面的示例使用包含 <xref:System.Text.RegularExpressions.Regex.IsMatch%28System.String%2CSystem.String%2CSystem.Text.RegularExpressions.RegexOptions%29?displayProperty=nameWithType> 的选项调用静态 <xref:System.Text.RegularExpressions.RegexOptions.CultureInvariant?displayProperty=nameWithType> 方法。 即使设置当前区域性到土耳其语（土耳其），正则表达式引擎仍能够成功匹配“FILE”和“file”并能阻止对文件资源的访问。

[!code-csharp[Conceptual.Regex.Language.Options#15](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.regex.language.options/cs/culture1.cs#15)]
[!code-vb[Conceptual.Regex.Language.Options#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.regex.language.options/vb/culture1.vb#15)]

## <a name="see-also"></a>请参阅

- [正则表达式语言 - 快速参考](regular-expression-language-quick-reference.md)
