---
title: 自定义日期和时间格式字符串
description: 了解如何使用自定义日期和时间格式字符串将 DateTime 或 DateTimeOffset 值转换为文本表示形式，或分析日期和时间字符串。
ms.date: 03/30/2017
ms.topic: reference
dev_langs:
- csharp
- vb
helpviewer_keywords:
- formatting [.NET], dates
- custom DateTime format string
- format specifiers, custom date and time
- format strings
- custom date and time format strings
- formatting [.NET], time
- date and time strings
ms.assetid: 98b374e3-0cc2-4c78-ab44-efb671d71984
ms.openlocfilehash: 70e68de3ce736bc4935dea80e37cc805068b0bc5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722461"
---
# <a name="custom-date-and-time-format-strings"></a>自定义日期和时间格式字符串

日期和时间格式字符串定义由格式设置操作生成的 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 值的文本表示形式。 它还可定义分析操作中需要的日期和时间值的表示形式，以便成功将字符串转换为日期和时间。 自定义格式字符串由一个或多个自定义日期和时间格式说明符组成。 任何不是[标准日期和时间格式字符串](standard-date-and-time-format-strings.md)的字符串都会解释为自定义日期和时间格式字符串。

> [!TIP]
> 你可以下载格式设置实用工具，它属于一种 .NET Core Windows 窗体应用程序，通过该应用程序，你可将格式字符串应用于数值或日期和时间值并显示结果字符串。 源代码适用于 [C#](/samples/dotnet/samples/windowsforms-formatting-utility-cs) 和 [Visual Basic](/samples/dotnet/samples/windowsforms-formatting-utility-vb)。

自定义日期和时间格式字符串可以与 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 值一起使用。

[!INCLUDE[C# interactive-note](~/includes/csharp-interactive-with-utc-partial-note.md)]

<a name="table"></a>在格式设置操作中，可将自定义日期和时间格式字符串与日期和时间实例的 `ToString` 方法或支持复合格式设置的方法结合使用。 下面的示例演示了这两种用法。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#17](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/custandformatting1.cs#17)]
[!code-vb[Formatting.DateAndTime.Custom#17](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/custandformatting1.vb#17)]

在分析操作中，自定义日期和时间格式字符串可用于 <xref:System.DateTime.ParseExact%2A?displayProperty=nameWithType>、<xref:System.DateTime.TryParseExact%2A?displayProperty=nameWithType>、<xref:System.DateTimeOffset.ParseExact%2A?displayProperty=nameWithType> 和 <xref:System.DateTimeOffset.TryParseExact%2A?displayProperty=nameWithType> 方法。 这些方法需要一个完全符合使分析操作成功所需的特定模式的输入字符串。 下面的示例演示对 <xref:System.DateTimeOffset.ParseExact%28System.String%2CSystem.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> 方法的调用，以分析必须包括日、月和两位数年份的日期。

[!code-csharp[Formatting.DateAndTime.Custom#18](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/custandparsing1.cs#18)]
[!code-vb[Formatting.DateAndTime.Custom#18](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/custandparsing1.vb#18)]

下表描述自定义日期和时间格式说明符并显示由每个格式说明符生成的结果字符串。 默认情况下，结果字符串反映 zh-cn 区域性的格式设置约定。 如果特定格式说明符生成本地化结果字符串，则该示例还注明结果字符串适用的区域性。 有关使用自定义日期和时间格式字符串的详细信息，请参阅[备注](#notes)部分。

| 格式说明符 | 描述 | 示例 |
|--|--|--|
| "d" | 一个月中的某一天（1 到 31）。<br /><br /> 更多信息：[“d”自定义格式说明符](#dSpecifier)。 | 2009-06-01T13:45:30 -> 1<br /><br /> 2009-06-15T13:45:30 -> 15 |
| “dd” | 一个月中的某一天（01 到 31）。<br /><br /> 更多信息：[“dd”自定义格式说明符](#ddSpecifier)。 | 2009-06-01T13:45:30 -> 01<br /><br /> 2009-06-15T13:45:30 -> 15 |
| “ddd” | 一周中某天的缩写名称。<br /><br /> 更多信息：[“ddd”自定义格式说明符](#dddSpecifier)。 | 2009-06-15T13:45:30 -> Mon (en-US)<br /><br /> 2009-06-15T13:45:30 -> Пн (ru-RU)<br /><br /> 2009-06-15T13:45:30 -> lun. (fr-FR) |
| “dddd” | 一周中某天的完整名称。<br /><br /> 更多信息：[“dddd”自定义格式说明符](#ddddSpecifier)。 | 2009-06-15T13:45:30 -> Monday (en-US)<br /><br /> 2009-06-15T13:45:30 -> понедельник (ru-RU)<br /><br /> 2009-06-15T13:45:30 -> lundi (fr-FR) |
| “f” | 日期和时间值的十分之几秒。<br /><br /> 更多信息：[“f”自定义格式说明符](#fSpecifier)。 | 2009-06-15T13:45:30.6170000 -> 6<br /><br /> 2009-06-15T13:45:30.05 -> 0 |
| “ff” | 日期和时间值的百分之几秒。<br /><br /> 更多信息：[“ff”自定义格式说明符](#ffSpecifier)。 | 2009-06-15T13:45:30.6170000 -> 61<br /><br /> 2009-06-15T13:45:30.0050000 -> 00 |
| “fff” | 日期和时间值的千分之几秒。<br /><br /> 更多信息：[“fff”自定义格式说明符](#fffSpecifier)。 | 6/15/2009 13:45:30.617 -> 617<br /><br /> 6/15/2009 13:45:30.0005 -> 000 |
| “ffff” | 日期和时间值的万分之几秒。<br /><br /> 更多信息：[“ffff”自定义格式说明符](#ffffSpecifier)。 | 2009-06-15T13:45:30.6175000 -> 6175<br /><br /> 2009-06-15T13:45:30.0000500  -> 0000 |
| “fffff” | 日期和时间值的十万分之几秒。<br /><br /> 更多信息：[“fffff”自定义格式说明符](#fffffSpecifier)。 | 2009-06-15T13:45:30.6175400 -> 61754<br /><br /> 6/15/2009 13:45:30.000005 -> 00000 |
| “ffffff” | 日期和时间值的百万分之几秒。<br /><br /> 更多信息：[“ffffff”自定义格式说明符](#ffffffSpecifier)。 | 2009-06-15T13:45:30.6175420 -> 617542<br /><br /> 2009-06-15T13:45:30.0000005 -> 000000 |
| “fffffff” | 日期和时间值的千万分之几秒。<br /><br /> 更多信息：[“fffffff”自定义格式说明符](#fffffffSpecifier)。 | 2009-06-15T13:45:30.6175425 -> 6175425<br /><br /> 2009-06-15T13:45:30.0001150 -> 0001150 |
| “F” | 如果非零，则为日期和时间值的十分之几秒。<br /><br /> 更多信息：[“F”自定义格式说明符](#F_Specifier)。 | 2009-06-15T13:45:30.6170000 -> 6<br /><br /> 2009-06-15T13:45:30.0500000 ->（无输出） |
| “FF” | 如果非零，则为日期和时间值的百分之几秒。<br /><br /> 更多信息：[“FF”自定义格式说明符](#FF_Specifier)。 | 2009-06-15T13:45:30.6170000 -> 61<br /><br /> 2009-06-15T13:45:30.0050000 ->（无输出） |
| “FFF” | 如果非零，则为日期和时间值的千分之几秒。<br /><br /> 更多信息：[“FFF”自定义格式说明符](#FFF_Specifier)。 | 2009-06-15T13:45:30.6170000 -> 617<br /><br /> 2009-06-15T13:45:30.0005000 ->（无输出） |
| “FFFF” | 如果非零，则为日期和时间值的万分之几秒。<br /><br /> 更多信息：[“FFFF”自定义格式说明符](#FFFF_Specifier)。 | 2009-06-15T13:45:30.5275000 -> 5275<br /><br /> 2009-06-15T13:45:30.0000500 ->（无输出） |
| “FFFFF” | 如果非零，则为日期和时间值的十万分之几秒。<br /><br /> 更多信息：[“FFFFF”自定义格式说明符](#FFFFF_Specifier)。 | 2009-06-15T13:45:30.6175400 -> 61754<br /><br /> 2009-06-15T13:45:30.0000050 ->（无输出） |
| “FFFFFF” | 如果非零，则为日期和时间值的百万分之几秒。<br /><br /> 更多信息：[“FFFFFF”自定义格式说明符](#FFFFFF_Specifier)。 | 2009-06-15T13:45:30.6175420 -> 617542<br /><br /> 2009-06-15T13:45:30.0000005 ->（无输出） |
| “FFFFFFF” | 如果非零，则为日期和时间值的千万分之几秒。<br /><br /> 更多信息：[“FFFFFFF”自定义格式说明符](#FFFFFFF_Specifier)。 | 2009-06-15T13:45:30.6175425 -> 6175425<br /><br /> 2009-06-15T13:45:30.0001150 -> 000115 |
| “g”、“gg” | 时期或纪元。<br /><br /> 更多信息：[“g”或“gg”自定义格式说明符](#gSpecifier)。 | 2009-06-15T13:45:30.6170000 -> A.D. |
| “h” | 采用 12 小时制的小时（从 1 到 12）。<br /><br /> 更多信息：[“h”自定义格式说明符](#hSpecifier)。 | 2009-06-15T01:45:30 -> 1<br /><br /> 2009-06-15T13:45:30 -> 1 |
| “hh” | 采用 12 小时制的小时（从 01 到 12）。<br /><br /> 更多信息：[“hh”自定义格式说明符](#hhSpecifier)。 | 2009-06-15T01:45:30 -> 01<br /><br /> 2009-06-15T13:45:30 -> 01 |
| “H” | 采用 24 小时制的小时（从 0 到 23）。<br /><br /> 更多信息：[“H”自定义格式说明符](#H_Specifier)。 | 2009-06-15T01:45:30 -> 1<br /><br /> 2009-06-15T13:45:30 -> 13 |
| “HH” | 采用 24 小时制的小时（从 00 到 23）。<br /><br /> 更多信息：[“HH”自定义格式说明符](#HH_Specifier)。 | 2009-06-15T01:45:30 -> 01<br /><br /> 2009-06-15T13:45:30 -> 13 |
| “K” | 时区信息。<br /><br /> 更多信息：[“K”自定义格式说明符](#KSpecifier)。 | 带 <xref:System.DateTime> 值：<br /><br /> 2009-06-15T13:45:30, Kind Unspecified -><br /><br /> 2009-06-15T13:45:30, Kind Utc -> Z<br /><br /> 2009-06-15T13:45:30, Kind Local -> -07:00（取决于本地计算机的设置）<br /><br /> 带 <xref:System.DateTimeOffset> 值：<br /><br /> 2009-06-15T01:45:30-07:00 --> -07:00<br /><br /> 2009-06-15T08:45:30+00:00 --> +00:00 |
| “m” | 分钟（0 到 59）。<br /><br /> 更多信息：[“m”自定义格式说明符](#mSpecifier)。 | 2009-06-15T01:09:30 -> 9<br /><br /> 2009-06-15T13:29:30 -> 29 |
| “mm” | 分钟（00 到 59）。<br /><br /> 更多信息：[“mm”自定义格式说明符](#mmSpecifier)。 | 2009-06-15T01:09:30 -> 09<br /><br /> 2009-06-15T01:45:30 -> 45 |
| “M” | 月份（1 到 12）。<br /><br /> 更多信息：[“M”自定义格式说明符](#M_Specifier)。 | 2009-06-15T13:45:30 -> 6 |
| “MM” | 月份（1 到 12）。<br /><br /> 更多信息：[“MM”自定义格式说明符](#MM_Specifier)。 | 2009-06-15T13:45:30 -> 06 |
| “MMM” | 月份的缩写名称。<br /><br /> 更多信息：[“MMM”自定义格式说明符](#MMM_Specifier)。 | 2009-06-15T13:45:30 -> Jun (en-US)<br /><br /> 2009-06-15T13:45:30 -> juin (fr-FR)<br /><br /> 2009-06-15T13:45:30 -> Jun (zu-ZA) |
| “MMMM” | 月份的完整名称。<br /><br /> 更多信息：[“MMMM”自定义格式说明符](#MMMM_Specifier)。 | 2009-06-15T13:45:30 -> June (en-US)<br /><br /> 2009-06-15T13:45:30 -> juni (da-DK)<br /><br /> 2009-06-15T13:45:30 -> uJuni (zu-ZA) |
| “s” | 秒（0 到 59）。<br /><br /> 更多信息：[“s”自定义格式说明符](#sSpecifier)。 | 2009-06-15T13:45:09 -> 9 |
| “ss” | 秒（00 到 59）。<br /><br /> 更多信息：[“ss”自定义格式说明符](#ssSpecifier)。 | 2009-06-15T13:45:09 -> 09 |
| “t” | AM/PM 指示符的第一个字符。<br /><br /> 更多信息：[“t”自定义格式说明符](#tSpecifier)。 | 2009-06-15T13:45:30 -> P (en-US)<br /><br /> 2009-06-15T13:45:30 -> 午 (ja-JP)<br /><br /> 2009-06-15T13:45:30 ->  (fr-FR) |
| “tt” | AM/PM 指示符。<br /><br /> 更多信息：[“tt”自定义格式说明符](#ttSpecifier)。 | 2009-06-15T13:45:30 -> PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> 午後 (ja-JP)<br /><br /> 2009-06-15T13:45:30 ->  (fr-FR) |
| “y” | 年份（0 到 99）。<br /><br /> 更多信息：[“y”自定义格式说明符](#ySpecifier)。 | 0001-01-01T00:00:00 -> 1<br /><br /> 0900-01-01T00:00:00 -> 0<br /><br /> 1900-01-01T00:00:00 -> 0<br /><br /> 2009-06-15T13:45:30 -> 9<br /><br /> 2019-06-15T13:45:30 -> 19 |
| “yy” | 年份（00 到 99）。<br /><br /> 更多信息：[“yy”自定义格式说明符](#yySpecifier)。 | 0001-01-01T00:00:00 -> 01<br /><br /> 0900-01-01T00:00:00 -> 00<br /><br /> 1900-01-01T00:00:00 -> 00<br /><br /> 2019-06-15T13:45:30 -> 19 |
| “yyy” | 年份（最少三位数字）。<br /><br /> 更多信息：[“yyy”自定义格式说明符](#yyySpecifier)。 | 0001-01-01T00:00:00 -> 001<br /><br /> 0900-01-01T00:00:00 -> 900<br /><br /> 1900-01-01T00:00:00 -> 1900<br /><br /> 2009-06-15T13:45:30 -> 2009 |
| “yyyy” | 由四位数字表示的年份。<br /><br /> 更多信息：[“yyyy”自定义格式说明符](#yyyySpecifier)。 | 0001-01-01T00:00:00 -> 0001<br /><br /> 0900-01-01T00:00:00 -> 0900<br /><br /> 1900-01-01T00:00:00 -> 1900<br /><br /> 2009-06-15T13:45:30 -> 2009 |
| “yyyyy” | 由五位数字表示的年份。<br /><br /> 更多信息：[“yyyyy”自定义格式说明符](#yyyyySpecifier)。 | 0001-01-01T00:00:00 -> 00001<br /><br /> 2009-06-15T13:45:30 -> 02009 |
| “z” | 相对于 UTC 的小时偏移量，无前导零。<br /><br /> 更多信息：[“z”自定义格式说明符](#zSpecifier)。 | 2009-06-15T13:45:30-07:00 -> -7 |
| “zz” | 相对于 UTC 的小时偏移量，带有表示一位数值的前导零。<br /><br /> 更多信息：[“zz”自定义格式说明符](#zzSpecifier)。 | 2009-06-15T13:45:30-07:00 -> -07 |
| “zzz” | 相对于 UTC 的小时和分钟偏移量。<br /><br /> 更多信息：[“zzz”自定义格式说明符](#zzzSpecifier)。 | 2009-06-15T13:45:30-07:00 -> -07:00 |
| ":" | 时间分隔符。<br /><br /> 更多信息：[“:”自定义格式说明符](#timeSeparator)。 | 2009-06-15T13:45:30 -> : (en-US)<br /><br /> 2009-06-15T13:45:30 -> . (it-IT)<br /><br /> 2009-06-15T13:45:30 -> : (ja-JP) |
| "/" | 日期分隔符。<br /><br /> 详细信息：[“/”自定义格式说明符](#dateSeparator)。 | 2009-06-15T13:45:30 -> / (en-US)<br /><br /> 2009-06-15T13:45:30 -> - (ar-DZ)<br /><br /> 2009-06-15T13:45:30 -> . (tr-TR) |
| "string"<br /><br /> 'string' | 文本字符串分隔符。<br /><br /> 更多信息：[字符文本](#Literals)。 | 2009-06-15T13:45:30 ("arr:" h:m t) -> arr:1:45 P<br /><br /> 2009-06-15T13:45:30 ('arr:' h:m t) -> arr:1:45 P |
| % | 将下面的字符定义为自定义格式说明符。<br /><br /> 有关详细信息，请参阅[使用单个自定义格式说明符](#UsingSingleSpecifiers)。 | 2009-06-15T13:45:30 (%h) -> 1 |
| &#92; | 转义字符。<br /><br /> 更多信息：[字符文本](#Literals)和[使用转义字符](#escape)。 | 2009-06-15T13:45:30 (h \h) -> 1 h |
| 任何其他字符 | 字符将复制到未更改的结果字符串。<br /><br /> 更多信息：[字符文本](#Literals)。 | 2009-06-15T01:45:30 (arr hh:mm t) -> arr 01:45 A |

以下各节提供有关每个自定义日期和时间格式说明符的附加信息。 除非另行说明，否则，每个说明符将生成相同的字符串表示形式，这与它是与 <xref:System.DateTime> 值一起使用还是与 <xref:System.DateTimeOffset> 值一起使用无关。

## <a name="day-d-format-specifier"></a>日期“d”格式说明符

### <a name="the-d-custom-format-specifier"></a><a name="dSpecifier"></a>“d”自定义格式说明符

“d”自定义格式说明符将一个月中的某一天表示为从 1 到 31 的数字。 一位数的日期设置为不带前导零的格式。

如果使用“d”格式说明符而没有其他自定义格式说明符，则将该说明符解释为“d”标准日期和时间格式说明符。 有关使用单个格式说明符的更多信息，请参阅本文后面的[使用单个自定义格式说明符](#UsingSingleSpecifiers)。

下面的示例在几个格式字符串中包含“d”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#1](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#1)]
[!code-vb[Formatting.DateAndTime.Custom#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#1)]

[返回表首](#table)

### <a name="the-dd-custom-format-specifier"></a><a name="ddSpecifier"></a>“dd”自定义格式说明符

“dd”自定义格式字符串将一个月中的某一天表示为从 01 到 31 的数字。 一位数的日期设置为带有前导零的格式。

下面的示例在一个自定义格式字符串中包含“dd”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#2](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#2)]
[!code-vb[Formatting.DateAndTime.Custom#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#2)]

[返回表首](#table)

### <a name="the-ddd-custom-format-specifier"></a><a name="dddSpecifier"></a>“ddd”自定义格式说明符

“ddd”自定义格式说明符表示一周中某天的缩写名称。 一周中某天的本地化缩写名称通过当前或指定区域性的 <xref:System.Globalization.DateTimeFormatInfo.AbbreviatedDayNames%2A?displayProperty=nameWithType> 属性进行检索。

下面的示例在一个自定义格式字符串中包含“ddd”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#3](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#3)]
[!code-vb[Formatting.DateAndTime.Custom#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#3)]

[返回表首](#table)

### <a name="the-dddd-custom-format-specifier"></a><a name="ddddSpecifier"></a>“dddd”自定义格式说明符

“dddd”自定义格式说明符（以及任意数量的附加“d”说明符）表示一周中某天的完整名称。 一周中某天的本地化名称通过当前或指定区域性的 <xref:System.Globalization.DateTimeFormatInfo.DayNames%2A?displayProperty=nameWithType> 属性进行检索。

下面的示例在一个自定义格式字符串中包含“dddd”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#4](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#4)]
[!code-vb[Formatting.DateAndTime.Custom#4](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#4)]

[返回表首](#table)

## <a name="lowercase-seconds-f-fraction-specifier"></a>小写秒“f”分数说明符

### <a name="the-f-custom-format-specifier"></a><a name="fSpecifier"></a>“f”自定义格式说明符

“f”自定义格式说明符表示秒部分的最高有效位；也就是说，它表示日期和时间值的十分之几秒。

如果使用“f”格式说明符而没有其他格式说明符，则将该说明符解释为“f”标准日期和时间格式说明符。 有关使用单个格式说明符的更多信息，请参阅本文后面的[使用单个自定义格式说明符](#UsingSingleSpecifiers)。

当使用作为格式字符串的一部分提供给 <xref:System.DateTime.ParseExact%2A>、<xref:System.DateTime.TryParseExact%2A>、<xref:System.DateTimeOffset.ParseExact%2A> 或 <xref:System.DateTimeOffset.TryParseExact%2A> 方法的“f”格式说明符时，所用的“f”格式说明符的数目指示为成功分析字符串而必须呈现的秒部分的最高有效位的数目。

下面的示例在一个自定义格式字符串中包含“f”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#5)]
[!code-vb[Formatting.DateAndTime.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#5)]

[返回表首](#table)

### <a name="the-ff-custom-format-specifier"></a><a name="ffSpecifier"></a>“ff”自定义格式说明符

“ff”自定义格式说明符表示秒部分的前两个有效位；也就是说，它表示日期和时间值的百分之几秒。

下面的示例在一个自定义格式字符串中包含“ff”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#5)]
[!code-vb[Formatting.DateAndTime.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#5)]

[返回表首](#table)

### <a name="the-fff-custom-format-specifier"></a><a name="fffSpecifier"></a>“fff”自定义格式说明符

“fff”自定义格式说明符表示秒部分的前三个有效位；也就是说，它表示日期和时间值的毫秒。

下面的示例在一个自定义格式字符串中包含“fff”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#5)]
[!code-vb[Formatting.DateAndTime.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#5)]

[返回表首](#table)

### <a name="the-ffff-custom-format-specifier"></a><a name="ffffSpecifier"></a>“ffff”自定义格式说明符

“ffff”自定义格式说明符表示秒部分的前四个有效位；也就是说，它表示日期和时间值的万分之几秒。

尽管可以显示时间值的秒部分的万分之几秒，但是该值可能并没有意义。 日期和时间值的精度取决于系统时钟的分辨率。 在 Windows NT 3.5 版和更高版本以及 Windows Vista 操作系统上，时钟的分辨率大约为 10-15 毫秒。

[返回表首](#table)

### <a name="the-fffff-custom-format-specifier"></a><a name="fffffSpecifier"></a>“fffff”自定义格式说明符

“fffff”自定义格式说明符表示秒部分的前五个有效位；也就是说，它表示日期和时间值的十万分之几秒。

尽管可以显示时间值的秒部分的十万分之几秒，但是该值可能并没有意义。 日期和时间值的精度取决于系统时钟的分辨率。 在 Windows NT 3.5 和更高版本以及 Windows Vista 操作系统上，时钟的分辨率大约为 10-15 毫秒。

[返回表首](#table)

### <a name="the-ffffff-custom-format-specifier"></a><a name="ffffffSpecifier"></a>“ffffff”自定义格式说明符

“ffffff”自定义格式说明符表示秒部分的前六个有效位；也就是说，它表示日期和时间值的百万分之几秒。

尽管可以显示时间值的秒部分的百万分之几秒，但是该值可能并没有意义。 日期和时间值的精度取决于系统时钟的分辨率。 在 Windows NT 3.5 和更高版本以及 Windows Vista 操作系统上，时钟的分辨率大约为 10-15 毫秒。

[返回表首](#table)

### <a name="the-fffffff-custom-format-specifier"></a><a name="fffffffSpecifier"></a>“fffffff”自定义格式说明符

“fffffff”自定义格式说明符表示秒部分的前七个有效位；也就是说，它表示日期和时间值的千万分之几秒。

尽管可以显示时间值的秒部分的千万分之几秒，但是该值可能并没有意义。 日期和时间值的精度取决于系统时钟的分辨率。 在 Windows NT 3.5 和更高版本以及 Windows Vista 操作系统上，时钟的分辨率大约为 10-15 毫秒。

[返回表首](#table)

## <a name="uppercase-seconds-f-fraction-specifier"></a>大写秒“F”分数说明符

### <a name="the-f-custom-format-specifier"></a><a name="F_Specifier"></a>“F”自定义格式说明符

“F”自定义格式说明符表示秒部分的最高有效位；也就是说，它表示日期和时间值的十分之几秒。 如果该数字为零，则不显示任何内容。

如果使用“F”格式说明符而没有其他格式说明符，则将该说明符解释为“F”标准日期和时间格式说明符。 有关使用单个格式说明符的更多信息，请参阅本文后面的[使用单个自定义格式说明符](#UsingSingleSpecifiers)。

用于 <xref:System.DateTime.ParseExact%2A>、<xref:System.DateTime.TryParseExact%2A>、<xref:System.DateTimeOffset.ParseExact%2A> 或 <xref:System.DateTimeOffset.TryParseExact%2A> 方法的“F”格式说明符的数目指示为成功分析字符串而可以呈现的秒部分的最高有效位的最大数目。

下面的示例在一个自定义格式字符串中包含“F”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#5)]
[!code-vb[Formatting.DateAndTime.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#5)]

[返回表首](#table)

### <a name="the-ff-custom-format-specifier"></a><a name="FF_Specifier"></a>“FF”自定义格式说明符

“FF”自定义格式说明符表示秒部分的前两个有效位；也就是说，它表示日期和时间值的百分之几秒。 但不显示尾随零或两个零位。

下面的示例在一个自定义格式字符串中包含“FF”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#5)]
[!code-vb[Formatting.DateAndTime.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#5)]

[返回表首](#table)

### <a name="the-fff-custom-format-specifier"></a><a name="FFF_Specifier"></a>“FFF”自定义格式说明符

“FFF”自定义格式说明符表示秒部分的前三个有效位；也就是说，它表示日期和时间值的毫秒。 但不显示尾随零或三个零位。

下面的示例在一个自定义格式字符串中包含“FFF”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#5)]
[!code-vb[Formatting.DateAndTime.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#5)]

[返回表首](#table)

### <a name="the-ffff-custom-format-specifier"></a><a name="FFFF_Specifier"></a>“FFFF”自定义格式说明符

“FFFF”自定义格式说明符表示秒部分的前四个有效位；也就是说，它表示日期和时间值的万分之几秒。 但不显示尾随零或四个零位。

尽管可以显示时间值的秒部分的万分之几秒，但是该值可能并没有意义。 日期和时间值的精度取决于系统时钟的分辨率。 在 Windows NT 3.5 和更高版本以及 Windows Vista 操作系统上，时钟的分辨率大约为 10-15 毫秒。

[返回表首](#table)

### <a name="the-fffff-custom-format-specifier"></a><a name="FFFFF_Specifier"></a>“FFFFF”自定义格式说明符

“FFFFF”自定义格式说明符表示秒部分的前五个有效位；也就是说，它表示日期和时间值的十万分之几秒。 但不显示尾随零或五个零位。

尽管可以显示时间值的秒部分的十万分之几秒，但是该值可能并没有意义。 日期和时间值的精度取决于系统时钟的分辨率。 在 Windows NT 3.5 和更高版本以及 Windows Vista 操作系统上，时钟的分辨率大约为 10-15 毫秒。

[返回表首](#table)

### <a name="the-ffffff-custom-format-specifier"></a><a name="FFFFFF_Specifier"></a>“FFFFFF”自定义格式说明符

“FFFFFF”自定义格式说明符表示秒部分的前六个有效位；也就是说，它表示日期和时间值的百万分之几秒。 但不显示尾随零或六个零位。

尽管可以显示时间值的秒部分的百万分之几秒，但是该值可能并没有意义。 日期和时间值的精度取决于系统时钟的分辨率。 在 Windows NT 3.5 和更高版本以及 Windows Vista 操作系统上，时钟的分辨率大约为 10-15 毫秒。

[返回表首](#table)

### <a name="the-fffffff-custom-format-specifier"></a><a name="FFFFFFF_Specifier"></a>“FFFFFFF”自定义格式说明符

“FFFFFFF”自定义格式说明符表示秒部分的前七个有效位；也就是说，它表示日期和时间值的千万分之几秒。 但不显示尾随零或七个零位。

尽管可以显示时间值的秒部分的千万分之几秒，但是该值可能并没有意义。 日期和时间值的精度取决于系统时钟的分辨率。 在 Windows NT 3.5 和更高版本以及 Windows Vista 操作系统上，时钟的分辨率大约为 10-15 毫秒。

[返回表首](#table)

## <a name="era-g-format-specifier"></a>纪元“g”格式说明符

### <a name="the-g-or-gg-custom-format-specifier"></a><a name="gSpecifier"></a>“g”或“gg”自定义格式说明符

“g”或“gg”自定义格式说明符（以及任意数量的附加“g”说明符）表示时期或纪元（例如 A.D）。 如果要设置格式的日期没有关联的时期或纪元字符串，则格式设置操作将忽略此说明符。

如果使用“g”格式说明符而没有其他自定义格式说明符，则将该说明符解释为“g”标准日期和时间格式说明符。 有关使用单个格式说明符的更多信息，请参阅本文后面的[使用单个自定义格式说明符](#UsingSingleSpecifiers)。

下面的示例在一个自定义格式字符串中包含“g”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#6](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#6)]
[!code-vb[Formatting.DateAndTime.Custom#6](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#6)]

[返回表首](#table)

## <a name="lowercase-hour-h-format-specifier"></a>小写小时“h”格式说明符

### <a name="the-h-custom-format-specifier"></a><a name="hSpecifier"></a>“h”自定义格式说明符

“h”自定义格式说明符将小时表示为从 1 至 12 的数字，即采用 12 小时制表示小时，自午夜或中午开始对整小时计数。 午夜后经过的某特定小时数与中午过后的相同小时数无法加以区分。 小时数不进行舍入，一位数字的小时数设置为不带前导零的格式。 例如，给定上午或下午时间为 5:43，则此自定义格式说明符显示“5”。

如果使用“h”格式说明符而没有其他自定义格式说明符，则将该说明符解释为标准日期和时间格式说明符，并引发 <xref:System.FormatException>。 有关使用单个格式说明符的更多信息，请参阅本文后面的[使用单个自定义格式说明符](#UsingSingleSpecifiers)。

下面的示例在一个自定义格式字符串中包含“h”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#7](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#7)]
[!code-vb[Formatting.DateAndTime.Custom#7](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#7)]

[返回表首](#table)

### <a name="the-hh-custom-format-specifier"></a><a name="hhSpecifier"></a>“hh”自定义格式说明符

“hh”自定义格式说明符（以及任意数量的附加“h”说明符）将小时表示为从 01 至 12 的数字，即采用 12 小时制表示小时，自午夜或中午开始对整小时计数。 午夜后经过的某特定小时数与中午过后的相同小时数无法加以区分。 小时数不进行舍入，一位数字的小时数设置为带前导零的格式。 例如，给定上午或下午时间为 5:43，则此格式说明符显示“05”。

下面的示例在一个自定义格式字符串中包含“hh”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#8](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#8)]
[!code-vb[Formatting.DateAndTime.Custom#8](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#8)]

[返回表首](#table)

## <a name="uppercase-hour-h-format-specifier"></a>大写小时“H”格式说明符

### <a name="the-h-custom-format-specifier"></a><a name="H_Specifier"></a>“H”自定义格式说明符

“H”自定义格式说明符将小时表示为从 0 至 23 的数字，即通过从零开始的 24 小时制表示小时，自午夜或中午开始对整小时计数。 一位数字的小时数设置为不带前导零的格式。

如果使用“H”格式说明符而没有其他自定义格式说明符，则将该说明符解释为标准日期和时间格式说明符，并引发 <xref:System.FormatException>。 有关使用单个格式说明符的更多信息，请参阅本文后面的[使用单个自定义格式说明符](#UsingSingleSpecifiers)。

下面的示例在一个自定义格式字符串中包含“H”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#9](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#9)]
[!code-vb[Formatting.DateAndTime.Custom#9](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#9)]

[返回表首](#table)

### <a name="the-hh-custom-format-specifier"></a><a name="HH_Specifier"></a>“HH”自定义格式说明符

“HH”自定义格式说明符（以及任意数量的附加“H”说明符）将小时表示为从 00 至 23 的数字，即通过从零开始的 24 小时制表示小时，自午夜或中午开始对整小时计数。 一位数字的小时数设置为带前导零的格式。

下面的示例在一个自定义格式字符串中包含“HH”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#10](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#10)]
[!code-vb[Formatting.DateAndTime.Custom#10](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#10)]

[返回表首](#table)

## <a name="time-zone-k-format-specifier"></a>时区“K”格式说明符

### <a name="the-k-custom-format-specifier"></a><a name="KSpecifier"></a>“K”自定义格式说明符

“K”自定义格式说明符表示日期和时间值的时区信息。 当此格式说明符与 <xref:System.DateTime> 值一起使用时，结果字符串由 <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> 属性的值进行定义：

- 对于本地时区（<xref:System.DateTime.Kind%2A?displayProperty=nameWithType> 的 <xref:System.DateTimeKind.Local?displayProperty=nameWithType> 属性值），此说明符等效于“zzz”说明符，并产生一个包含相对于协调世界时 (UTC) 的本地偏移量的结果字符串，例如“-07:00”。

- 对于 UTC 时间（<xref:System.DateTime.Kind%2A?displayProperty=nameWithType> 的 <xref:System.DateTimeKind.Utc?displayProperty=nameWithType> 属性值），结果字符串包含用于表示 UTC 日期的“Z”字符。

- 对于未指定时区的时间（其 <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> 属性等于 <xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType> 的时间），结果等效于 <xref:System.String.Empty?displayProperty=nameWithType>。

对于 <xref:System.DateTimeOffset> 值，"K" 格式说明符相当于 "zzz" 格式说明符，生成的结果字符串包含 <xref:System.DateTimeOffset> 值与 UTC 的偏移。

如果使用“K”格式说明符而没有其他自定义格式说明符，则将该说明符解释为标准日期和时间格式说明符，并引发 <xref:System.FormatException>。 有关使用单个格式说明符的更多信息，请参阅本文后面的[使用单个自定义格式说明符](#UsingSingleSpecifiers)。

下面的示例显示对美国太平洋时区中的系统上的各种 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 值使用“K”自定义格式说明符。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#12](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#12)]
[!code-vb[Formatting.DateAndTime.Custom#12](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#12)]

[返回表首](#table)

## <a name="minute-m-format-specifier"></a>分钟“m”格式说明符

### <a name="the-m-custom-format-specifier"></a><a name="mSpecifier"></a>“m”自定义格式说明符

“m”自定义格式说明符将分钟表示为从 0 到 59 的数字。 分钟表示自上一小时以来经过的整分钟数。 一位数字的分钟数设置为不带前导零的格式。

如果使用“m”格式说明符而没有其他自定义格式说明符，则将该说明符解释为“m”标准日期和时间格式说明符。 有关使用单个格式说明符的更多信息，请参阅本文后面的[使用单个自定义格式说明符](#UsingSingleSpecifiers)。

下面的示例在一个自定义格式字符串中包含“m”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#7](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#7)]
[!code-vb[Formatting.DateAndTime.Custom#7](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#7)]

[返回表首](#table)

### <a name="the-mm-custom-format-specifier"></a><a name="mmSpecifier"></a>“mm”自定义格式说明符

“mm”自定义格式说明符（以及任意数量的附加“m”说明符）将分钟表示为从 00 到 59 的数字。 分钟表示自上一小时以来经过的整分钟数。 一位数字的分钟数设置为带前导零的格式。

下面的示例在一个自定义格式字符串中包含“mm”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#8](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#8)]
[!code-vb[Formatting.DateAndTime.Custom#8](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#8)]

[返回表首](#table)

## <a name="month-m-format-specifier"></a>月份“M”格式说明符

### <a name="the-m-custom-format-specifier"></a><a name="M_Specifier"></a>“M”自定义格式说明符

“M”自定义格式说明符将月份表示为从 1 到 12 的数字（对于有 13 个月的日历，将月份表示为从 1 到 13 的数字）。 一位数字的月份数设置为不带前导零的格式。

如果使用“M”格式说明符而没有其他自定义格式说明符，则将该说明符解释为“M”标准日期和时间格式说明符。 有关使用单个格式说明符的更多信息，请参阅本文后面的[使用单个自定义格式说明符](#UsingSingleSpecifiers)。

下面的示例在一个自定义格式字符串中包含“M”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#11](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#11)]
[!code-vb[Formatting.DateAndTime.Custom#11](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#11)]

[返回表首](#table)

### <a name="the-mm-custom-format-specifier"></a><a name="MM_Specifier"></a>“MM”自定义格式说明符

“MM”自定义格式说明符将月份表示为从 01 到 12 的数字（对于有 13 个月的日历，将月份表示为从 1 到 13 的数字）。 一位数字的月份数设置为带有前导零的格式。

下面的示例在一个自定义格式字符串中包含“MM”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#2](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#2)]
[!code-vb[Formatting.DateAndTime.Custom#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#2)]

[返回表首](#table)

### <a name="the-mmm-custom-format-specifier"></a><a name="MMM_Specifier"></a>“MMM”自定义格式说明符

“MMM”自定义格式说明符表示月份的缩写名称。 月份的本地化缩写名称通过当前或指定区域性的 <xref:System.Globalization.DateTimeFormatInfo.AbbreviatedMonthNames%2A?displayProperty=nameWithType> 属性进行检索。

下面的示例在一个自定义格式字符串中包含“MMM”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#3](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#3)]
[!code-vb[Formatting.DateAndTime.Custom#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#3)]

[返回表首](#table)

### <a name="the-mmmm-custom-format-specifier"></a><a name="MMMM_Specifier"></a>“MMMM”自定义格式说明符

“MMMM”自定义格式说明符表示月份的完整名称。 月份的本地化名称通过当前或指定区域性的 <xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A?displayProperty=nameWithType> 属性进行检索。

下面的示例在一个自定义格式字符串中包含“MMMM”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#4](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#4)]
[!code-vb[Formatting.DateAndTime.Custom#4](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#4)]

[返回表首](#table)

## <a name="seconds-s-format-specifier"></a>秒“s”格式说明符

### <a name="the-s-custom-format-specifier"></a><a name="sSpecifier"></a>“s”自定义格式说明符

“s”自定义格式说明符将秒表示为从 0 到 59 的数字。 结果表示自上一分钟以来经过的整秒数。 一位数字的秒数设置为不带前导零的格式。

如果使用“s”格式说明符而没有其他自定义格式说明符，则将该说明符解释为“s”标准日期和时间格式说明符。 有关使用单个格式说明符的更多信息，请参阅本文后面的[使用单个自定义格式说明符](#UsingSingleSpecifiers)。

下面的示例在一个自定义格式字符串中包含“s”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#7](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#7)]
[!code-vb[Formatting.DateAndTime.Custom#7](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#7)]

[返回表首](#table)

### <a name="the-ss-custom-format-specifier"></a><a name="ssSpecifier"></a>“ss”自定义格式说明符

“ss”自定义格式说明符（以及任意数量的附加“s”说明符）将秒表示为从 00 到 59 的数字。 结果表示自上一分钟以来经过的整秒数。 一位数字的秒数设置为带前导零的格式。

下面的示例在一个自定义格式字符串中包含“ss”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#8](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#8)]
[!code-vb[Formatting.DateAndTime.Custom#8](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#8)]

[返回表首](#table)

## <a name="meridiem-t-format-specifier"></a>正午“t”格式说明符

### <a name="the-t-custom-format-specifier"></a><a name="tSpecifier"></a>“t”自定义格式说明符

“t”自定义格式说明符表示 AM/PM 指示符的第一个字符。 相应的本地化指示符通过当前或特定区域性的 <xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A?displayProperty=nameWithType> 或 <xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A?displayProperty=nameWithType> 属性进行检索。 AM 指示符用于自 0:00:00（午夜）到 11:59:59.999 的所有时间。 PM 指示符用于自 12:00:00（中午）到 23:59:59.999 的所有时间。

如果使用“t”格式说明符而没有其他自定义格式说明符，则将该说明符解释为“t”标准日期和时间格式说明符。 有关使用单个格式说明符的更多信息，请参阅本文后面的[使用单个自定义格式说明符](#UsingSingleSpecifiers)。

下面的示例在一个自定义格式字符串中包含“t”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#7](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#7)]
[!code-vb[Formatting.DateAndTime.Custom#7](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#7)]

[返回表首](#table)

### <a name="the-tt-custom-format-specifier"></a><a name="ttSpecifier"></a>“tt”自定义格式说明符

“tt”自定义格式说明符（以及任意数量的附加“t”说明符）表示整个 AM/PM 指示符。 相应的本地化指示符通过当前或特定区域性的 <xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A?displayProperty=nameWithType> 或 <xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A?displayProperty=nameWithType> 属性进行检索。 AM 指示符用于自 0:00:00（午夜）到 11:59:59.999 的所有时间。 PM 指示符用于自 12:00:00（中午）到 23:59:59.999 的所有时间。

对于需要维护 AM 与 PM 之间的差异的语言，应确保使用“tt”说明符。 以日语为例，其 AM 和 PM 指示符的差异点为第二个字符，而非第一个字符。

下面的示例在一个自定义格式字符串中包含“tt”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#8](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#8)]
[!code-vb[Formatting.DateAndTime.Custom#8](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#8)]

[返回表首](#table)

## <a name="year-y-format-specifier"></a>年份“y”格式说明符

### <a name="the-y-custom-format-specifier"></a><a name="ySpecifier"></a>“y”自定义格式说明符

“y”自定义格式说明符将年份表示为一位或两位数字。 如果年份多于两位数，则结果中仅显示两位低位数。 如果两位数字的年份的第一个数字以零开始（例如，2008），则该数字设置为不带前导零的格式。

如果使用“y”格式说明符而没有其他自定义格式说明符，则将该说明符解释为“y”标准日期和时间格式说明符。 有关使用单个格式说明符的更多信息，请参阅本文后面的[使用单个自定义格式说明符](#UsingSingleSpecifiers)。

下面的示例在一个自定义格式字符串中包含“y”自定义格式说明符。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#13](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#13)]
[!code-vb[Formatting.DateAndTime.Custom#13](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#13)]

[返回表首](#table)

### <a name="the-yy-custom-format-specifier"></a><a name="yySpecifier"></a>“yy”自定义格式说明符

“yy”自定义格式说明符将年份表示为一位或两位数字。 如果年份多于两位数，则结果中仅显示两位低位数。 如果两位数年份的有效数字少于两个，则用前导零填充该数字以产生两位数。

在分析操作中，使用“yy”自定义格式说明符分析的两位数年份是基于格式提供程序的当前日历的 <xref:System.Globalization.Calendar.TwoDigitYearMax%2A?displayProperty=nameWithType> 属性被释义的。 下面的示例通过使用默认 zh-cn 区域性（在这种情况下为当前区域性）的公历来分析具有二位数年份的日期的字符串表示形式。 然后，它将更改当前区域的 <xref:System.Globalization.CultureInfo> 对象以便使用其 <xref:System.Globalization.GregorianCalendar> 属性已更改的 <xref:System.Globalization.GregorianCalendar.TwoDigitYearMax%2A> 对象。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#19](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/parseexact2digityear1.cs#19)]
[!code-vb[Formatting.DateAndTime.Custom#19](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/parseexact2digityear1.vb#19)]

下面的示例在一个自定义格式字符串中包含“yy”自定义格式说明符。

[!code-csharp[Formatting.DateAndTime.Custom#13](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#13)]
[!code-vb[Formatting.DateAndTime.Custom#13](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#13)]

[返回表首](#table)

### <a name="the-yyy-custom-format-specifier"></a><a name="yyySpecifier"></a>“yyy”自定义格式说明符

“yyy”自定义格式说明符至少使用三位数字表示年份。 如果年份的有效数字多于三个，则将它们包括在结果字符串中。 如果年份少于三位数，则用前导零填充该数字以产生三位数。

> [!NOTE]
> 对于年份可以为五位数的泰国佛历，此格式说明符将显示全部有效数字。

下面的示例在一个自定义格式字符串中包含“yyy”自定义格式说明符。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#13](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#13)]
[!code-vb[Formatting.DateAndTime.Custom#13](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#13)]

[返回表首](#table)

### <a name="the-yyyy-custom-format-specifier"></a><a name="yyyySpecifier"></a>“yyyy”自定义格式说明符

“yyyy”自定义格式说明符至少使用四位数字表示年份。 如果年份的有效数字多于四个，则将它们包括在结果字符串中。 如果年份少于四位数，则用前导零填充该数字使其达到四位数。

> [!NOTE]
> 对于年份可以为五位数的泰国佛历，此格式说明符将最少显示四位数字。

下面的示例在一个自定义格式字符串中包含“yyyy”自定义格式说明符。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#13](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#13)]
[!code-vb[Formatting.DateAndTime.Custom#13](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#13)]

[返回表首](#table)

### <a name="the-yyyyy-custom-format-specifier"></a><a name="yyyyySpecifier"></a>“yyyyy”自定义格式说明符

“yyyyy”自定义格式说明符（以及任意数量的附加“y”说明符）最少将年份表示为五位数字。 如果年份的有效数字多于五个，则将它们包括在结果字符串中。 如果年份少于五位数，则用前导零填充该数字以产生五位数。

如果存在额外的“y”说明符，则用所需个数的前导零填充该数字以产生“y”说明符的数目。

下面的示例在一个自定义格式字符串中包含“yyyyy”自定义格式说明符。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#13](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#13)]
[!code-vb[Formatting.DateAndTime.Custom#13](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#13)]

[返回表首](#table)

## <a name="offset-z-format-specifier"></a>偏移量“z”格式说明符

### <a name="the-z-custom-format-specifier"></a><a name="zSpecifier"></a>“z”自定义格式说明符

与 <xref:System.DateTime> 值一起使用时，“z”自定义格式说明符表示本地操作系统的时区相对于协调世界时 (UTC) 的有符号偏移量（以小时为单位）。 它不反映一个实例的 <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> 属性的值。 出于此原因，不建议将“z”格式说明符与 <xref:System.DateTime> 值一起使用。

与 <xref:System.DateTimeOffset> 值一起使用时，此格式说明符表示 <xref:System.DateTimeOffset> 值相对于 UTC 的偏移量（以小时为单位）。

偏移量始终显示为带有前导符号。 加号 (+) 指示早于 UTC 的小时数，减号 (-) 指示晚于 UTC 的小时数。 一位数字的偏移量设置为不带前导零的格式。

如果使用“z”格式说明符而没有其他自定义格式说明符，则将该说明符解释为标准日期和时间格式说明符，并引发 <xref:System.FormatException>。 有关使用单个格式说明符的更多信息，请参阅本文后面的[使用单个自定义格式说明符](#UsingSingleSpecifiers)。

下面的示例在一个自定义格式字符串中包含“z”自定义格式说明符。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#14](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#14)]
[!code-vb[Formatting.DateAndTime.Custom#14](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#14)]

[返回表首](#table)

### <a name="the-zz-custom-format-specifier"></a><a name="zzSpecifier"></a>“zz”自定义格式说明符

与 <xref:System.DateTime> 值一起使用时，“zz”自定义格式说明符表示本地操作系统的时区相对于 UTC 的有符号偏移量（以小时为单位）。 它不反映一个实例的 <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> 属性的值。 出于此原因，不建议将“zz”格式说明符与 <xref:System.DateTime> 值一起使用。

与 <xref:System.DateTimeOffset> 值一起使用时，此格式说明符表示 <xref:System.DateTimeOffset> 值相对于 UTC 的偏移量（以小时为单位）。

偏移量始终显示为带有前导符号。 加号 (+) 指示早于 UTC 的小时数，减号 (-) 指示晚于 UTC 的小时数。 一位数字的偏移量设置为带前导零的格式。

下面的示例在一个自定义格式字符串中包含“zz”自定义格式说明符。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#14](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#14)]
[!code-vb[Formatting.DateAndTime.Custom#14](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#14)]

[返回表首](#table)

### <a name="the-zzz-custom-format-specifier"></a><a name="zzzSpecifier"></a>“zzz”自定义格式说明符

与 <xref:System.DateTime> 值一起使用时，“zzz”自定义格式说明符表示本地操作系统的时区相对于 UTC 的有符号偏移量（以小时和分钟为单位）。 它不反映一个实例的 <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> 属性的值。 出于此原因，不建议将“zzz”格式说明符与 <xref:System.DateTime> 值一起使用。

与 <xref:System.DateTimeOffset> 值一起使用时，此格式说明符表示 <xref:System.DateTimeOffset> 值相对于 UTC 的偏移量（以小时和分钟为单位）。

偏移量始终显示为带有前导符号。 加号 (+) 指示早于 UTC 的小时数，减号 (-) 指示晚于 UTC 的小时数。 一位数字的偏移量设置为带前导零的格式。

下面的示例在一个自定义格式字符串中包含“zzz”自定义格式说明符。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#14](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/Custom1.cs#14)]
[!code-vb[Formatting.DateAndTime.Custom#14](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/Custom1.vb#14)]

[返回表首](#table)

## <a name="date-and-time-separator-specifiers"></a>日期和时间分隔符说明符

### <a name="the--custom-format-specifier"></a><a name="timeSeparator"></a>“:”自定义格式说明符

“:”自定义格式说明符表示时间分隔符，它用于区分小时、分钟和秒。 相应的本地化时间分隔符通过当前或指定区域性的 <xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A?displayProperty=nameWithType> 属性进行检索。

> [!NOTE]
> 若要更改特定日期和时间字符串的时间分隔符，请指定文本字符串分隔符内的分隔字符。 例如，在自定义格式字符串 `hh'_'dd'_'ss` 产生的结果字符串中，始终将“\_”（下划线）用作时间分隔符。 若要更改区域所有日期的时间分隔符，可更改当前区域的 <xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A?displayProperty=nameWithType> 属性，或者实例化 <xref:System.Globalization.DateTimeFormatInfo> 对象，将字符分配到其 <xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A> 属性并调用包含 <xref:System.IFormatProvider> 形参的格式设置方法的重载。

如果使用“:”格式说明符而没有其他自定义格式说明符，则将该说明符解释为标准日期和时间格式说明符，并引发 <xref:System.FormatException>。 有关使用单个格式说明符的更多信息，请参阅本文后面的[使用单个自定义格式说明符](#UsingSingleSpecifiers)。

[返回表首](#table)

### <a name="the--custom-format-specifier"></a><a name="dateSeparator"></a>“/”自定义格式说明符

“/”自定义格式说明符表示日期分隔符，它用于区分年、月和日。 相应的本地化日期分隔符检索自当前或指定区域性的 <xref:System.Globalization.DateTimeFormatInfo.DateSeparator%2A?displayProperty=nameWithType> 属性。

> [!NOTE]
> 若要更改特定日期和时间字符串的日期分隔符，请指定文本字符串分隔符内的分隔字符。 例如，在自定义格式字符串 `mm'/'dd'/'yyyy` 产生的结果字符串中，始终将“/”用作日期分隔符。 若要更改区域所有日期的日期分隔符，可更改当前区域的 <xref:System.Globalization.DateTimeFormatInfo.DateSeparator%2A?displayProperty=nameWithType> 属性，或者实例化 <xref:System.Globalization.DateTimeFormatInfo> 对象，将字符分配到其 <xref:System.Globalization.DateTimeFormatInfo.DateSeparator%2A> 属性并调用包含 <xref:System.IFormatProvider> 形参的格式设置方法的重载。

如果使用“/”格式说明符而没有其他自定义格式说明符，则将该说明符解释为标准日期和时间格式说明符，并引发 <xref:System.FormatException>。 有关使用单个格式说明符的更多信息，请参阅本文后面的[使用单个自定义格式说明符](#UsingSingleSpecifiers)。

[返回表首](#table)

## <a name="character-literals"></a><a name="Literals"></a>字符文本

自定义日期和时间格式字符串中的以下字符是保留的字符，始终解释为格式字符，但 `"`、`'`、`/` 和 `\` 解释为特殊字符。

|     |     |     |     |     |
|-----|-----|-----|-----|-----|
| `F` | `H` | `K` | `M` | `d` |
| `f` | `g` | `h` | `m` | `s` |
| `t` | `y` | `z` | `%` | `:` |
| `/` | `"` | `'` | `\` |     |

所有其他字符始终解释为字符文本，在格式设置操作中，将按原样包含在结果字符串中。  在分析操作中，这些字符必须与输入字符串中的字符完全匹配；比较时区分大小写。

下面的示例在格式字符串中包含用于表示时区的文本字符“PST”（太平洋标准时间）和“PDT”（太平洋夏令时）。 请注意，该字符串将包含在结果字符串中，并且包含本地时区字符串的字符串也可成功完成分析。

[!code-csharp[Formatting.DateAndTime.Custom#20](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/LiteralsEx1.cs#20)]
[!code-vb[Formatting.DateAndTime.Custom#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/LiteralsEx1.vb#20)]

可通过两种方法来指示要将字符解释为文本字符而不是保留字符，以便这些字符可以包含在结果字符串中，或者在输入字符串中成功完成分析：

- 通过转义每个保留字符。 有关详细信息，请参阅[使用转义字符](#escape)。

下面的示例在格式字符串中包含用于表示时区的文本字符“pst”（太平洋标准时间）。 由于“s”和“t”都是自定义格式字符串，因此这两个字符必须经过转义才能解释为字符文本。

[!code-csharp[Formatting.DateAndTime.Custom#21](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/LiteralsEx2.cs#21)]
[!code-vb[Formatting.DateAndTime.Custom#21](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/LiteralsEx2.vb#21)]

- 通过将整个文本字符串括在双引号或单引号中。 下面的示例与前一个示例类似，不过，pst 括在引号中，指示应将整个分隔字符串解释为字符文本。

[!code-csharp[Formatting.DateAndTime.Custom#22](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/LiteralsEx3.cs#22)]
[!code-vb[Formatting.DateAndTime.Custom#22](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/LiteralsEx3.vb#22)]

## <a name="notes"></a>说明

### <a name="using-single-custom-format-specifiers"></a><a name="UsingSingleSpecifiers"></a>使用单个自定义格式说明符

自定义日期和时间格式字符串由两个或更多字符组成。 日期和时间格式设置方法将任何单字符字符串解释标准日期和时间格式字符串。 如果它们无法将该字符识别为有效格式说明符，则会引发 <xref:System.FormatException>。 例如，仅包含说明符“h”的格式字符串解释为标准日期和时间格式字符串。 但是，在此特定情况下将引发异常，原因是不存在“h”标准日期和时间格式说明符。

若要将任何自定义日期和时间格式说明符作为格式字符串中的唯一说明符使用（即若要使用“d”、“f”、“F”、“g”、“h”、“H”、“K”、“m”、“M”、“s”、“t”、“y”、“z”、“:”或“/”自定义格式说明符自身），请在该说明符之前或之后添加一个空格，或在单个自定义日期和时间说明符之前包括一个百分号（“%”）格式说明符。

例如，“`%h"`”将被解释为一个自定义日期和时间格式字符串，用于显示当前日期和时间值表示的小时。 也可以使用“ h”或“h ”格式字符串，尽管它在结果字符串及小时中包含一个空格。 下面的示例阐释了这三个格式字符串。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#16](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/literal1.cs#16)]
[!code-vb[Formatting.DateAndTime.Custom#16](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/literal1.vb#16)]

#### <a name="using-the-escape-character"></a><a name="escape"></a> 使用转义字符

格式字符串中的“d”、“f”、“F”、“g”、“h”、“H”、“K”、“m”、“M”、“s”、“t”、“y”、“z”、“:”或“/”字符被解释为自定义格式说明符而不是文本字符。 若要防止某个字符被解释为格式说明符，可以在该字符前面加上反斜杠 (\\)，即转义字符。 转义字符表示以下字符为应包含在未更改的结果字符串中的字符文本。

若在要结果字符串中包括反斜杠，必须使用另一个反斜杠 (`\\`) 对其转义。

> [!NOTE]
> 一些编译器（如 C++ 和 C# 编译器）也可能会将单个反斜杠字符解释为转义字符。 若要确保在设置格式时正确解释字符串，在 C# 中，可以在字符串之前使用原义字符串文本字符（@ 字符），或者在 C# 和 C++ 中，在每个反斜杠之前另外添加一个反斜杠字符。 下面的 C# 示例阐释了这两种方法。

下面的示例使用转义字符，以防止格式设置操作将“h”和“m”字符解释为格式说明符。

[!code-csharp-interactive[Formatting.DateAndTime.Custom#15](~/samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Custom/cs/escape1.cs#15)]
[!code-vb[Formatting.DateAndTime.Custom#15](~/samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Custom/vb/escape1.vb#15)]

### <a name="control-panel-settings"></a>控制面板设置

对于包含许多自定义日期和时间格式说明符的格式设置操作，控制面板中的“区域和语言选项”设置会影响其产生的结果字符串。 这些设置用于初始化与当前线程区域性关联的 <xref:System.Globalization.DateTimeFormatInfo> 对象，当前线程区域性提供用于控制格式设置的值。 使用不同设置的计算机将生成不同的结果字符串。

此外，如果使用 <xref:System.Globalization.CultureInfo.%23ctor%28System.String%29> 构造函数实例化一个新的 <xref:System.Globalization.CultureInfo> 对象以表示与当前的系统区域性相同的区域性，则通过控制面板中的 **“区域和语言选项”** 建立的任何自定义都将应用到新的 <xref:System.Globalization.CultureInfo> 对象。 可以使用 <xref:System.Globalization.CultureInfo.%23ctor%28System.String%2CSystem.Boolean%29> 构造函数来创建不会反映系统的自定义项的 <xref:System.Globalization.CultureInfo> 对象。

### <a name="datetimeformatinfo-properties"></a>DateTimeFormatInfo 属性

格式化受当前的 <xref:System.Globalization.DateTimeFormatInfo> 对象的属性影响，其由当前线程区域性隐式提供或由调用格式化的方法的 <xref:System.IFormatProvider> 参数显式提供。 对于 <xref:System.IFormatProvider> 参数，应当指定一个表示区域性的 <xref:System.Globalization.CultureInfo> 对象或指定一个 <xref:System.Globalization.DateTimeFormatInfo> 对象。

由许多自定义日期和时间格式说明符产生的结果字符串还取决于当前的 <xref:System.Globalization.DateTimeFormatInfo> 对象的属性。 应用程序通过更改相应的 <xref:System.Globalization.DateTimeFormatInfo> 属性，可以改变由某些自定义日期和时间格式说明符产生的结果。 例如，“ddd”格式说明符将在 <xref:System.Globalization.DateTimeFormatInfo.AbbreviatedDayNames%2A> 字符串数组中找到的缩写的星期名称添加到结果字符串。 类似地，"MMMM"格式说明符将在 <xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A> 字符串数组中找到的月的完整名称添加到结果字符串。

## <a name="see-also"></a>请参阅

- <xref:System.DateTime?displayProperty=nameWithType>
- <xref:System.IFormatProvider?displayProperty=nameWithType>
- [格式设置类型](formatting-types.md)
- [标准日期和时间格式字符串](standard-date-and-time-format-strings.md)
- [示例：.NET Core WinForms 格式设置实用工具 (C#)](/samples/dotnet/samples/windowsforms-formatting-utility-cs)
- [示例：.NET Core WinForms 格式设置实用工具 (Visual Basic)](/samples/dotnet/samples/windowsforms-formatting-utility-vb)
