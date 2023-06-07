---
title: 标准数字格式字符串
description: 本文介绍如何在 .NET 中使用标准数字格式字符串将常用数值类型格式化为文本表示形式。
ms.date: 06/10/2018
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- numeric format strings [.NET]
- formatting [.NET], numbers
- standard format strings, numeric
- format strings
- numbers [.NET], formatting
- format specifiers, numeric
- standard numeric format strings
- formatting numbers [.NET]
- format specifiers, standard numeric format strings
ms.openlocfilehash: b8537e8dcfdd45ed8da18b8b82aae5e9fc0cd96e
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94818760"
---
# <a name="standard-numeric-format-strings"></a>标准数字格式字符串

标准数字格式字符串用于格式化通用数值类型。 标准数字格式字符串采用 `Axx` 的形式，其中：

- `A` 是称为“格式说明符”的单个字母字符。 任何包含一个以上字母字符（包括空白）的数字格式字符串都被解释为自定义数字格式字符串。 有关更多信息，请参见[自定义数字格式字符串](custom-numeric-format-strings.md)。

- `xx` 是称为“精度说明符”的可选整数。 精度说明符的范围从 0 到 99，并且影响结果中的位数。 请注意，精度说明符控制数字的字符串表示形式中的数字个数。 它不舍入该数字。 若要执行舍入运算，请使用 <xref:System.Math.Ceiling%2A?displayProperty=nameWithType>、<xref:System.Math.Floor%2A?displayProperty=nameWithType> 或 <xref:System.Math.Round%2A?displayProperty=nameWithType> 方法。

  当精度说明符控制结果字符串中的小数位数时，结果字符串会反映一个数字，该数字四舍五入到最接近无限精确结果的可表示结果。 如果有两个同样接近的可表示结果：
  - 在 .NET Framework 和.NET Core（.NET Core 2.0 及以下）上，运行时选择最低有效数字更高的结果（即使用 <xref:System.MidpointRounding.AwayFromZero?displayProperty=nameWithType>）。
  - 在 .NET Core 2.1 及更高版本上，运行时选择最低有效数字为偶数的结果（即使用 <xref:System.MidpointRounding.ToEven?displayProperty=nameWithType>）。

  > [!NOTE]
  > 精度说明符确定结果字符串中的位数。 若要使用前导或尾随空格填充结果字符串，请使用 [复合格式设置](composite-formatting.md)功能，并在格式项中定义 *对齐组件*。

下列支持标准数字格式字符串：

- 所有数字类型的一些 `ToString` 方法重载。 例如，可以向 <xref:System.Int32.ToString%28System.String%29?displayProperty=nameWithType> 和 <xref:System.Int32.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> 方法提供数字格式字符串。

- .NET [复合格式功能](composite-formatting.md)，由 <xref:System.Console> 和 <xref:System.IO.StreamWriter> 类的一些 `Write` 和 `WriteLine` 方法、<xref:System.String.Format%2A?displayProperty=nameWithType> 方法以及 <xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType> 方法使用。 复合格式功能允许你将多个数据项的字符串表示形式包含在单个字符串中，以指定字段宽度，并在字段中对齐数字。 有关更多信息，请参见[复合格式设置](composite-formatting.md)。

- C# 和 Visual Basic 中的[内插的字符串](../../csharp/language-reference/tokens/interpolated.md)，与复合格式字符串相比，语法更简化。

