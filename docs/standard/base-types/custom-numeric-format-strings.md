---
title: 自定义数字格式字符串
description: 了解如何创建自定义数字格式字符串，以便在 .NET 中设置数值数据的格式。 自定义数字格式字符串有一个或多个自定义数值说明符。
ms.date: 06/25/2018
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- numeric format strings [.NET]
- formatting [.NET], numbers
- format strings
- custom numeric format strings
- numbers [.NET], formatting
- format specifiers, numeric
- formatting numbers [.NET]
- format specifiers, custom numeric format strings
ms.assetid: 6f74fd32-6c6b-48ed-8241-3c2b86dea5f4
ms.openlocfilehash: 7665c0980d631069728bcce8178763eb934e054b
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94824384"
---
# <a name="custom-numeric-format-strings"></a>自定义数字格式字符串

你可以创建自定义数字格式字符串，这种字符串由一个或多个自定义数字说明符组成，用于定义设置数值数据格式的方式。 自定义数字格式字符串是任何不属于 [标准数字格式字符串](standard-numeric-format-strings.md)的格式字符串。

所有数字类型的 `ToString` 方法的某些重载支持自定义数字格式字符串。 例如，可将数字格式字符串提供给 <xref:System.Int32.ToString%28System.String%29> 类型的 <xref:System.Int32.ToString%28System.String%2CSystem.IFormatProvider%29> 方法和 <xref:System.Int32> 方法。 .NET [复合格式功能](composite-formatting.md)也支持自定义数字格式字符串，以供 <xref:System.Console> 和 <xref:System.IO.StreamWriter> 类的一些 `Write` 和 `WriteLine` 方法、<xref:System.String.Format%2A?displayProperty=nameWithType> 方法以及 <xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType> 方法使用。 [字符串内插](../../csharp/language-reference/tokens/interpolated.md)功能还支持自定义数字格式字符串。

