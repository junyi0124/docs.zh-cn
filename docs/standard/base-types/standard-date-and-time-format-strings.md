---
title: 标准日期和时间格式字符串
description: 介绍如何在 .NET 中使用标准日期和时间格式字符串来定义日期和时间值的文本表示形式。
ms.date: 12/07/2020
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
ms.topic: reference
ms.custom: contperfq2
ms.openlocfilehash: 688aaf7a1814e132f3bffa48394873653bf314e8
ms.sourcegitcommit: 45c7148f2483db2501c1aa696ab6ed2ed8cb71b2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/08/2020
ms.locfileid: "96851127"
---
# <a name="standard-date-and-time-format-strings"></a>标准日期和时间格式字符串

标准日期和时间格式字符串使用单个字符作为格式说明符来定义 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 值的文本表示形式。 任何包含一个以上字符（包括空白）的日期和时间格式字符串都会作为[自定义日期和时间格式字符串](custom-date-and-time-format-strings.md)进行解释。 可通过两种方式使用标准或自定义格式字符串：

- 定义由格式设置操作生成的字符串。

- 定义可通过分析操作转换为 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 值的日期和时间值的文本表示形式。

> [!TIP]
> 你可以下载格式设置实用工具，它属于一种 .NET Windows 窗体应用程序，通过该应用程序，你可将格式字符串应用于数值或日期和时间值并显示结果字符串。 源代码适用于 [C#](/samples/dotnet/samples/windowsforms-formatting-utility-cs) 和 [Visual Basic](/samples/dotnet/samples/windowsforms-formatting-utility-vb)。

[!INCLUDE[C# interactive-note](~/includes/csharp-interactive-with-utc-partial-note.md)]

## <a name="table-of-format-specifiers"></a>格式说明符表

<a name="table"></a>下表描述了标准日期和时间格式说明符。 除非另行说明，否则，特定的标准日期和时间格式说明符将产生相同的字符串表示形式，这与它是与 <xref:System.DateTime> 值还是 <xref:System.DateTimeOffset> 值一起使用无关。 有关使用标准日期和时间格式字符串的更多信息，请参阅[控制面板设置](#control-panel-settings)和[DateTimeFormatInfo 属性](#datetimeformatinfo-properties)。

|格式说明符|描述|示例|
|----------------------|-----------------|--------------|
|"d"|短日期模式。<br /><br /> 有关详细信息，请参阅[短日期（“d”）格式说明符](#ShortDate)。|2009-06-15T13:45:30 -> 6/15/2009 (en-US)<br /><br /> 2009-06-15T13:45:30 -> 15/06/2009 (fr-FR)<br /><br /> 2009-06-15T13:45:30 -> 2009/06/15 (ja-JP)|
|“D”|长日期模式。<br /><br /> 有关详细信息，请参阅[长日期（“D”）格式说明符](#LongDate)。|2009-06-15T13:45:30 -> Monday, June 15, 2009 (en-US)<br /><br /> 2009-06-15T13:45:30 -> 15 июня 2009 г. (ru-RU)<br /><br /> 2009-06-15T13:45:30 -> Montag, 15. Juni 2009 (de-DE)|
|“f”|完整日期/时间模式（短时间）。<br /><br /> 更多信息：[完整日期短时间（“f”）格式说明符](#FullDateShortTime)。|2009-06-15T13:45:30 -> Monday, June 15, 2009 1:45 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> den 15 juni 2009 13:45 (sv-SE)<br /><br /> 2009-06-15T13:45:30 -> Δευτέρα, 15 Ιουνίου 2009 1:45 μμ (el-GR)|
|“F”|完整日期/时间模式（长时间）。<br /><br /> 更多信息：[完整日期长时间（“F”）格式说明符](#FullDateLongTime)。|2009-06-15T13:45:30 -> Monday, June 15, 2009 1:45:30 PM (zh-CN)<br /><br /> 2009-06-15T13:45:30 -> den 15 juni 2009 13:45:30 (sv-SE)<br /><br /> 2009-06-15T13:45:30 -> Δευτέρα, 15 Ιουνίου 2009 1:45:30 μμ (el-GR)|
|“g”|常规日期/时间模式（短时间）。<br /><br /> 更多信息：[常规日期短时间（“g”）格式说明符](#GeneralDateShortTime)。|2009-06-15T13:45:30 -> 6/15/2009 1:45 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> 15/06/2009 13:45 (es-ES)<br /><br /> 2009-06-15T13:45:30 -> 2009/6/15 13:45 (zh-CN)|
|“G”|常规日期/时间模式（长时间）。<br /><br /> 更多信息：[常规日期长时间（“G”）格式说明符](#GeneralDateLongTime)。|2009-06-15T13:45:30 -> 6/15/2009 1:45:30 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> 15/06/2009 13:45:30 (es-ES)<br /><br /> 2009-06-15T13:45:30 -> 2009/6/15 13:45:30 (zh-CN)|
|“M”、“m”|月/日模式。<br /><br /> 更多信息：[月（“M”、“m”）格式说明符](#MonthDay)。|2009-06-15T13:45:30 -> June 15 (en-US)<br /><br /> 2009-06-15T13:45:30 -> 15. juni (da-DK)<br /><br /> 2009-06-15T13:45:30 -> 15 Juni (id-ID)|
|“O”、“o”|往返日期/时间模式。<br /><br /> 更多信息：[往返（“O”、“o”）格式说明符](#Roundtrip)。|<xref:System.DateTime> 值：<br /><br /> 2009-06-15T13:45:30 (DateTimeKind.Local) --> 2009-06-15T13:45:30.0000000-07:00<br /><br /> 2009-06-15T13:45:30 (DateTimeKind.Utc) --> 2009-06-15T13:45:30.0000000Z<br /><br /> 2009-06-15T13:45:30 (DateTimeKind.Unspecified) --> 2009-06-15T13:45:30.0000000<br /><br /> <xref:System.DateTimeOffset> 值：<br /><br /> 2009-06-15T13:45:30-07:00 --> 2009-06-15T13:45:30.0000000-07:00|
|“R”、“r”|RFC1123 模式。<br /><br /> 更多信息：[RFC1123（“R”、“r”）格式说明符](#RFC1123)。|2009-06-15T13:45:30 -> Mon, 15 Jun 2009 20:45:30 GMT|
|“s”|可排序日期/时间模式。<br /><br /> 更多信息：[可排序（“s”）格式说明符](#Sortable)。|2009-06-15T13:45:30 (DateTimeKind.Local) -> 2009-06-15T13:45:30<br /><br /> 2009-06-15T13:45:30 (DateTimeKind.Utc) -> 2009-06-15T13:45:30|
|“t”|短时间模式。<br /><br /> 更多信息：[短时间（“t”）格式说明符](#ShortTime)。|2009-06-15T13:45:30 -> 1:45 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> 13:45 (hr-HR)<br /><br /> 2009-06-15T13:45:30 -> 01:45 م (ar-EG)|
|“T”|长时间模式。<br /><br /> 更多信息：[长时间（“T”）格式说明符](#LongTime)。|2009-06-15T13:45:30 -> 1:45:30 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> 13:45:30 (hr-HR)<br /><br /> 2009-06-15T13:45:30 -> 01:45:30 م (ar-EG)|
|“u”|通用可排序日期/时间模式。<br /><br /> 更多信息：[通用可排序（“u”）格式说明符](#UniversalSortable)。|带有 <xref:System.DateTime> 值：2009-06-15T13:45:30 -> 2009-06-15 13:45:30Z<br /><br /> 带有 <xref:System.DateTimeOffset> 值：2009-06-15T13:45:30 -> 2009-06-15 20:45:30Z|
|“U”|通用完整日期/时间模式。<br /><br /> 更多信息：[通用完整（“U”）格式说明符](#UniversalFull)。|2009-06-15T13:45:30 -> Monday, June 15, 2009 8:45:30 PM (en-US)<br /><br /> 2009-06-15T13:45:30 -> den 15 juni 2009 20:45:30 (sv-SE)<br /><br /> 2009-06-15T13:45:30 -> Δευτέρα, 15 Ιουνίου 2009 8:45:30 μμ (el-GR)|
|“Y”、“y”|年月模式。<br /><br /> 更多信息：[年月（“Y”、“y”）格式说明符](#YearMonth)。|2009-06-15T13:45:30 -> June 2009 (en-US)<br /><br /> 2009-06-15T13:45:30 -> juni 2009 (da-DK)<br /><br /> 2009-06-15T13:45:30 -> Juni 2009 (id-ID)|
|任何其他单个字符|未知说明符。|引发运行时 <xref:System.FormatException>。|

## <a name="how-standard-format-strings-work"></a>标准格式字符串的工作原理

在格式设置操作中，标准格式字符串只是自定义格式字符串的别名。 使用别名引用自定义格式字符串的优点是：尽管别名保持固定不变，自定义格式字符串自身也可以变化。 这很重要，因为日期和时间值的字符串表示形式通常会因区域性而异。 例如，“d”标准格式字符串指示应使用短日期模式显示日期和时间值。 对于固定区域性，此模式为“MM/dd/yyyy”。 对于 fr-FR 区域性，此模式为“dd/MM/yyyy”。 对于 ja-JP 区域性，此模式为“yyyy/MM/dd”。

如果格式设置操作中的标准格式字符串映射到某个特定区域性的自定义格式字符串，则应用程序可定义该特定区域性，并通过以下方式之一使用其自定义格式字符串：

- 可使用默认的（或当前的）区域性。 下面的示例使用当前区域性的短日期格式显示日期。 在此情况下，当前区域性为 en-US。

  [!code-csharp-interactive[System.DateTime.Conceptual.Formatting#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTime.Conceptual.Formatting/cs/StandardFormats1.cs#1)]
  [!code-vb[System.DateTime.Conceptual.Formatting#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTime.Conceptual.Formatting/vb/StandardFormats1.vb#1)]

- 可以传递一个表示区域性的 <xref:System.Globalization.CultureInfo> 对象，该区域性的格式设置将用于具有 <xref:System.IFormatProvider> 参数的方法。 下面的示例使用 pt-BR 区域性的短日期格式显示日期。

  [!code-csharp[System.DateTime.Conceptual.Formatting#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTime.Conceptual.Formatting/cs/StandardFormats1.cs#2)]
  [!code-vb[System.DateTime.Conceptual.Formatting#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTime.Conceptual.Formatting/vb/StandardFormats1.vb#2)]

- 可以传递一个 <xref:System.Globalization.DateTimeFormatInfo> 对象，该对象向具有 <xref:System.IFormatProvider> 参数的方法提供格式设置信息。 下面的示例使用 hr-HR 区域性的 <xref:System.Globalization.DateTimeFormatInfo> 对象中的短日期格式显示日期。

  [!code-csharp[System.DateTime.Conceptual.Formatting#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTime.Conceptual.Formatting/cs/StandardFormats1.cs#3)]
  [!code-vb[System.DateTime.Conceptual.Formatting#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTime.Conceptual.Formatting/vb/StandardFormats1.vb#3)]

> [!NOTE]
> 有关自定义用于格式化日期和时间值的模式或字符串的信息，请参见 <xref:System.Globalization.NumberFormatInfo> 类主题。

某些情况下，标准格式字符串用作固定不变的较长自定义格式字符串的简便缩写。 有四个标准格式字符串属于这一类别：“O”（或“o”）、“R”（或“r”）、“s”和“u”。 这些字符串对应于由固定区域性定义的自定义格式字符串。 通过这些字符串得到的日期和时间值的字符串表示形式在各个区域性中都应是相同的。 下表提供了有关这四个标准日期和时间格式字符串的信息。

|标准格式字符串|由 DateTimeFormatInfo.InvariantInfo 属性定义|自定义格式字符串|
|----------------------------|----------------------------------------------------------|--------------------------|
|“O”或“o”|None|yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'fffffffzz|
|“R”或“r”|<xref:System.Globalization.DateTimeFormatInfo.RFC1123Pattern%2A>|ddd, dd MMM yyyy HH':'mm':'ss 'GMT'|
|“s”|<xref:System.Globalization.DateTimeFormatInfo.SortableDateTimePattern%2A>|yyyy'-'MM'-'dd'T'HH':'mm':'ss|
|“u”|<xref:System.Globalization.DateTimeFormatInfo.UniversalSortableDateTimePattern%2A>|yyyy'-'MM'-'dd HH':'mm':'ss'Z'|

通过 <xref:System.DateTime.ParseExact%2A?displayProperty=nameWithType> 或 <xref:System.DateTimeOffset.ParseExact%2A?displayProperty=nameWithType> 方法，还可以在分析操作中使用标准格式字符串，这些方法需要输入字符串才能完全符合确保分析操作成功的特定模式。 许多标准格式字符串都映射到多个自定义格式字符串，因此，可采用各种格式表示日期和时间值并且分析操作仍然会成功。 通过调用 <xref:System.Globalization.DateTimeFormatInfo.GetAllDateTimePatterns%28System.Char%29?displayProperty=nameWithType> 方法，你可以确定与标准格式字符串对应的自定义格式字符串。 下面的示例显示了映射到“d”（短日期模式）标准格式字符串的自定义格式字符串。

[!code-csharp[Formatting.DateAndTime.Standard#17](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/stdandparsing1.cs#17)]
[!code-vb[Formatting.DateAndTime.Standard#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/stdandparsing1.vb#17)]

以下几节描述了 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 值的标准格式说明符。

## <a name="date-formats"></a>日期格式

此组包括以下格式：

- [短日期（“d”）格式说明符](#the-short-date-d-format-specifier)
- [长日期（“D”）格式说明符](#the-long-date-d-format-specifier)

<a name="ShortDate"></a>

### <a name="the-short-date-d-format-specifier"></a>短日期（“d”）格式说明符

“d”标准格式说明符表示通过特定区域性的 <xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A?displayProperty=nameWithType> 属性定义的自定义日期和时间格式字符串。 例如，由固定区域性的 <xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A> 属性返回的自定义格式字符串为“MM/dd/yyyy”。

下表列出用于控制返回字符串格式的 <xref:System.Globalization.DateTimeFormatInfo> 对象属性。

|Property|描述|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A>|定义结果字符串的总体格式。|
|<xref:System.Globalization.DateTimeFormatInfo.DateSeparator%2A>|定义用于分隔日期中年、月、日部分的字符串。|

下面的示例使用“d”格式说明符来显示日期和时间值。

[!code-csharp[Formatting.DateAndTime.Standard#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#1)]
[!code-vb[Formatting.DateAndTime.Standard#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#1)]

[返回表首](#table)

<a name="LongDate"></a>

### <a name="the-long-date-d-format-specifier"></a>长日期（“D”）格式说明符

“D”标准格式说明符表示由当前的 <xref:System.Globalization.DateTimeFormatInfo.LongDatePattern%2A?displayProperty=nameWithType> 属性定义的自定义日期和时间格式字符串。 例如，用于固定区域性的自定义格式字符串为“dddd, dd MMMM yyyy”。

下表列出了用于控制返回字符串格式的 <xref:System.Globalization.DateTimeFormatInfo> 对象的属性。

|Property|描述|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.LongDatePattern%2A>|定义结果字符串的总体格式。|
|<xref:System.Globalization.DateTimeFormatInfo.DayNames%2A>|定义可在结果字符串中出现的本地化日名称。|
|<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A>|定义可在结果字符串中出现的本地化月份名称。|

下面的示例使用“D”格式说明符来显示日期和时间值。

[!code-csharp[Formatting.DateAndTime.Standard#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#2)]
[!code-vb[Formatting.DateAndTime.Standard#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#2)]

[返回表首](#table)

## <a name="date-and-time-formats"></a>日期和时间格式

此组包括以下格式：

- [完整日期短时间（“f”）格式说明符](#the-full-date-short-time-f-format-specifier)
- [完整日期长时间（“F”）格式说明符](#the-full-date-long-time-f-format-specifier)
- [常规日期短时间（“g”）格式说明符](#the-general-date-short-time-g-format-specifier)
- [常规日期长时间（“G”）格式说明符](#the-general-date-long-time-g-format-specifier)
- [往返（“O”、“o”）格式说明符](#the-round-trip-o-o-format-specifier)
- [RFC1123（“R”、“r”）格式说明符](#the-rfc1123-r-r-format-specifier)
- [可排序（“s”）格式说明符](#the-sortable-s-format-specifier)
- [通用可排序（“u”）格式说明符](#the-universal-sortable-u-format-specifier)
- [通用完整（“U”）格式说明符](#the-universal-full-u-format-specifier)

<a name="FullDateShortTime"></a>

### <a name="the-full-date-short-time-f-format-specifier"></a>完整日期短时间（“f”）格式说明符

“f”标准格式说明符表示长日期（“D”）和短时间（“t”）模式的组合，由空格分隔。

结果字符串受特定 <xref:System.Globalization.DateTimeFormatInfo> 对象的格式信息的影响。 下表列出了 <xref:System.Globalization.DateTimeFormatInfo> 对象属性，这些属性可控制返回字符串的格式。 由某些区域性的 <xref:System.Globalization.DateTimeFormatInfo.LongDatePattern%2A?displayProperty=nameWithType> 和 <xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A?displayProperty=nameWithType> 属性返回的自定义格式说明符可能未利用所有属性。

|Property|描述|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.LongDatePattern%2A>|定义结果字符串中日期部分的格式。|
|<xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A>|定义结果字符串中时间部分的格式。|
|<xref:System.Globalization.DateTimeFormatInfo.DayNames%2A>|定义可在结果字符串中出现的本地化日名称。|
|<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A>|定义可在结果字符串中出现的本地化月份名称。|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|定义分隔时间中小时、分钟和秒钟几个组成部分的字符串。|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|定义以 12 小时时钟制表示午夜至正午之前这段时间的字符串。|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|定义以 12 小时时钟制表示正午至午夜之前这段时间的字符串。|

下面的示例使用“f”格式说明符来显示日期和时间值。

[!code-csharp[Formatting.DateAndTime.Standard#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#3)]
[!code-vb[Formatting.DateAndTime.Standard#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#3)]

[返回表首](#table)

<a name="FullDateLongTime"></a>

### <a name="the-full-date-long-time-f-format-specifier"></a>完整日期长时间（“F”）格式说明符

“F”标准格式说明符表示由当前的 <xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A?displayProperty=nameWithType> 属性定义的自定义日期和时间格式字符串。 例如，用于固定区域性的自定义格式字符串为“dddd, dd MMMM yyyy HH:mm:ss”。

下表列出了 <xref:System.Globalization.DateTimeFormatInfo> 对象属性，这些属性可控制返回字符串的格式。 由某些区域性的 <xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A> 属性返回的自定义格式说明符可能未利用所有属性。

|Property|描述|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A>|定义结果字符串的总体格式。|
|<xref:System.Globalization.DateTimeFormatInfo.DayNames%2A>|定义可在结果字符串中出现的本地化日名称。|
|<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A>|定义可在结果字符串中出现的本地化月份名称。|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|定义分隔时间中小时、分钟和秒钟几个组成部分的字符串。|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|定义以 12 小时时钟制表示午夜至正午之前这段时间的字符串。|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|定义以 12 小时时钟制表示正午至午夜之前这段时间的字符串。|

下面的示例使用“F”格式说明符来显示日期和时间值。

[!code-csharp[Formatting.DateAndTime.Standard#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#4)]
[!code-vb[Formatting.DateAndTime.Standard#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#4)]

[返回表首](#table)

<a name="GeneralDateShortTime"></a>

### <a name="the-general-date-short-time-g-format-specifier"></a>常规日期短时间（“g”）格式说明符

“g”标准格式说明符表示短日期（“d”）和短时间（“t”）模式的组合，由空格分隔。

结果字符串受特定 <xref:System.Globalization.DateTimeFormatInfo> 对象的格式信息的影响。 下表列出了 <xref:System.Globalization.DateTimeFormatInfo> 对象属性，这些属性可控制返回字符串的格式。 由某些区域性的 <xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A?displayProperty=nameWithType> 和 <xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A?displayProperty=nameWithType> 属性返回的自定义格式说明符可能未利用所有属性。

|Property|描述|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A>|定义结果字符串中日期部分的格式。|
|<xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A>|定义结果字符串中时间部分的格式。|
|<xref:System.Globalization.DateTimeFormatInfo.DateSeparator%2A>|定义用于分隔日期中年、月、日部分的字符串。|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|定义分隔时间中小时、分钟和秒钟几个组成部分的字符串。|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|定义以 12 小时时钟制表示午夜至正午之前这段时间的字符串。|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|定义以 12 小时时钟制表示正午至午夜之前这段时间的字符串。|

下面的示例使用“g”格式说明符来显示日期和时间值。

[!code-csharp[Formatting.DateAndTime.Standard#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#5)]
[!code-vb[Formatting.DateAndTime.Standard#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#5)]

[返回表首](#table)

<a name="GeneralDateLongTime"></a>

### <a name="the-general-date-long-time-g-format-specifier"></a>常规日期长时间（“G”）格式说明符

“G”标准格式说明符表示短日期（“d”）和长时间（“T”）模式的组合，由空格分隔。

结果字符串受特定 <xref:System.Globalization.DateTimeFormatInfo> 对象的格式信息的影响。 下表列出了 <xref:System.Globalization.DateTimeFormatInfo> 对象属性，这些属性可控制返回字符串的格式。 由某些区域性的 <xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A?displayProperty=nameWithType> 和 <xref:System.Globalization.DateTimeFormatInfo.LongTimePattern%2A?displayProperty=nameWithType> 属性返回的自定义格式说明符可能未利用所有属性。

|Property|描述|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.ShortDatePattern%2A>|定义结果字符串中日期部分的格式。|
|<xref:System.Globalization.DateTimeFormatInfo.LongTimePattern%2A>|定义结果字符串中时间部分的格式。|
|<xref:System.Globalization.DateTimeFormatInfo.DateSeparator%2A>|定义用于分隔日期中年、月、日部分的字符串。|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|定义分隔时间中小时、分钟和秒钟几个组成部分的字符串。|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|定义以 12 小时时钟制表示午夜至正午之前这段时间的字符串。|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|定义以 12 小时时钟制表示正午至午夜之前这段时间的字符串。|

下面的示例使用“G”格式说明符来显示日期和时间值。

[!code-csharp[Formatting.DateAndTime.Standard#6](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#6)]
[!code-vb[Formatting.DateAndTime.Standard#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#6)]

[返回表首](#table)

<a name="Roundtrip"></a>

### <a name="the-round-trip-o-o-format-specifier"></a>往返（“O”、“o”）格式说明符

“O”或“o”标准格式说明符表示使用保留时区信息的模式的自定义日期和时间格式字符串，并发出符合 ISO8601 的结果字符串。 对于 <xref:System.DateTime> 值，此格式说明符设计用于在文本中将日期和时间值与 <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> 属性一起保留。 如果将 <xref:System.DateTime.Parse%28System.String%2CSystem.IFormatProvider%2CSystem.Globalization.DateTimeStyles%29?displayProperty=nameWithType> 参数设置为 <xref:System.DateTime.ParseExact%2A?displayProperty=nameWithType>，则可通过使用 `styles` 或 <xref:System.Globalization.DateTimeStyles.RoundtripKind?displayProperty=nameWithType> 方法对设置了格式的字符串进行反向分析。

对于 <xref:System.DateTime> 值，“O”或“o”标准格式说明符对应于“yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'fffffffK”自定义格式字符串，对于 <xref:System.DateTimeOffset> 值，“O”或“o”标准格式说明符则对应于“yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'fffffffzzz”自定义格式字符串。 在此字符串中，分隔各个字符（例如连字符、冒号和字母“T”）的单引号标记对指示各个字符是不能更改的文本。 撇号不会出现在输出字符串中。

"O" 或 "o" 标准格式说明符（和 "yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'fffffffK" 自定义格式字符串）利用 ISO 8601 表示时区信息的三种方式，从而暂留 <xref:System.DateTime> 值的 <xref:System.DateTime.Kind%2A> 属性：

- <xref:System.DateTimeKind.Local?displayProperty=nameWithType> 日期和时间值的时区组件是相对于 UTC 的偏移量（例如，+01:00，-07:00）。 所有 <xref:System.DateTimeOffset> 值也以这种格式表示。

- <xref:System.DateTimeKind.Utc?displayProperty=nameWithType> 日期和时间值的时区组件使用“Z”（它代表零偏移量）以表示 UTC。

- <xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType> 日期和时间值没有时区信息。

由于“O”或“o”标准格式说明符遵循国际标准，使用说明符的格式设置或分析操作始终使用固定区域性和公历。

如果字符串采用了这些格式中的某个格式,则可以通过使用“O”或“o”格式说明符分析传递到 `Parse` 和 `TryParse` 的 `ParseExact`、`TryParseExact`、<xref:System.DateTime> 和 <xref:System.DateTimeOffset> 方法的这些字符串。 对于 <xref:System.DateTime> 对象，你调用的分析重载还应当包含带有 `styles` 值的 <xref:System.Globalization.DateTimeStyles.RoundtripKind?displayProperty=nameWithType> 参数。 请注意，如果你使用对应于“O”或“o”格式说明符的自定义格式字符串调用分析方法，则你不会获得与“O”或“o”相同的结果。 这是因为使用自定义格式字符串的分析方法不能分析缺少时区组件的日期和时间值的字符串表示形式，或使用“Z”指示 UTC。

下面的示例使用“o”格式说明符在美国太平洋时区中的系统上显示一系列 <xref:System.DateTime> 值和 <xref:System.DateTimeOffset> 值。

[!code-csharp[Formatting.DateAndTime.Standard#8](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/o1.cs#8)]
[!code-vb[Formatting.DateAndTime.Standard#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/o1.vb#8)]

下面的示例使用“O”格式说明符创建格式字符串，然后通过调用日期和时间 `Parse` 方法还原原始日期和时间值。

[!code-csharp[Formatting.DateandTime.Standard#16](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Roundtrip1.cs#16)]
[!code-vb[Formatting.DateandTime.Standard#16](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/RoundTrip1.vb#16)]

[返回表首](#table)

<a name="RFC1123"></a>

### <a name="the-rfc1123-r-r-format-specifier"></a>RFC1123（“R”、“r”）格式说明符

“R”或“r”标准格式说明符表示由 <xref:System.Globalization.DateTimeFormatInfo.RFC1123Pattern%2A?displayProperty=nameWithType> 属性定义的自定义日期和时间格式字符串。 该模式反映已定义的标准，并且属性是只读的。 因此，无论所使用的区域性或所提供的格式提供程序是什么，它总是相同的。 定义格式字符串为“ddd, dd MMM yyyy HH':'mm':'ss 'GMT'”。 当使用此标准格式说明符时，格式设置或分析操作始终使用固定区域性。

结果字符串受由 <xref:System.Globalization.DateTimeFormatInfo> 属性（该属性表示固定区域性）返回的 <xref:System.Globalization.DateTimeFormatInfo.InvariantInfo%2A?displayProperty=nameWithType> 对象的下列属性的影响。

|Property|描述|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.RFC1123Pattern%2A>|定义结果字符串的格式。|
|<xref:System.Globalization.DateTimeFormatInfo.AbbreviatedDayNames%2A>|定义可在结果字符串中出现的缩写的日期名称。|
|<xref:System.Globalization.DateTimeFormatInfo.AbbreviatedMonthNames%2A>|定义可在结果字符串中出现的缩写的月份名称。|

尽管 RFC 1123 标准将时间表示为协调世界时 (UTC)，格式设置操作也不会修改正在格式化的 <xref:System.DateTime> 对象的值。 因此，执行格式设置操作之前，必须通过调用 <xref:System.DateTime> 方法将 <xref:System.DateTime.ToUniversalTime%2A?displayProperty=nameWithType> 值转换为 UTC。 相反，<xref:System.DateTimeOffset> 值自动执行此转换；即执行格式设置操作之前无需调用 <xref:System.DateTimeOffset.ToUniversalTime%2A?displayProperty=nameWithType> 方法。

下面的示例使用“r”格式说明符在美国太平洋时区中的系统上显示 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 值。

[!code-csharp-interactive[Formatting.DateAndTime.Standard#9](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#9)]
[!code-vb[Formatting.DateAndTime.Standard#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#9)]

[返回表首](#table)

<a name="Sortable"></a>

### <a name="the-sortable-s-format-specifier"></a>可排序（“s”）格式说明符

“s”标准格式说明符表示由 <xref:System.Globalization.DateTimeFormatInfo.SortableDateTimePattern%2A?displayProperty=nameWithType> 属性定义的自定义日期和时间格式字符串。 该模式反映已定义的标准 (ISO 8601)，并且属性是只读的。 因此，无论所使用的区域性或所提供的格式提供程序是什么，它总是相同的。 自定义格式字符串为“yyyy'-'MM'-'dd'T'HH':'mm':'ss”。

使用“s”格式说明符的目的是使生成的结果字符串基于日期和时间值一致按升序或降序顺序进行排序。 因此，尽管“s”标准格式说明符采用一致格式表示日期和时间值，但是格式化操作不会修改正在格式化以反映其 <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> 属性或 <xref:System.DateTimeOffset.Offset%2A?displayProperty=nameWithType> 值的日期和时间对象的值。 例如，通过格式化日期和时间值 2014-11-15T18:32:17+00:00 和 2014-11-15T18:32:17+08:00 生成的结果字符串完全相同。

当使用此标准格式说明符时，格式设置或分析操作始终使用固定区域性。

下面的示例使用“s”格式说明符在美国太平洋时区中的系统上显示 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 值。

[!code-csharp-interactive[Formatting.DateAndTime.Standard#10](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#10)]
[!code-vb[Formatting.DateAndTime.Standard#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#10)]

[返回表首](#table)

<a name="UniversalSortable"></a>

### <a name="the-universal-sortable-u-format-specifier"></a>通用可排序（“u”）格式说明符

“u”标准格式说明符表示由 <xref:System.Globalization.DateTimeFormatInfo.UniversalSortableDateTimePattern%2A?displayProperty=nameWithType> 属性定义的自定义日期和时间格式字符串。 该模式反映已定义的标准，并且属性是只读的。 因此，无论所使用的区域性或所提供的格式提供程序是什么，它总是相同的。 自定义格式字符串为“yyyy'-'MM'-'dd HH':'mm':'ss'Z'”。 当使用此标准格式说明符时，格式设置或分析操作始终使用固定区域性。

尽管结果字符串应将时间表达为协调世界时 (UTC)，但在格式设置操作过程中不转换原始 <xref:System.DateTime> 值。 因此，在对 <xref:System.DateTime> 值进行格式设置之前，必须通过调用 <xref:System.DateTime.ToUniversalTime%2A?displayProperty=nameWithType> 方法将该值转换为 UTC。  相反，<xref:System.DateTimeOffset> 值自动执行此转换；即执行格式设置操作之前无需调用 <xref:System.DateTimeOffset.ToUniversalTime%2A?displayProperty=nameWithType> 方法。

下面的示例使用“u”格式说明符来显示日期和时间值。

[!code-csharp-interactive[Formatting.DateAndTime.Standard#13](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#13)]
[!code-vb[Formatting.DateAndTime.Standard#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#13)]

[返回表首](#table)

<a name="UniversalFull"></a>

### <a name="the-universal-full-u-format-specifier"></a>通用完整（“U”）格式说明符

“U”标准格式说明符表示由特定区域性的 <xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A?displayProperty=nameWithType> 属性定义的自定义日期和时间格式字符串。 此模式与“F”模式相同。 但是，在对 <xref:System.DateTime> 值进行格式设置之前，该值自动转换为 UTC。

下表列出了 <xref:System.Globalization.DateTimeFormatInfo> 对象属性，这些属性可控制返回字符串的格式。 由某些区域性的 <xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A> 属性返回的自定义格式说明符可能未利用所有属性。

|Property|描述|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.FullDateTimePattern%2A>|定义结果字符串的总体格式。|
|<xref:System.Globalization.DateTimeFormatInfo.DayNames%2A>|定义可在结果字符串中出现的本地化日名称。|
|<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A>|定义可在结果字符串中出现的本地化月份名称。|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|定义分隔时间中小时、分钟和秒钟几个组成部分的字符串。|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|定义以 12 小时时钟制表示午夜至正午之前这段时间的字符串。|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|定义以 12 小时时钟制表示正午至午夜之前这段时间的字符串。|

<xref:System.DateTimeOffset> 类型不支持“U”格式说明符。如果使用“U”格式说明符来设置 <xref:System.FormatException> 值的格式，则将引发 <xref:System.DateTimeOffset>。

下面的示例使用“U”格式说明符来显示日期和时间值。

[!code-csharp[Formatting.DateAndTime.Standard#14](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#14)]
[!code-vb[Formatting.DateAndTime.Standard#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#14)]

[返回表首](#table)

## <a name="time-formats"></a>时间格式

此组包括以下格式：

- [短时间（“t”）格式说明符](#the-short-time-t-format-specifier)
- [长时间（“T”）格式说明符](#the-long-time-t-format-specifier)

<a name="ShortTime"></a>

### <a name="the-short-time-t-format-specifier"></a>短时间（“t”）格式说明符

“t”标准格式说明符表示由当前的 <xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A?displayProperty=nameWithType> 属性定义的自定义日期和时间格式字符串。 例如，用于固定区域性的自定义格式字符串为“HH:mm”。

结果字符串受特定 <xref:System.Globalization.DateTimeFormatInfo> 对象的格式信息的影响。 下表列出了 <xref:System.Globalization.DateTimeFormatInfo> 对象属性，这些属性可控制返回字符串的格式。 由某些区域性的 <xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A?displayProperty=nameWithType> 属性返回的自定义格式说明符可能未利用所有属性。

|Property|描述|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.ShortTimePattern%2A>|定义结果字符串中时间部分的格式。|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|定义分隔时间中小时、分钟和秒钟几个组成部分的字符串。|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|定义以 12 小时时钟制表示午夜至正午之前这段时间的字符串。|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|定义以 12 小时时钟制表示正午至午夜之前这段时间的字符串。|

下面的示例使用“t”格式说明符来显示日期和时间值。

[!code-csharp[Formatting.DateAndTime.Standard#11](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#11)]
[!code-vb[Formatting.DateAndTime.Standard#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#11)]

[返回表首](#table)

<a name="LongTime"></a>

### <a name="the-long-time-t-format-specifier"></a>长时间（“T”）格式说明符

“T”标准格式说明符表示由特定区域性的 <xref:System.Globalization.DateTimeFormatInfo.LongTimePattern%2A?displayProperty=nameWithType> 属性定义的自定义日期和时间格式字符串。 例如，用于固定区域性的自定义格式字符串为“HH:mm:ss”。

下表列出了 <xref:System.Globalization.DateTimeFormatInfo> 对象属性，这些属性可控制返回字符串的格式。 由某些区域性的 <xref:System.Globalization.DateTimeFormatInfo.LongTimePattern%2A?displayProperty=nameWithType> 属性返回的自定义格式说明符可能未利用所有属性。

|Property|描述|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.LongTimePattern%2A>|定义结果字符串中时间部分的格式。|
|<xref:System.Globalization.DateTimeFormatInfo.TimeSeparator%2A>|定义分隔时间中小时、分钟和秒钟几个组成部分的字符串。|
|<xref:System.Globalization.DateTimeFormatInfo.AMDesignator%2A>|定义以 12 小时时钟制表示午夜至正午之前这段时间的字符串。|
|<xref:System.Globalization.DateTimeFormatInfo.PMDesignator%2A>|定义以 12 小时时钟制表示正午至午夜之前这段时间的字符串。|

下面的示例使用“T”格式说明符来显示日期和时间值。

[!code-csharp[Formatting.DateAndTime.Standard#12](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#12)]
[!code-vb[Formatting.DateAndTime.Standard#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#12)]

[返回表首](#table)

## <a name="partial-date-formats"></a>部分日期格式

此组包括以下格式：

- [月（“M”、“m”）格式说明符](#the-month-m-m-format-specifier)
- [年月（“Y”、“y”）格式说明符](#the-year-month-y-y-format-specifier)

<a name="MonthDay"></a>

### <a name="the-month-m-m-format-specifier"></a>月（“M”、“m”）格式说明符

“M”或“m”标准格式说明符表示由当前的 <xref:System.Globalization.DateTimeFormatInfo.MonthDayPattern%2A?displayProperty=nameWithType> 属性定义的自定义日期和时间格式字符串。 例如，用于固定区域性的自定义格式字符串为“MMMM dd”。

下表列出用于控制返回字符串格式的 <xref:System.Globalization.DateTimeFormatInfo> 对象属性。

|Property|描述|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.MonthDayPattern%2A>|定义结果字符串的总体格式。|
|<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A>|定义可在结果字符串中出现的本地化月份名称。|

下面的示例使用“m”格式说明符来显示日期和时间值。

[!code-csharp[Formatting.DateAndTime.Standard#7](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#7)]
[!code-vb[Formatting.DateAndTime.Standard#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#7)]

[返回表首](#table)

<a name="YearMonth"></a>

### <a name="the-year-month-y-y-format-specifier"></a>年月（“Y”、“y”）格式说明符

“Y”或“y”标准格式说明符表示由指定区域性的 <xref:System.Globalization.DateTimeFormatInfo.YearMonthPattern%2A?displayProperty=nameWithType> 属性定义的自定义日期和时间格式字符串。 例如，用于固定区域性的自定义格式字符串为“yyyy MMMM”。

下表列出用于控制返回字符串格式的 <xref:System.Globalization.DateTimeFormatInfo> 对象属性。

|Property|描述|
|--------------|-----------------|
|<xref:System.Globalization.DateTimeFormatInfo.YearMonthPattern%2A>|定义结果字符串的总体格式。|
|<xref:System.Globalization.DateTimeFormatInfo.MonthNames%2A>|定义可在结果字符串中出现的本地化月份名称。|

下面的示例使用“y”格式说明符来显示日期和时间值。

[!code-csharp[Formatting.DateAndTime.Standard#15](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.DateAndTime.Standard/cs/Standard1.cs#15)]
[!code-vb[Formatting.DateAndTime.Standard#15](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.DateAndTime.Standard/vb/Standard1.vb#15)]

[返回表首](#table)

<a name="Notes"></a>

## <a name="control-panel-settings"></a>控制面板设置

Windows 控制面板中“区域和语言选项”项的设置会影响由格式化操作产生的结果字符串。 这些设置用于初始化与当前线程区域性关联的 <xref:System.Globalization.DateTimeFormatInfo> 对象，当前线程区域性提供用于控制格式设置的值。 使用不同设置的计算机将生成不同的结果字符串。

此外，如果使用 <xref:System.Globalization.CultureInfo.%23ctor%28System.String%29> 构造函数实例化一个新的 <xref:System.Globalization.CultureInfo> 对象以表示与当前的系统区域性相同的区域性，则通过控制面板中的 **“区域和语言选项”** 建立的任何自定义都将应用到新的 <xref:System.Globalization.CultureInfo> 对象。 可以使用 <xref:System.Globalization.CultureInfo.%23ctor%28System.String%2CSystem.Boolean%29> 构造函数来创建不会反映系统的自定义项的 <xref:System.Globalization.CultureInfo> 对象。

## <a name="datetimeformatinfo-properties"></a>DateTimeFormatInfo 属性

格式化受当前的 <xref:System.Globalization.DateTimeFormatInfo> 对象的属性影响，其由当前线程区域性隐式提供或由调用格式化的方法的 <xref:System.IFormatProvider> 参数显式提供。 对于 <xref:System.IFormatProvider> 参数，应用程序应指定一个表示区域性的 <xref:System.Globalization.CultureInfo> 对象或表示特定区域性的日期和时间格式设置约定的 <xref:System.Globalization.DateTimeFormatInfo> 对象。 许多标准日期和时间格式说明符是由当前的 <xref:System.Globalization.DateTimeFormatInfo> 对象的属性定义的格式设置模式的别名。 应用程序通过更改相应 <xref:System.Globalization.DateTimeFormatInfo> 属性的相应日期和时间格式模式，可以更改由某些标准日期和时间格式说明符产生的结果。

## <a name="see-also"></a>请参阅

- <xref:System.DateTime?displayProperty=nameWithType>
- <xref:System.DateTimeOffset?displayProperty=nameWithType>
- [格式设置类型](formatting-types.md)
- [自定义日期和时间格式字符串](custom-date-and-time-format-strings.md)
- [示例：.NET Core WinForms 格式设置实用工具 (C#)](/samples/dotnet/samples/windowsforms-formatting-utility-cs)
- [示例：.NET Core WinForms 格式设置实用工具 (Visual Basic)](/samples/dotnet/samples/windowsforms-formatting-utility-vb)