> [!TIP]
> 你可以下载格式设置实用工具，它属于一种 .NET Core Windows 窗体应用程序，通过该应用程序，你可将格式字符串应用于数值或日期和时间值并显示结果字符串。 源代码适用于 [C#](/samples/dotnet/samples/windowsforms-formatting-utility-cs) 和 [Visual Basic](/samples/dotnet/samples/windowsforms-formatting-utility-vb)。

<a name="table"></a>下表介绍标准数字格式说明符并显示每个格式说明符产生的示例输出。 有关使用标准数字格式字符串的其他信息，请参见[注释](#NotesStandardFormatting)一节；有关使用方法的完整演示，请参见[示例](#example)一节。

|格式说明符|“属性”|描述|示例|
|----------------------|----------|-----------------|--------------|
|“C”或“c”|货币|结果:货币值。<br /><br /> 受以下类型支持：所有数值类型。<br /><br /> 精度说明符：十进制小数位数。<br /><br /> 默认值精度说明符：由 <xref:System.Globalization.NumberFormatInfo.CurrencyDecimalDigits%2A?displayProperty=nameWithType> 定义。<br /><br /> 更多信息：[货币（“C”）格式说明符](#CFormatString)。|123.456 ("C", en-US) -> \\$123.46<br /><br /> 123.456 ("C", fr-FR) -> 123,46 &euro;<br /><br /> 123.456 ("C", ja-JP) -> ¥123<br /><br /> -123.456 ("C3", en-US) -> (\\$123.456)<br /><br /> -123.456 ("C3", fr-FR) -> -123,456 &euro;<br /><br /> -123.456 ("C3", ja-JP) -> -¥123.456|
|“D”或“d”|十进制|结果:整型数字，负号可选。<br /><br /> 受以下类型支持：仅限整型类型。<br /><br /> 精度说明符：数字位数下限。<br /><br /> 默认值精度说明符：所需数字位数下限。<br /><br /> 更多信息：[十进制（“D”）格式说明符](#DFormatString)。|1234 ("D") -> 1234<br /><br /> -1234 ("D6") -> -001234|
|“E”或“e”|指数（科学型）|结果:指数表示法。<br /><br /> 受以下类型支持：所有数值类型。<br /><br /> 精度说明符：十进制小数位数。<br /><br /> 默认值精度说明符：6.<br /><br /> 更多信息：[指数（“E”）格式说明符](#EFormatString)。|1052.0329112756 ("E", en-US) -> 1.052033E+003<br /><br /> 1052.0329112756 ("e", fr-FR) -> 1,052033e+003<br /><br /> -1052.0329112756 ("e2", en-US) -> -1.05e+003<br /><br /> -1052.0329112756 ("E2", fr-FR) -> -1,05E+003|
|“F”或“f”|定点|结果:整数和十进制小数，负号可选。<br /><br /> 受以下类型支持：所有数值类型。<br /><br /> 精度说明符：十进制小数位数。<br /><br /> 默认值精度说明符：由 <xref:System.Globalization.NumberFormatInfo.NumberDecimalDigits%2A?displayProperty=nameWithType> 定义。<br /><br /> 更多信息：[定点（“F”）格式说明符](#FFormatString)。|1234.567 ("F", en-US) -> 1234.57<br /><br /> 1234.567 ("F", de-DE) -> 1234,57<br /><br /> 1234 ("F1", en-US) -> 1234.0<br /><br /> 1234 ("F1", de-DE) -> 1234,0<br /><br /> -1234.56 ("F4", en-US) -> -1234.5600<br /><br /> -1234.56 ("F4", de-DE) -> -1234,5600|
|“G”或“g”|常规|结果:更紧凑的定点表示法或科学记数法。<br /><br /> 受以下类型支持：所有数值类型。<br /><br /> 精度说明符：有效位数。<br /><br /> 默认值精度说明符：具体取决于数值类型。<br /><br /> 更多信息：[常规（“G”）格式说明符](#GFormatString)。|-123.456 ("G", en-US) -> -123.456<br /><br /> -123.456 ("G", sv-SE) -> -123,456<br /><br /> 123.4546 ("G4", en-US) -> 123.5<br /><br /> 123.4546 ("G4", sv-SE) -> 123,5<br /><br /> -1.234567890e-25 ("G", en-US) -> -1.23456789E-25<br /><br /> -1.234567890e-25 ("G", sv-SE) -> -1,23456789E-25|
|“N”或“n”|数字|结果:整数和十进制小数、组分隔符和十进制小数分隔符，负号可选。<br /><br /> 受以下类型支持：所有数值类型。<br /><br /> 精度说明符：所需的小数位数。<br /><br /> 默认值精度说明符：由 <xref:System.Globalization.NumberFormatInfo.NumberDecimalDigits%2A?displayProperty=nameWithType> 定义。<br /><br /> 更多信息：[数字（“N”）格式说明符](#NFormatString)。|1234.567 ("N", en-US) -> 1,234.57<br /><br /> 1234.567 ("N", ru-RU) -> 1 234,57<br /><br /> 1234 ("N1", en-US) -> 1,234.0<br /><br /> 1234 ("N1", ru-RU) -> 1 234,0<br /><br /> -1234.56 ("N3", en-US) -> -1,234.560<br /><br /> -1234.56 ("N3", ru-RU) -> -1 234,560|
|“P”或“p”|百分比|结果:数字乘以 100 并以百分比符号显示。<br /><br /> 受以下类型支持：所有数值类型。<br /><br /> 精度说明符：所需的小数位数。<br /><br /> 默认值精度说明符：由 <xref:System.Globalization.NumberFormatInfo.PercentDecimalDigits%2A?displayProperty=nameWithType> 定义。<br /><br /> 更多信息：[百分比（“P”）格式说明符](#PFormatString)。|1 ("P", en-US) -> 100.00 %<br /><br /> 1 ("P", fr-FR) -> 100,00 %<br /><br /> -0.39678 ("P1", en-US) -> -39.7 %<br /><br /> -0.39678 ("P1", fr-FR) -> -39,7 %|
|“R”或“r”|往返过程|结果:可以往返至相同数字的字符串。<br /><br /> 受以下类型支持：<xref:System.Single>、<xref:System.Double> 和 <xref:System.Numerics.BigInteger>。<br /><br /> 注意：建议只用于 <xref:System.Numerics.BigInteger> 类型。 对于 <xref:System.Double> 类型，请使用 "G17"；对于 <xref:System.Single> 类型，请使用 "G9"。 <br> 精度说明符：已忽略。<br /><br /> 更多信息：[往返过程（“R”）格式说明符](#RFormatString)。|123456789.12345678 ("R") -> 123456789.12345678<br /><br /> -1234567890.12345678 ("R") -> -1234567890.1234567|
|“X”或“x”|十六进制|结果:十六进制字符串。<br /><br /> 受以下类型支持：仅限整型类型。<br /><br /> 精度说明符：结果字符串中的位数。<br /><br /> 更多信息：[十六进制（“X”）格式说明符](#XFormatString)。|255 ("X") -> FF<br /><br /> -1 ("x") -> ff<br /><br /> 255 ("x4") -> 00ff<br /><br /> -1 ("X4") -> 00FF|
|任何其他单个字符|未知说明符|结果:在运行时引发 <xref:System.FormatException>。||

<a name="Using"></a>

## <a name="using-standard-numeric-format-strings"></a>使用标准数字格式字符串

[!INCLUDE[interactive-note](~/includes/csharp-interactive-note.md)]

可采用以下两种方式之一使用标准数字格式字符串定义数值的格式：

- 可以传递给拥有 `ToString` 参数的 `format` 方法的重载。 下面的示例将数值的格式设置为当前区域性（在此示例中，为 en-US 区域性）中的货币字符串。

  [!code-cpp[Formatting.Numeric.Standard#10](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/standardusage1.cpp#10)]
  [!code-csharp-interactive[Formatting.Numeric.Standard#10](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/standardusage1.cs#10)]
  [!code-vb[Formatting.Numeric.Standard#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/standardusage1.vb#10)]

- 它可作为与 <xref:System.String.Format%2A?displayProperty=nameWithType>、<xref:System.Console.WriteLine%2A?displayProperty=nameWithType> 和 <xref:System.Text.StringBuilder.AppendFormat%2A?displayProperty=nameWithType> 等方法一起使用的格式项中的 `formatString` 参数提供。 有关更多信息，请参见[复合格式设置](composite-formatting.md)。 下面的示例使用格式项在字符串中插入货币值。

  [!code-cpp[Formatting.Numeric.Standard#11](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/standardusage1.cpp#11)]
  [!code-csharp-interactive[Formatting.Numeric.Standard#11](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/standardusage1.cs#11)]
  [!code-vb[Formatting.Numeric.Standard#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/standardusage1.vb#11)]

  可以视需要选择提供 `alignment` 参数，以指定数字字段宽度以及值是右对齐还是左对齐。 下面的示例在 28 位字符的字段中左对齐货币值，在 14 位字符的字段中右对齐货币值。

  [!code-cpp[Formatting.Numeric.Standard#12](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/standardusage1.cpp#12)]
  [!code-csharp-interactive[Formatting.Numeric.Standard#12](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/standardusage1.cs#12)]
  [!code-vb[Formatting.Numeric.Standard#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/standardusage1.vb#12)]

- 可以提供它作为内插字符串的内插表达式项中的 `formatString` 参数。 有关详细信息，请参阅 C# 参考中的[字符串内插](../../csharp/language-reference/tokens/interpolated.md)主题，或 Visual Basic 参考中的[内插字符串](../../visual-basic/programming-guide/language-features/strings/interpolated-strings.md)主题。

以下各节提供有关每个标准数字格式字符串的详细信息。

<a name="CFormatString"></a>

## <a name="the-currency-c-format-specifier"></a>货币（“C”）格式说明符

“C”（或货币）格式说明符将数字转换为表示货币金额的字符串。 精度说明符指示结果字符串中的所需小数位数。 如果省略精度说明符，则默认精度由 <xref:System.Globalization.NumberFormatInfo.CurrencyDecimalDigits%2A?displayProperty=nameWithType> 属性定义。

如果要设置格式的值的小数位数多于指定或默认数量，将在结果字符串中舍入小数值。 如果指定的小数位数右侧的值大于或等于 5，则结果字符串中的最后一位数将向远离零的一侧舍入。

结果字符串受当前 <xref:System.Globalization.NumberFormatInfo> 对象的格式信息的影响。 下表列出了 <xref:System.Globalization.NumberFormatInfo> 属性，这些属性控制返回字符串的格式。

|NumberFormatInfo 属性|描述|
|-------------------------------|-----------------|
|<xref:System.Globalization.NumberFormatInfo.CurrencyPositivePattern%2A>|定义正值的货币符号的位置。|
|<xref:System.Globalization.NumberFormatInfo.CurrencyNegativePattern%2A>|定义负值的货币符号的位置，并指定负号由括号表示还是由 <xref:System.Globalization.NumberFormatInfo.NegativeSign%2A> 属性表示。|
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|定义在 <xref:System.Globalization.NumberFormatInfo.CurrencyNegativePattern%2A> 指明不使用括号时使用的负号。|
|<xref:System.Globalization.NumberFormatInfo.CurrencySymbol%2A>|定义货币符号。|
|<xref:System.Globalization.NumberFormatInfo.CurrencyDecimalDigits%2A>|定义货币值中的默认小数位数。 可使用精度说明符重写此值。|
|<xref:System.Globalization.NumberFormatInfo.CurrencyDecimalSeparator%2A>|定义分隔整数位和小数位的字符串。|
|<xref:System.Globalization.NumberFormatInfo.CurrencyGroupSeparator%2A>|定义分隔整数的组的字符串。|
|<xref:System.Globalization.NumberFormatInfo.CurrencyGroupSizes%2A>|指定在组中显示的整数位数。|

下面的示例使用货币格式说明符设置 <xref:System.Double> 值的格式：

[!code-cpp[Formatting.Numeric.Standard#1](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#1)]
[!code-csharp[Formatting.Numeric.Standard#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#1)]
[!code-vb[Formatting.Numeric.Standard#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#1)]

[返回表首](#table)

<a name="DFormatString"></a>

## <a name="the-decimal-d-format-specifier"></a>十进制（“D”）格式说明符

“D”（或十进制）格式说明符将数字转换为十进制数字 (0-9) 的字符串，如果数字为负，则前面加负号。 只有整型才支持此格式。

精度说明符指示结果字符串中所需的最少数字个数。 如果需要的话，则用零填充该数字的左侧，以产生精度说明符给定的数字个数。 如果未指定精度说明符，则默认值为表示不带前导零的整数所需的最小值。

结果字符串受当前 <xref:System.Globalization.NumberFormatInfo> 对象的格式信息的影响。 如下表所示，一个属性会影响结果字符串的格式。

|NumberFormatInfo 属性|描述|
|-------------------------------|-----------------|
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|定义指示数字为负值的字符串。|

下面的示例使用十进制格式说明符设置 <xref:System.Int32> 值的格式。

[!code-cpp[Formatting.Numeric.Standard#2](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#2)]
[!code-csharp-interactive[Formatting.Numeric.Standard#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#2)]
[!code-vb[Formatting.Numeric.Standard#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#2)]

[返回表首](#table)

<a name="EFormatString"></a>

## <a name="the-exponential-e-format-specifier"></a>指数（“E”）格式说明符

指数（“E”）格式说明符将数字转换为“-d.ddd…E+ddd”或“-d.ddd…e+ddd”形式的字符串，其中每个“d”表示一个数字 (0-9)。 如果该数字为负，则该字符串以减号开头。 小数点前总是恰好有一个数字。

精度说明符指示小数点后所需的位数。 如果省略精度说明符，则使用默认值，即小数点后六位数字。

格式说明符的大小写指示为指数加前缀“E”还是“e”。 指数总是由正号或负号以及最少三位数字组成。 如果需要，用零填充指数以满足最少三位数字的要求。

结果字符串受当前 <xref:System.Globalization.NumberFormatInfo> 对象的格式信息的影响。 下表列出了 <xref:System.Globalization.NumberFormatInfo> 属性，这些属性控制返回字符串的格式。

|NumberFormatInfo 属性|描述|
|-------------------------------|-----------------|
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|定义表示数字对于系数和指数都为负值的字符串。|
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A>|定义在系数中将整数位与小数位分隔的字符串。|
|<xref:System.Globalization.NumberFormatInfo.PositiveSign%2A>|定义指示指数为正值的字符串。|

下面的示例使用指数格式说明符设置 <xref:System.Double> 值的格式：

[!code-cpp[Formatting.Numeric.Standard#3](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#3)]
[!code-csharp[Formatting.Numeric.Standard#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#3)]
[!code-vb[Formatting.Numeric.Standard#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#3)]

[返回表首](#table)

<a name="FFormatString"></a>

## <a name="the-fixed-point-f-format-specifier"></a>定点（“F”）格式说明符

定点（“F”）格式说明符将数字转换为“-ddd.ddd…”形式的字符串，其中每个“d”表示一个数字 (0-9)。 如果该数字为负，则该字符串以减号开头。

精度说明符指示所需的小数位数。 如果省略精度说明符，则当前 <xref:System.Globalization.NumberFormatInfo.NumberDecimalDigits%2A?displayProperty=nameWithType> 属性提供数值精度。

结果字符串受当前 <xref:System.Globalization.NumberFormatInfo> 对象的格式信息的影响。 下表列出了 <xref:System.Globalization.NumberFormatInfo> 对象的属性，这些属性控制结果字符串的格式。

|NumberFormatInfo 属性|描述|
|-------------------------------|-----------------|
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|定义指示数字为负值的字符串。|
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A>|定义将整数位与小数位分隔的字符串。|
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalDigits%2A>|定义默认小数位数。 可使用精度说明符重写此值。|

下面的示例使用定点格式说明符设置 <xref:System.Double> 和 <xref:System.Int32> 值的格式：

[!code-cpp[Formatting.Numeric.Standard#4](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#4)]
[!code-csharp[Formatting.Numeric.Standard#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#4)]
[!code-vb[Formatting.Numeric.Standard#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#4)]

[返回表首](#table)

<a name="GFormatString"></a>

## <a name="the-general-g-format-specifier"></a>常规（“G”）格式说明符

根据数字类型以及是否存在精度说明符，常规（“G”）格式说明符将数字转换为更紧凑的定点表示法或科学记数法。 精度说明符定义可以出现在结果字符串中的最大有效位数。 如果精度说明符被省略或为零，则数字的类型决定默认精度，如下表所示。

|数值类型|默认值精度|
|------------------|-----------------------|
|<xref:System.Byte> 或 <xref:System.SByte>|3 位|
|<xref:System.Int16> 或 <xref:System.UInt16>|5 位|
|<xref:System.Int32> 或 <xref:System.UInt32>|10 位|
|<xref:System.Int64>|19 位|
|<xref:System.UInt64>|20 位|
|<xref:System.Numerics.BigInteger>|无限制（与[“R”](#RFormatString)相同）|
|<xref:System.Single>|7 位|
|<xref:System.Double>|15 位|
|<xref:System.Decimal>|29 位|

如果用科学记数法表示数字时指数大于 -5 而且小于精度说明符，则使用定点表示法；否则使用科学记数法。 结果包含小数点（如果需要），并且忽略小数点后面的尾部零。 如果精度说明符存在，并且结果的有效位数超过指定精度，则通过舍入移除多余的尾部数字。

但是，如果数字是 <xref:System.Decimal> 并且省略精度说明符，将总是使用定点表示法并保留尾部零。

使用科学记数法时，如果格式说明符是“G”，则结果的指数带前缀“E”；如果格式说明符是“g”，则结果的指数带前缀“e”。 指数最少包含两个数字。 这与由指数格式说明符生成的科学记数法的格式不同，后者在指数中最少包括三个数字。

请注意，如果与 <xref:System.Double> 值结合使用，"G17" 格式说明符可确保原始 <xref:System.Double> 值成功往返。 这是因为 <xref:System.Double> 是符合 IEEE 754-2008 要求的双精度 (`binary64`) 浮点数，最高可提供 17 位有效数字的精度。 建议使用此说明符，而不是 ["R" 格式说明符](#RFormatString)，因为在某些情况下，"R" 无法成功往返双精度浮点值。 下面的示例阐释了这样一种情况。

[!code-csharp-interactive[Round-tripping a Double](../../../samples/snippets/standard/base-types/format-strings/csharp/g17.cs#GeneralFormatSpecifier)]
[!code-vb[Round-tripping a Double](../../../samples/snippets/standard/base-types/format-strings/vb/g17.vb)]

如果与 <xref:System.Single> 值结合使用，"G9" 格式说明符可确保原始 <xref:System.Single> 值成功往返。 这是因为 <xref:System.Single> 是符合 IEEE 754-2008 要求的但精度 (`binary32`) 浮点数，最高可提供 9 位有效数字的精度。 出于性能原因，我们建议使用它而不是[“R”格式说明符](#RFormatString)。

结果字符串受当前 <xref:System.Globalization.NumberFormatInfo> 对象的格式信息的影响。 下表列出了 <xref:System.Globalization.NumberFormatInfo> 属性，这些属性控制结果字符串的格式。

|NumberFormatInfo 属性|描述|
|-------------------------------|-----------------|
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|定义指示数字为负值的字符串。|
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A>|定义将整数位与小数位分隔的字符串。|
|<xref:System.Globalization.NumberFormatInfo.PositiveSign%2A>|定义指示指数为正值的字符串。|

下面的示例使用常规格式说明符设置各种浮点值的格式：

[!code-cpp[Formatting.Numeric.Standard#5](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#5)]
[!code-csharp[Formatting.Numeric.Standard#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#5)]
[!code-vb[Formatting.Numeric.Standard#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#5)]

[返回表首](#table)

<a name="NFormatString"></a>

## <a name="the-numeric-n-format-specifier"></a>数字（“N”）格式说明符

数字（"N"）格式说明符将数字转换为"-d,ddd,ddd.ddd…"形式的字符串，其中"-"表示负数符号（如果需要），"d"表示数字 (0-9)，","表示组分隔符，"."表示小数点符号。 精度说明符指示小数点后所需的位数。 如果省略精度限定符，则小数位数由当前的 <xref:System.Globalization.NumberFormatInfo.NumberDecimalDigits%2A?displayProperty=nameWithType> 属性来定义。

结果字符串受当前 <xref:System.Globalization.NumberFormatInfo> 对象的格式信息的影响。 下表列出了 <xref:System.Globalization.NumberFormatInfo> 属性，这些属性控制结果字符串的格式。

|NumberFormatInfo 属性|描述|
|-------------------------------|-----------------|
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|定义指示数字为负值的字符串。|
|<xref:System.Globalization.NumberFormatInfo.NumberNegativePattern%2A>|定义负值的格式，并指定负号由括号表示还是由 <xref:System.Globalization.NumberFormatInfo.NegativeSign%2A> 属性表示。|
|<xref:System.Globalization.NumberFormatInfo.NumberGroupSizes%2A>|指定在组分隔符之间显示的整数位数。|
|<xref:System.Globalization.NumberFormatInfo.NumberGroupSeparator%2A>|定义分隔整数的组的字符串。|
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A>|定义分隔整数位和小数位的字符串。|
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalDigits%2A>|定义默认小数位数。 可使用精度说明符重写此值。|

下面的示例使用数字格式说明符设置各种浮点值的格式：

[!code-cpp[Formatting.Numeric.Standard#6](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#6)]
[!code-csharp[Formatting.Numeric.Standard#6](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#6)]
[!code-vb[Formatting.Numeric.Standard#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#6)]

[返回表首](#table)

<a name="PFormatString"></a>

## <a name="the-percent-p-format-specifier"></a>百分比（“P”）格式说明符

百分比（“P”）格式说明符将数字乘以 100 并将其转换为表示百分比的字符串。 精度说明符指示所需的小数位数。 如果省略精度说明符，则使用当前 <xref:System.Globalization.NumberFormatInfo.PercentDecimalDigits%2A> 属性提供的默认数值精度。

下表列出了 <xref:System.Globalization.NumberFormatInfo> 属性，这些属性控制返回字符串的格式。

|NumberFormatInfo 属性|描述|
|-------------------------------|-----------------|
|<xref:System.Globalization.NumberFormatInfo.PercentPositivePattern%2A>|定义正值的百分比符号的位置。|
|<xref:System.Globalization.NumberFormatInfo.PercentNegativePattern%2A>|定义负值的百分比符号和负号的位置。|
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|定义指示数字为负值的字符串。|
|<xref:System.Globalization.NumberFormatInfo.PercentSymbol%2A>|定义百分比符号。|
|<xref:System.Globalization.NumberFormatInfo.PercentDecimalDigits%2A>|定义百分比值中的默认小数位数。 可使用精度说明符重写此值。|
|<xref:System.Globalization.NumberFormatInfo.PercentDecimalSeparator%2A>|定义分隔整数位和小数位的字符串。|
|<xref:System.Globalization.NumberFormatInfo.PercentGroupSeparator%2A>|定义分隔整数的组的字符串。|
|<xref:System.Globalization.NumberFormatInfo.PercentGroupSizes%2A>|指定在组中显示的整数位数。|

下面的示例使用百分比格式说明符设置浮点值的格式：

[!code-cpp[Formatting.Numeric.Standard#7](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#7)]
[!code-csharp[Formatting.Numeric.Standard#7](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#7)]
[!code-vb[Formatting.Numeric.Standard#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#7)]

[返回表首](#table)

<a name="RFormatString"></a>

## <a name="the-round-trip-r-format-specifier"></a>往返过程（“R”）格式说明符

往返 ("R") 格式说明符试图确保将转换为字符串的数值分析回相同的数值。 只有 <xref:System.Single>、<xref:System.Double> 和 <xref:System.Numerics.BigInteger> 类型支持此格式。

对于 <xref:System.Double> 值，某些情况下的“R”格式说明符未能成功往返原始值。 对于两个 <xref:System.Double> 和 <xref:System.Single> 值，它还提供相对较差的性能。 建议改用 ["G17"](#GFormatString) 格式说明符以成功往返 <xref:System.Double> 值，并改用 ["G9"](#GFormatString) 格式说明符以成功往返 <xref:System.Single> 值。

如果使用此说明符设置 <xref:System.Numerics.BigInteger> 值的格式，其字符串表示形式将包含 <xref:System.Numerics.BigInteger> 值中的所有有效位。

尽管可以包括精度说明符，但会忽略它。 使用此说明符时，往返过程优先于精度。
结果字符串受当前 <xref:System.Globalization.NumberFormatInfo> 对象的格式信息的影响。 下表列出了 <xref:System.Globalization.NumberFormatInfo> 属性，这些属性控制结果字符串的格式。

|NumberFormatInfo 属性|描述|
|-------------------------------|-----------------|
|<xref:System.Globalization.NumberFormatInfo.NegativeSign%2A>|定义指示数字为负值的字符串。|
|<xref:System.Globalization.NumberFormatInfo.NumberDecimalSeparator%2A>|定义将整数位与小数位分隔的字符串。|
|<xref:System.Globalization.NumberFormatInfo.PositiveSign%2A>|定义指示指数为正值的字符串。|

下面的示例使用往返格式说明符设置 <xref:System.Numerics.BigInteger> 值的格式。

[!code-cpp[R format specifier with a BigInteger](../../../samples/snippets/standard/base-types/format-strings/biginteger-r.cpp)]
[!code-csharp[R format specifier with a BigInteger](../../../samples/snippets/standard/base-types/format-strings/biginteger-r.cs)]
[!code-vb[R format specifier with a BigInteger](../../../samples/snippets/standard/base-types/format-strings/biginteger-r.vb)]

> [!IMPORTANT]
> 在某些情况下，如果使用“R”标准数字格式字符串格式化的 <xref:System.Double> 值使用 `/platform:x64` 或 `/platform:anycpu` 开关编译并在 64 位系统上运行，则该值将无法成功往返。 有关详细信息，请参阅以下段落。

若要解决使用“R”标准数字格式字符串格式化的 <xref:System.Double> 值在使用 `/platform:x64` 或 `/platform:anycpu` 交换机进行编译并在 64 位系统上运行时所出现的往返不成功的问题，可以使用“G17”标准数字格式字符格式化 <xref:System.Double> 值。 以下示例将“R”格式字符串与无法成功往返的 <xref:System.Double> 值配合使用，并使用“G17”格式字符串以成功往返原始值：

[!code-csharp[System.Double.ToString#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Double.ToString/cs/roundtripex1.cs#RoundTrip)]
[!code-vb[System.Double.ToString#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Double.ToString/vb/roundtripex1.vb#5)]

[返回表首](#table)

<a name="XFormatString"></a>

## <a name="the-hexadecimal-x-format-specifier"></a>十六进制（“X”）格式说明符

十六进制（“X”）格式说明符将数字转换为十六进制数的字符串。 格式说明符的大小写指示对大于 9 的十六进制数使用大写字符还是小写字符。 例如，使用“X”产生“ABCDEF”，使用“x”产生“abcdef”。 只有整型才支持此格式。

精度说明符指示结果字符串中所需的最少数字个数。 如果需要的话，则用零填充该数字的左侧，以产生精度说明符给定的数字个数。

结果字符串不受当前 <xref:System.Globalization.NumberFormatInfo> 对象的格式信息的影响。

下面的示例使用十六进制数法格式说明符设置 <xref:System.Int32> 值的格式。

[!code-cpp[Formatting.Numeric.Standard#9](../../../samples/snippets/cpp/VS_Snippets_CLR/Formatting.Numeric.Standard/cpp/Standard.cpp#9)]
[!code-csharp-interactive[Formatting.Numeric.Standard#9](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.Numeric.Standard/cs/Standard.cs#9)]
[!code-vb[Formatting.Numeric.Standard#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.Numeric.Standard/vb/Standard.vb#9)]

[返回表首](#table)

<a name="NotesStandardFormatting"></a>

## <a name="notes"></a>说明

### <a name="control-panel-settings"></a>控制面板设置

控制面板中 **“区域和语言选项”** 项中的设置会影响由格式化操作产生的结果字符串。 这些设置用于初始化与当前线程区域性关联的 <xref:System.Globalization.NumberFormatInfo> 对象，当前线程区域性提供用于控制格式设置的值。 使用不同设置的计算机将生成不同的结果字符串。

此外，如果使用 <xref:System.Globalization.CultureInfo.%23ctor%28System.String%29> 构造函数实例化表示当前系统区域性的新 <xref:System.Globalization.CultureInfo> 对象，通过控制面板中的“区域和语言选项”项创建的任何自定义都会应用于新 <xref:System.Globalization.CultureInfo> 对象。 可以使用 <xref:System.Globalization.CultureInfo.%23ctor%28System.String%2CSystem.Boolean%29> 构造函数来创建不会反映系统的自定义项的 <xref:System.Globalization.CultureInfo> 对象。

### <a name="numberformatinfo-properties"></a>NumberFormatInfo 属性

格式设置受当前 <xref:System.Globalization.NumberFormatInfo> 对象的属性影响，它由当前线程区域性隐式提供或由调用格式设置的方法的 <xref:System.IFormatProvider> 参数显式提供。 为该参数指定 <xref:System.Globalization.NumberFormatInfo> 或 <xref:System.Globalization.CultureInfo> 对象。

> [!NOTE]
> 有关自定义用于格式化数值的模式或字符串的信息，请参见 <xref:System.Globalization.NumberFormatInfo> 类主题。

### <a name="integral-and-floating-point-numeric-types"></a>整型和浮点型数值类型

对标准数字格式说明符的一些说明涉及到整型或浮点型数值类型。 整型数值类型包括 <xref:System.Byte>、<xref:System.SByte>、<xref:System.Int16>、<xref:System.Int32>、<xref:System.Int64>、<xref:System.UInt16>、<xref:System.UInt32>、<xref:System.UInt64> 和 <xref:System.Numerics.BigInteger>。 浮点型数值类型有 <xref:System.Decimal>、<xref:System.Single> 和 <xref:System.Double>。

### <a name="floating-point-infinities-and-nan"></a>浮点型无穷大和 NaN

无论格式字符串原来是什么值，只要 <xref:System.Single> 或 <xref:System.Double> 浮点类型的值为正无穷大、负无穷大或非数值 (NaN)，格式字符串就分别是当前适用的 <xref:System.Globalization.NumberFormatInfo.PositiveInfinitySymbol%2A> 对象指定的 <xref:System.Globalization.NumberFormatInfo.NegativeInfinitySymbol%2A>、<xref:System.Globalization.NumberFormatInfo.NaNSymbol%2A> 或 <xref:System.Globalization.NumberFormatInfo> 属性的值。

## <a name="example"></a>示例

[!INCLUDE[interactive-note](~/includes/csharp-interactive-partial-note.md)]

下面的示例使用 en-US 区域性和所有标准数字格式说明符设置一个整型数值和一个浮点型数值的格式。 此示例使用两个特定的数值类型（<xref:System.Double> 和 <xref:System.Int32>），但对于任何一个其他数值基类型（<xref:System.Byte>、<xref:System.SByte>、<xref:System.Int16>、<xref:System.Int32>、<xref:System.Int64>、<xref:System.UInt16>、<xref:System.UInt32>、<xref:System.UInt64>、<xref:System.Numerics.BigInteger>、<xref:System.Decimal> 和 <xref:System.Single>）都将产生类似的结果。

[!code-csharp[system.x.tostring-and-culture#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.X.ToString-and-Culture/cs/xts.cs#FinalExample)]
[!code-vb[system.x.tostring-and-culture#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.X.ToString-and-Culture/vb/xts.vb#1)]

## <a name="see-also"></a>请参阅

- <xref:System.Globalization.NumberFormatInfo>
- [自定义数字格式字符串](custom-numeric-format-strings.md)
- [格式设置类型](formatting-types.md)
- [如何：用前导零填充数字](how-to-pad-a-number-with-leading-zeros.md)
- [复合格式设置](composite-formatting.md)
- [示例：.NET Core WinForms 格式设置实用工具 (C#)](/samples/dotnet/samples/windowsforms-formatting-utility-cs)
- [示例：.NET Core WinForms 格式设置实用工具 (Visual Basic)](/samples/dotnet/samples/windowsforms-formatting-utility-vb)