> [!TIP]
> 你可以下载格式设置实用工具，它属于一种 .NET Core Windows 窗体应用程序，通过该应用程序，你可将格式字符串应用于数值或日期和时间值并显示结果字符串。 源代码适用于 [C#](/samples/dotnet/samples/windowsforms-formatting-utility-cs) 和 [Visual Basic](/samples/dotnet/samples/windowsforms-formatting-utility-vb)。

<a name="table"></a> 下表描述自定义数字格式说明符并显示由每个格式说明符产生的示例输出。 有关使用自定义数字格式字符串的其他信息，请参见 [说明](#NotesCustomFormatting) 一节，有关使用方法的完整演示，请参见 [示例](#example) 一节。

|格式说明符|“属性”|描述|示例|
|----------------------|----------|-----------------|--------------|
|“0”|零占位符|用对应的数字（如果存在）替换零；否则，将在结果字符串中显示零。<br /><br /> 更多信息：[“0”自定义说明符](#Specifier0)。|1234.5678 ("00000") -> 01235<br /><br /> 0.45678 ("0.00", en-US) -> 0.46<br /><br /> 0.45678 ("0.00", fr-FR) -> 0,46|
|"#"|数字占位符|用对应的数字（如果存在）替换“#”符号；否则，不会在结果字符串中显示任何数字。<br /><br /> 请注意，如果输入字符串中的相应数字是无意义的 0，则在结果字符串中不会出现任何数字。 例如，0003 ("####") -> 3。<br /><br /> 更多信息：[“#”自定义说明符](#SpecifierD)。|1234.5678 ("#####") -> 1235<br /><br /> 0.45678 ("#.##", en-US) -> .46<br /><br /> 0.45678 ("#.##", fr-FR) -> ,46|
|"."|小数点|确定小数点分隔符在结果字符串中的位置。<br /><br /> 更多信息：[“.”自定义说明符](#SpecifierPt)。|0.45678 ("0.00", en-US) -> 0.46<br /><br /> 0.45678 ("0.00", fr-FR) -> 0,46|
|","|组分隔符和数字比例换算|用作组分隔符和数字比例换算说明符。 作为组分隔符时，它在各个组之间插入本地化的组分隔符字符。 作为数字比例换算说明符，对于每个指定的逗号，它将数字除以 1000。<br /><br /> 更多信息：[“,”自定义说明符](#SpecifierTh)。|组分隔符说明符：<br /><br /> 2147483647 ("##,#", en-US) -> 2,147,483,647<br /><br /> 2147483647 ("##,#", es-ES) -> 2.147.483.647<br /><br /> 比例换算说明符：<br /><br /> 2147483647 ("#,#,,", en-US) -> 2,147<br /><br /> 2147483647 ("#,#,,", es-ES) -> 2.147|
|"%"|百分比占位符|将数字乘以 100，并在结果字符串中插入本地化的百分比符号。<br /><br /> 更多信息：[“%”自定义说明符](#SpecifierPct)。|0.3697 ("%#0.00", en-US) -> %36.97<br /><br /> 0.3697 ("%#0.00", el-GR) -> %36,97<br /><br /> 0.3697 ("##.0 %", en-US) -> 37.0 %<br /><br /> 0.3697 ("##.0 %", el-GR) -> 37,0 %|
|"‰"|千分比占位符|将数字乘以 1000，并在结果字符串中插入本地化的千分比符号。<br /><br /> 更多信息：[“‰”自定义说明符](#SpecifierPerMille)。|0.03697 ("#0.00‰", en-US) -> 36.97‰<br /><br /> 0.03697 ("#0.00‰", ru-RU) -> 36,97‰|
|“E0”<br /><br /> “E+0”<br /><br /> “E-0”<br /><br /> “E0”<br /><br /> “E+0”<br /><br /> “E-0”|指数表示法|如果后跟至少一个 0（零），则使用指数表示法设置结果格式。 “E”或“e”指示指数符号在结果字符串中是大写还是小写。 跟在“E”或“e”字符后面的零的数目确定指数中的最小位数。 加号 (+) 指示符号字符总是置于指数前面。 减号 (-) 指示符号字符仅置于负指数前面。<br /><br /> 更多信息：[“E”和“e”自定义说明符](#SpecifierExponent)。|987654 ("#0.0e0") -> 98.8e4<br /><br /> 1503.92311 ("0.0##e+00") -> 1.504e+03<br /><br /> 1.8901385E-16 ("0.0e+00") -> 1.9e-16|
|“\\”|转义符|使下一个字符被解释为文本而不是自定义格式说明符。<br /><br /> 更多信息：[“\\”转义字符](#SpecifierEscape)。|987654 ("\\###00\\#") -> #987654#|
|'string'<br /><br /> "*string*"|文本字符串分隔符|指示应复制到未更改的结果字符串的封闭字符。<br/><br/>更多信息：[字符文本](#character-literals)。|68 ("# 'degrees'") -> 68 degrees<br /><br /> 68 ("# ' degrees'") -> 68  degrees|
|;|部分分隔符|通过分隔格式字符串定义正数、负数和零各部分。<br /><br /> 更多信息：[“;”部分分隔符](#SectionSeparator)。|12.345 ("#0.0#;(#0.0#);-\0-") -> 12.35<br /><br /> 0 ("#0.0#;(#0.0#);-\0-") -> -0-<br /><br /> -12.345 ("#0.0#;(#0.0#);-\0-") -> (12.35)<br /><br /> 12.345 ("#0.0#;(#0.0#)") -> 12.35<br /><br /> 0 ("#0.0#;(#0.0#)") -> 0.0<br /><br /> -12.345 ("#0.0#;(#0.0#)") -> (12.35)|
|其他|所有其他字符|字符将复制到未更改的结果字符串。<br/><br/>更多信息：[字符文本](#character-literals)。|68 ("# °") -> 68 °|

以下各节提供有关每个自定义数字格式说明符的详细信息。

[!INCLUDE[C# interactive-note](~/includes/csharp-interactive-partial-note.md)]

<a name="Specifier0"></a>

## <a name="the-0-custom-specifier"></a>“0”自定义说明符

“0”自定义格式说明符用作零占位符符号。 如果要设置格式的值在格式字符串中出现零的位置有一个数字，则将此数字复制到结果字符串中；否则，在结果字符串中显示零。 小数点前最左边的零的位置和小数点后最右边的零的位置确定总在结果字符串中出现的数字范围。

“00”说明符使得值被舍入到小数点前最近的数字，其中零位总被舍去。 例如，用“00”格式化 34.5 将得到值 35。

下面的示例显示几个使用包含零占位符的自定义格式字符串设置格式的值。

[!code-cpp[Formatting.Numeric.Custom#1](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#1)]
[!code-csharp[Formatting.Numeric.Custom#1](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#1)]
[!code-vb[Formatting.Numeric.Custom#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#1)]

[返回表首](#table)

<a name="SpecifierD"></a>

## <a name="the--custom-specifier"></a>“#”自定义说明符

“#”自定义格式说明符用作数字占位符符号。 如果设置了格式的值在格式字符串中显示"#"符号的位置有一个数字，则此数字被复制到结果字符串中。 否则，结果字符串中的此位置不存储任何值。

请注意，如果零不是有效数字，此说明符永不显示零，即使零是字符串中的唯一数字也是如此。 仅当零是所显示的数字中的有效数字时，才会显示零。

“##”格式字符串使得值被舍入到小数点前最近的数字，其中零总被舍去。 例如，用“##”格式化 34.5 将得到值 35。

下面的示例显示几个使用包含数字占位符的自定义格式字符串设置格式的值。

[!code-cpp[Formatting.Numeric.Custom#2](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#2)]
[!code-csharp[Formatting.Numeric.Custom#2](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#2)]
[!code-vb[Formatting.Numeric.Custom#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#2)]

若要返回空缺数字或前导零替换为空格的结果字符串，请使用 [复合格式功能](composite-formatting.md) 并指定字段宽度，如以下示例所示。

[!code-cpp[Formatting.Numeric.Custom#12](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/SpaceOrDigit1.cpp#12)]
[!code-csharp[Formatting.Numeric.Custom#12](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/SpaceOrDigit1.cs#12)]
[!code-vb[Formatting.Numeric.Custom#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/SpaceOrDigit1.vb#12)]

[返回表首](#table)

<a name="SpecifierPt"></a>

## <a name="the--custom-specifier"></a>“.”自定义说明符

"."自定义格式说明符在结果字符串中插入本地化的小数分隔符。 格式字符串中的第一个小数点确定设置了格式的值中的小数分隔符的位置；任何其他小数点会被忽略。

在结果字符串中用作小数分隔符的字符并非总是小数点；它由控制格式设置的 <xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A> 对象的 <xref:System.Globalization.NumberFormatInfo> 属性确定。

下面的示例使用"."格式说明符定义几个结果字符串中的小数点的位置。

[!code-cpp[Formatting.Numeric.Custom#3](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#3)]
[!code-csharp[Formatting.Numeric.Custom#3](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#3)]
[!code-vb[Formatting.Numeric.Custom#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#3)]

[返回表首](#table)

<a name="SpecifierTh"></a>

## <a name="the--custom-specifier"></a>“,”自定义说明符

“,”字符用作组分隔符和数字比例换算说明符。

- 组分隔符：如果在两个设置数字的整数位格式的数字占位符（0 或 #）之间指定一个或多个逗号，则在输出的整数部分中的每个数字组之间插入一个组分隔符字符。

  当前 <xref:System.Globalization.NumberFormatInfo.NumberGroupSeparator%2A> 对象的 <xref:System.Globalization.NumberFormatInfo.NumberGroupSizes%2A> 和 <xref:System.Globalization.NumberFormatInfo> 属性将确定用作数字组分隔符的字符以及每个数字组的大小。 例如，如果使用字符串“#,#”和固定区域性对数字 1000 进行格式化，则输出为“1,000”。

- 数字比例换算符：如果在紧邻显式或隐式小数点的左侧指定一个或多个逗号，则对于每个逗号，将要设置格式的数字除以 1000。 例如，如果使用字符串“0,,”对数字 100000000 进行格式化，则输出为“100”。

可以在同一格式字符串中使用组分隔符和数字比例换算说明符。 例如，如果使用字符串“#,0,,”和固定区域性对数字 1000000000 进行格式化，则输出为“1,000”。

下面的示例演示如何使用逗号作为组分隔符。

[!code-cpp[Formatting.Numeric.Custom#4](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#4)]
[!code-csharp[Formatting.Numeric.Custom#4](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#4)]
[!code-vb[Formatting.Numeric.Custom#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#4)]

下面的示例演示如何使用逗号作为数字比例换算说明符。

[!code-cpp[Formatting.Numeric.Custom#5](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#5)]
[!code-csharp[Formatting.Numeric.Custom#5](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#5)]
[!code-vb[Formatting.Numeric.Custom#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#5)]

[返回表首](#table)

<a name="SpecifierPct"></a>

## <a name="the--custom-specifier"></a>“%”自定义说明符

格式字符串中的百分号 (%) 将使数字在设置格式之前乘以 100。 本地化的百分比符号插入到数字在格式字符串中出现 % 的位置。 使用的百分比字符由当前 <xref:System.Globalization.NumberFormatInfo.PercentSymbol%2A> 对象的 <xref:System.Globalization.NumberFormatInfo> 属性定义。

下面的示例定义几个包含“%”自定义说明符的自定义格式字符串。

[!code-cpp[Formatting.Numeric.Custom#6](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#6)]
[!code-csharp[Formatting.Numeric.Custom#6](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#6)]
[!code-vb[Formatting.Numeric.Custom#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#6)]

[返回表首](#table)

<a name="SpecifierPerMille"></a>

## <a name="the--custom-specifier"></a>“‰”自定义说明符

格式字符串中的千分比字符（‰ 或 \u2030）将使数字在设置格式之前乘以 1000。 在返回的字符串中，相应的千分比符号插在格式字符串中出现 ‰ 符号的位置。 所用的千分比字符由提供特定于区域性的格式设置信息的对象的 <xref:System.Globalization.NumberFormatInfo.PerMilleSymbol%2A?displayProperty=nameWithType> 属性定义。

下面的示例定义一个包含“‰”自定义说明符的自定义格式字符串。

[!code-cpp[Formatting.Numeric.Custom#9](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#9)]
[!code-csharp[Formatting.Numeric.Custom#9](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#9)]
[!code-vb[Formatting.Numeric.Custom#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#9)]

[返回表首](#table)

<a name="SpecifierExponent"></a>

## <a name="the-e-and-e-custom-specifiers"></a>“E”和“e”自定义说明符

如果“E”、“E+”、“E-”、“e”、“e+”或“e-”中的任何一个字符串出现在格式字符串中，而且后面紧跟至少一个零，则数字用科学记数法来设置格式，在数字和指数之间插入“E”或“e”。 跟在科学记数法指示符后面的零的数目确定指数输出的最小位数。 “E+”和“e+”格式指示加号或减号应总是置于指数前面。 “E”、“E-”、“e”或“e-”格式指示符号字符应仅置于负指数前面。

下面的示例使用科学记数法说明符设置几个数值的格式。

[!code-cpp[Formatting.Numeric.Custom#7](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#7)]
[!code-csharp[Formatting.Numeric.Custom#7](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#7)]
[!code-vb[Formatting.Numeric.Custom#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#7)]

[返回表首](#table)

<a name="SpecifierEscape"></a>

## <a name="the--escape-character"></a>“\\”转义字符

格式字符串中的“#”、“0”、“.”、“,”、“%”和“‰”符号被解释为格式说明符而不是文本字符。 大写和小写“E”以及 + 和 - 符号也可能被解释为格式说明符，具体取决于它们在自定义格式字符串中的位置。

若要防止某个字符被解释为格式说明符，你可以在该字符前面加上反斜杠（即转义字符）。 转义字符表示以下字符为应包含在未更改的结果字符串中的字符文本。

若在要结果字符串中包括反斜杠，必须使用另一个反斜杠 (`\\`) 对其转义。

> [!NOTE]
> 一些编译器（如 C++ 和 C# 编译器）也可能会将单个反斜杠字符解释为转义字符。 若要确保在设置格式时正确解释字符串，在 C# 中，可以在字符串之前使用原义字符串文本字符（@ 字符），或者在 C# 和 C++ 中，在每个反斜杠之前另外添加一个反斜杠字符。 下面的 C# 示例阐释了这两种方法。

下面的示例使用转义字符，以防格式设置操作将“#”、“0”和“\\”字符解释为转义字符或格式说明符。 C# 示例使用附加的反斜杠以确保将原反斜杠解释为文本字符。

[!code-cpp[Formatting.Numeric.Custom#11](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/escape1.cpp#11)]
[!code-csharp-interactive[Formatting.Numeric.Custom#11](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/escape1.cs#11)]
[!code-vb[Formatting.Numeric.Custom#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/escape1.vb#11)]

[返回表首](#table)

<a name="SectionSeparator"></a>

## <a name="the--section-separator"></a>“;”部分分隔符

分号 (;) 是条件格式说明符，它可以对数字应用不同的格式设置，具体取决于值为正、为负还是为零。 为产生这种行为，自定义格式字符串可以包含最多三个用分号分隔的部分。 下表描述了这些部分。

|部分数目|描述|
|------------------------|-----------------|
|一个部分|格式字符串应用于所有值。|
|两个部分|第一部分应用于正值和零，第二部分应用于负值。<br /><br /> 如果要设置格式的数字为负，但根据第二部分中的格式舍入后为零，则最终的零根据第一部分进行格式设置。|
|三个部分|第一部分应用于正值，第二部分应用于负值，第三部分应用于零。<br /><br /> 第二部分可以留空（分号间没有任何内容），在这种情况下，第一部分应用于所有非零值。<br /><br /> 如果要设置格式的数字为非零值，但根据第一部分或第二部分中的格式舍入后为零，则最终的零根据第三部分进行格式设置。|

格式化最终值时，部分分隔符忽略所有先前存在的与数字关联的格式设置。 例如，使用部分分隔符时，显示的负值永远不带负号。 如果你希望格式化后的最终值带有负号，则应明确包含负号，让它作为自定义格式说明符的组成部分。

下面的示例使用“;”格式说明符来分别设置正数、负数和零的格式。

[!code-cpp[Formatting.Numeric.Custom#8](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/custom.cpp#8)]
[!code-csharp-interactive[Formatting.Numeric.Custom#8](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/custom.cs#8)]
[!code-vb[Formatting.Numeric.Custom#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/Custom.vb#8)]

[返回表首](#table)

## <a name="character-literals"></a>字符文本

出现在自定义数值格式字符串中的格式说明符始终解释为格式字符而不是文本字符。 这包括以下字符：

- [0](#Specifier0)
- [\#](#SpecifierD)
- [%](#SpecifierPct)
- [‰](#SpecifierPerMille)
- '
- [\\](#SpecifierEscape)
- [.](#SpecifierPt)
- [，](#SpecifierTh)
- [E 或 e](#SpecifierExponent)，取决于它在格式字符串中的位置。

所有其他字符始终解释为字符文本，在格式设置操作中，将按原样包含在结果字符串中。  在分析操作中，这些字符必须与输入字符串中的字符完全匹配；比较时区分大小写。

以下示例演示了文本字符单位（这里是“千”）的一种常见用法：

[!code-csharp-interactive[literal characters](~/samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/literal2.cs#1)]
[!code-vb[literal characters](~/samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/literal2.vb#1)]

可通过两种方法来指示要将字符解释为文本字符而不是格式字符，以便这些字符可以包含在结果字符串中，或者在输入字符串中成功完成分析：

- 通过对格式字符进行转义处理。 有关详细信息，请参阅[“\\”转义字符](#SpecifierEscape)。

- 通过将整个文本字符串括在单引号中。

以下示例使用这两种方法将保留字符包含在自定义数值格式字符串中。

[!code-csharp-interactive[including reserved characters](~/samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/literal1.cs#1)]
[!code-vb[including reserved characters](~/samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/literal1.vb#1)]

<a name="NotesCustomFormatting"></a>

## <a name="notes"></a>说明

### <a name="floating-point-infinities-and-nan"></a>浮点型无穷大和 NaN

无论格式字符串原来是什么值，只要 <xref:System.Single> 或 <xref:System.Double> 浮点类型的值为正无穷大、负无穷大或非数值 (NaN)，格式字符串就分别是当前适用的 <xref:System.Globalization.NumberFormatInfo.PositiveInfinitySymbol%2A>对象指定的 <xref:System.Globalization.NumberFormatInfo.NegativeInfinitySymbol%2A>、 <xref:System.Globalization.NumberFormatInfo.NaNSymbol%2A> 或 <xref:System.Globalization.NumberFormatInfo> 属性的值。

### <a name="control-panel-settings"></a>控制面板设置

控制面板中 **“区域和语言选项”** 项中的设置会影响由格式化操作产生的结果字符串。 这些设置用于初始化与当前线程区域性关联的 <xref:System.Globalization.NumberFormatInfo> 对象，并且当前线程区域性将提供用于控制格式设置的值。 使用不同设置的计算机将生成不同的结果字符串。

此外，如果使用 <xref:System.Globalization.CultureInfo.%23ctor%28System.String%29> 构造函数实例化一个新的 <xref:System.Globalization.CultureInfo> 对象以表示与当前的系统区域性相同的区域性，则通过控制面板中的 **“区域和语言选项”** 建立的任何自定义都将应用到新的 <xref:System.Globalization.CultureInfo> 对象。 可以使用 <xref:System.Globalization.CultureInfo.%23ctor%28System.String%2CSystem.Boolean%29> 构造函数来创建不会反映系统的自定义项的 <xref:System.Globalization.CultureInfo> 对象。

### <a name="rounding-and-fixed-point-format-strings"></a>舍入和定点格式字符串

对于固定点格式字符串（即不包含科学记数法格式字符的格式字符串），数字被舍入为与小数点右边的数字占位符数目相同的小数位数。 如果格式字符串不包含小数点，数字被舍入为最接近的整数。 如果数字位数多于小数点左边数字占位符的个数，多余的数字被复制到结果字符串中紧挨着第一个数字占位符的前面。

[返回表首](#table)

<a name="example"></a>

## <a name="example"></a>示例

下面的示例演示两个自定义数字格式字符串。 在这两个示例中，数字占位符 (`#`) 显示数值数据，且所有其他字符被复制到结果字符串。

[!code-cpp[Formatting.Numeric.Custom#10](../../../samples/snippets/cpp/VS_Snippets_CLR/formatting.numeric.custom/cpp/example1.cpp#10)]
[!code-csharp-interactive[Formatting.Numeric.Custom#10](../../../samples/snippets/csharp/VS_Snippets_CLR/formatting.numeric.custom/cs/example1.cs#10)]
[!code-vb[Formatting.Numeric.Custom#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/formatting.numeric.custom/vb/example1.vb#10)]

[返回表首](#table)

## <a name="see-also"></a>请参阅

- <xref:System.Globalization.NumberFormatInfo?displayProperty=nameWithType>
- [格式设置类型](formatting-types.md)
- [标准数字格式字符串](standard-numeric-format-strings.md)
- [如何：用前导零填充数字](how-to-pad-a-number-with-leading-zeros.md)
- [示例：.NET Core WinForms 格式设置实用工具 (C#)](/samples/dotnet/samples/windowsforms-formatting-utility-cs)
- [示例：.NET Core WinForms 格式设置实用工具 (Visual Basic)](/samples/dotnet/samples/windowsforms-formatting-utility-vb)
