---
title: 自定义 TimeSpan 格式字符串
description: 了解 .NET 中的自定义 TimeSpan 格式字符串。 自定义格式字符串包含一个或多个自定义 TimeSpan 格式说明符以及任何数量的文本字符。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- format specifiers, custom time interval
- format strings
- formatting [.NET], time interval
- custom time interval format strings
- formatting [.NET], time
- custom TimeSpan format strings
ms.assetid: a63ebf55-7269-416b-b4f5-286f6c03bf0e
ms.openlocfilehash: e8447e580ac85e1ebbc33f8c8334b7c5cfeb334b
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94824358"
---
# <a name="custom-timespan-format-strings"></a>自定义 TimeSpan 格式字符串

<xref:System.TimeSpan> 格式字符串定义格式设置操作生成的 <xref:System.TimeSpan> 值的字符串表示形式。 自定义格式字符串包含一个或多个自定义 <xref:System.TimeSpan> 格式说明符，以及任意数量的文本字符。 任何不是[标准 TimeSpan 格式字符串](standard-timespan-format-strings.md)的字符串都会解释为自定义 <xref:System.TimeSpan> 格式字符串。

> [!IMPORTANT]
> 自定义 <xref:System.TimeSpan> 格式说明符不包含占位符分隔符符号，如分隔天与小时、小时与分钟或秒与秒若干分之一的符号。 相反，这些符号必须以字符串形式包含在自定义格式字符串中。 例如，`"dd\.hh\:mm"` 将句点 (.) 定义为天与小时之间的分隔符，将冒号 (:) 定义为小时与分钟之间的分隔符。
>
> 自定义 <xref:System.TimeSpan> 格式说明符还不包括正负符号，无法区分正负时间间隔。 若要包含正负符号，必须使用条件逻辑构造格式字符串。 [其他字符](#other-characters)部分包括一个示例。

<xref:System.TimeSpan> 值的字符串表示形式通过调用 <xref:System.TimeSpan.ToString%2A?displayProperty=nameWithType> 方法重载由支持复合格式的方法（如 <xref:System.String.Format%2A?displayProperty=nameWithType>）生成。 有关更多信息，请参见[格式设置类型](formatting-types.md)和[复合格式设置](composite-formatting.md)。 下面的示例展示了自定义格式字符串在格式设置操作中的用法。

[!code-csharp[Conceptual.TimeSpan.Custom#1](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customformatexample1.cs#1)]
[!code-vb[Conceptual.TimeSpan.Custom#1](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customformatexample1.vb#1)]

自定义 <xref:System.TimeSpan> 格式字符串也被 <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> 和 <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType> 方法用于定义分析操作所需的输入字符串格式。 （分析将值的字符串表示形式转换成该值。）以下示例演示了标准格式字符串在分析操作中的用法。

[!code-csharp[Conceptual.TimeSpan.Custom#2](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customparseexample1.cs#2)]
[!code-vb[Conceptual.TimeSpan.Custom#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customparseexample1.vb#2)]

<a name="table"></a> 下表列出了自定义日期和时间格式说明符。

| 格式说明符 | 描述 | 示例 |
|----------------------|-----------------|-------------|
|“d”，“%d”|时间间隔中的整天数。<br /><br /> 更多信息：[“d”自定义格式说明符](#dSpecifier)。|`new TimeSpan(6, 14, 32, 17, 685):`<br /><br /> `%d` --> "6"<br /><br /> `d\.hh\:mm` --> "6.14:32"|
|"dd"-"dddddddd"|时间间隔中的整天数，根据需要使用前导零填充。<br /><br /> 更多信息：[“dd”-“dddddddd”自定义格式说明符](#ddSpecifier)。|`new TimeSpan(6, 14, 32, 17, 685):`<br /><br /> `ddd` --> "006"<br /><br /> `dd\.hh\:mm` --> "06.14:32"|
|“h”，“%h”|时间间隔中不计为天数一部分的整小时数。 一位数小时数没有前导零。<br /><br /> 更多信息：[“h”自定义格式说明符](#hSpecifier)。|`new TimeSpan(6, 14, 32, 17, 685):`<br /><br /> `%h` --> "14"<br /><br /> `hh\:mm` --> "14:32"|
|“hh”|时间间隔中不计为天数一部分的整小时数。 一位数小时具有前导零。<br /><br /> 更多信息：[“hh”自定义格式说明符](#hhSpecifier)。|`new TimeSpan(6, 14, 32, 17, 685):`<br /><br /> `hh` --> "14"<br /><br /> `new TimeSpan(6, 8, 32, 17, 685):`<br /><br /> `hh` --> 08|
|“m”，“%m”|时间间隔中不包含在小时或天数中的整分钟数。 一位数分钟数没有前导零。<br /><br /> 更多信息：[“m”自定义格式说明符](#mSpecifier)。|`new TimeSpan(6, 14, 8, 17, 685):`<br /><br /> `%m` --> "8"<br /><br /> `h\:m` --> "14:8"|
|“mm”|时间间隔中不包含在小时或天数中的整分钟数。 一位数分钟具有前导零。<br /><br /> 更多信息：[“mm”自定义格式说明符](#mmSpecifier)。|`new TimeSpan(6, 14, 8, 17, 685):`<br /><br /> `mm` --> "08"<br /><br /> `new TimeSpan(6, 8, 5, 17, 685):`<br /><br /> `d\.hh\:mm\:ss` --> 6.08:05:17|
|“s”，“%s”|时间间隔中不包含在小时、天数或分钟中的整秒数。 一位数秒数没有前导零。<br /><br /> 更多信息：[“s”自定义格式说明符](#sSpecifier)。|`TimeSpan.FromSeconds(12.965)`：<br /><br /> `%s` --> 12<br /><br /> `s\.fff` --> 12.965|
|“ss”|时间间隔中不包含在小时、天数或分钟中的整秒数。  一位数秒具有前导零。<br /><br /> 更多信息：[“ss”自定义格式说明符](#ssSpecifier)。|`TimeSpan.FromSeconds(6.965)`：<br /><br /> `ss` --> 06<br /><br /> `ss\.fff` --> 06.965|
|“f”，“%f”|时间间隔中的十分之几秒。<br /><br /> 更多信息：[“f”自定义格式说明符](#fSpecifier)。|`TimeSpan.FromSeconds(6.895)`：<br /><br /> `f` --> 8<br /><br /> `ss\.f` --> 06.8|
|“ff”|时间间隔中的百分之几秒。<br /><br /> 更多信息：[“ff”自定义格式说明符](#ffSpecifier)。|`TimeSpan.FromSeconds(6.895)`：<br /><br /> `ff` --> 89<br /><br /> `ss\.ff` --> 06.89|
|“fff”|时间间隔中的毫秒。<br /><br /> 更多信息：[“fff”自定义格式说明符](#f3Specifier)。|`TimeSpan.FromSeconds(6.895)`：<br /><br /> `fff` --> 895<br /><br /> `ss\.fff` --> 06.895|
|“ffff”|时间间隔中的万分之几秒。<br /><br /> 更多信息：[“ffff”自定义格式说明符](#f4Specifier)。|`TimeSpan.Parse("0:0:6.8954321")`：<br /><br /> `ffff` --> 8954<br /><br /> `ss\.ffff` --> 06.8954|
|“fffff”|时间间隔中的十万分之几秒。<br /><br /> 更多信息：[“fffff”自定义格式说明符](#f5Specifier)。|`TimeSpan.Parse("0:0:6.8954321")`：<br /><br /> `fffff` --> 89543<br /><br /> `ss\.fffff` --> 06.89543|
|“ffffff”|时间间隔中的百万分之几秒。<br /><br /> 更多信息：[“ffffff”自定义格式说明符](#f6Specifier)。|`TimeSpan.Parse("0:0:6.8954321")`：<br /><br /> `ffffff` --> 895432<br /><br /> `ss\.ffffff` --> 06.895432|
|“fffffff”|时间间隔中的千万分之几秒（或小数时钟周期）。<br /><br /> 更多信息：[“fffffff”自定义格式说明符](#f7Specifier)。|`TimeSpan.Parse("0:0:6.8954321")`：<br /><br /> `fffffff` --> 8954321<br /><br /> `ss\.fffffff` --> 06.8954321|
|“F”，“%F”|时间间隔中的十分之几秒。 如果该数字为零，则不显示任何内容。<br /><br /> 更多信息：[“F”自定义格式说明符](#F_Specifier)。|`TimeSpan.Parse("00:00:06.32")`：<br /><br /> `%F`：3<br /><br /> `TimeSpan.Parse("0:0:3.091")`：<br /><br /> `ss\.F`：03.|
|“FF”|时间间隔中的百分之几秒。 不包含任何小数尾随零或两个零位。<br /><br /> 更多信息：[“FF”自定义格式说明符](#FF_Specifier)。|`TimeSpan.Parse("00:00:06.329")`：<br /><br /> `FF`：32<br /><br /> `TimeSpan.Parse("0:0:3.101")`：<br /><br /> `ss\.FF`：03.1|
|“FFF”|时间间隔中的毫秒。 不包含任何小数尾随零。<br /><br /> 更多信息：|`TimeSpan.Parse("00:00:06.3291")`：<br /><br /> `FFF`：329<br /><br /> `TimeSpan.Parse("0:0:3.1009")`：<br /><br /> `ss\.FFF`：03.1|
|“FFFF”|时间间隔中的万分之几秒。 不包含任何小数尾随零。<br /><br /> 更多信息：[“FFFF”自定义格式说明符](#F4_Specifier)。|`TimeSpan.Parse("00:00:06.32917")`：<br /><br /> `FFFFF`：3291<br /><br /> `TimeSpan.Parse("0:0:3.10009")`：<br /><br /> `ss\.FFFF`：03.1|
|“FFFFF”|时间间隔中的十万分之几秒。 不包含任何小数尾随零。<br /><br /> 更多信息：[“FFFFF”自定义格式说明符](#F5_Specifier)。|`TimeSpan.Parse("00:00:06.329179")`：<br /><br /> `FFFFF`：32917<br /><br /> `TimeSpan.Parse("0:0:3.100009")`：<br /><br /> `ss\.FFFFF`：03.1|
|“FFFFFF”|时间间隔中的百万分之几秒。 不显示任何小数尾随零。<br /><br /> 更多信息：[“FFFFFF”自定义格式说明符](#F6_Specifier)。|`TimeSpan.Parse("00:00:06.3291791")`：<br /><br /> `FFFFFF`：329179<br /><br /> `TimeSpan.Parse("0:0:3.1000009")`：<br /><br /> `ss\.FFFFFF`：03.1|
|“FFFFFFF”|时间间隔中的千万分之几秒。 不显示任何小数尾随零或七个零。<br /><br /> 更多信息：[“FFFFFFF”自定义格式说明符](#F7_Specifier)。|`TimeSpan.Parse("00:00:06.3291791")`：<br /><br /> `FFFFFF`：3291791<br /><br /> `TimeSpan.Parse("0:0:3.1900000")`：<br /><br /> `ss\.FFFFFF`：03.19|
|'string'|文本字符串分隔符。<br /><br /> 更多信息：[其他字符](#other-characters)。|`new TimeSpan(14, 32, 17):`<br /><br /> `hh':'mm':'ss` --> "14:32:17"|
|&#92;|转义字符。<br /><br /> 更多信息：[其他字符](#other-characters)。|`new TimeSpan(14, 32, 17):`<br /><br /> `hh\:mm\:ss` --> "14:32:17"|
|任何其他字符|任何其他未转义字符会解释为自定义格式说明符。<br /><br /> 详细信息：[其他字符](#other-characters)。|`new TimeSpan(14, 32, 17):`<br /><br /> `hh\:mm\:ss` --> "14:32:17"|

## <a name="the-d-custom-format-specifier"></a><a name="dSpecifier"></a>“d”自定义格式说明符

"d" 自定义格式说明符输出 <xref:System.TimeSpan.Days%2A?displayProperty=nameWithType> 属性的值，表示时间间隔中的整天数。 它在 <xref:System.TimeSpan> 值中输出整天数（即使值有多位，也不例外）。 如果 <xref:System.TimeSpan.Days%2A?displayProperty=nameWithType> 属性的值为零，说明符输出“0”。

如果“d”自定义格式说明符单独使用，则指定“%d”，以便它不会错误地解释为标准格式字符串。 下面的示例进行了这方面的演示。

[!code-csharp[Conceptual.TimeSpan.Custom#3](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#3)]
[!code-vb[Conceptual.TimeSpan.Custom#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#3)]

下面的示例演示如何使用“d”自定义格式说明符。

[!code-csharp[Conceptual.TimeSpan.Custom#4](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#4)]
[!code-vb[Conceptual.TimeSpan.Custom#4](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#4)]

[返回表首](#table)

## <a name="the-dd-dddddddd-custom-format-specifiers"></a><a name="ddSpecifier"></a>“dd”-“dddddddd”自定义格式说明符

"dd"、"ddd"、"dddd"、"ddddd"、"dddddd"、"ddddddd" 和 "dddddddd" 自定义格式说明符输出 <xref:System.TimeSpan.Days%2A?displayProperty=nameWithType> 属性的值，表示时间间隔中的整天数。

输出字符串包含格式说明符中的“d”字符数指定的最小位数，会根据需要使用前导零填充。 如果天数中的位数超过格式说明符中的“d”字符数，则完整天数会在结果字符串中输出。

下面的示例使用这些格式说明符，显示两个 <xref:System.TimeSpan> 值的字符串表示形式。 第一个时间间隔的天数部分的值为 0；第二个的天数部分的值为 365。

[!code-csharp[Conceptual.TimeSpan.Custom#5](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#5)]
[!code-vb[Conceptual.TimeSpan.Custom#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#5)]

[返回表首](#table)

## <a name="the-h-custom-format-specifier"></a><a name="hSpecifier"></a>“h”自定义格式说明符

“h”自定义格式说明符输出 <xref:System.TimeSpan.Hours%2A?displayProperty=nameWithType> 属性的值，表示时间间隔中不计入天数的整小时数。 如果 <xref:System.TimeSpan.Hours%2A?displayProperty=nameWithType> 属性的值介于 0 和 9 之间，它返回一位数字符串值；如果 <xref:System.TimeSpan.Hours%2A?displayProperty=nameWithType> 属性的值介于 10 和 23 之间，它返回两位数字符串值。

如果“h”自定义格式说明符单独使用，则指定“%h”，以便它不会错误地解释为标准格式字符串。 下面的示例进行了这方面的演示。

[!code-csharp[Conceptual.TimeSpan.Custom#6](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#6)]
[!code-vb[Conceptual.TimeSpan.Custom#6](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#6)]

通常在分析操作中，只包含单个数字的输入字符串会解释为天数。 可以改为使用“%h”自定义格式说明符将数字字符串解释为小时数。 下面的示例进行了这方面的演示。

[!code-csharp[Conceptual.TimeSpan.Custom#8](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#8)]
[!code-vb[Conceptual.TimeSpan.Custom#8](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#8)]

下面的示例演示如何使用“h”自定义格式说明符。

[!code-csharp[Conceptual.TimeSpan.Custom#7](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#7)]
[!code-vb[Conceptual.TimeSpan.Custom#7](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#7)]

[返回表首](#table)

## <a name="the-hh-custom-format-specifier"></a><a name="hhSpecifier"></a>“hh”自定义格式说明符

“hh”自定义格式说明符输出 <xref:System.TimeSpan.Hours%2A?displayProperty=nameWithType> 属性的值，表示时间间隔中不计入天数的整小时数。 对于 0 到 9 的值，输出字符串包含一个前导零。

通常在分析操作中，只包含单个数字的输入字符串会解释为天数。 可以改为使用“hh”自定义格式说明符将数字字符串解释为小时数。 下面的示例进行了这方面的演示。

[!code-csharp[Conceptual.TimeSpan.Custom#9](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#9)]
[!code-vb[Conceptual.TimeSpan.Custom#9](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#9)]

下面的示例演示如何使用“hh”自定义格式说明符。

[!code-csharp[Conceptual.TimeSpan.Custom#10](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#10)]
[!code-vb[Conceptual.TimeSpan.Custom#10](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#10)]

[返回表首](#table)

## <a name="the-m-custom-format-specifier"></a><a name="mSpecifier"></a>“m”自定义格式说明符

“m”自定义格式说明符输出 <xref:System.TimeSpan.Minutes%2A?displayProperty=nameWithType> 属性的值，表示时间间隔中不计入天数的整分钟数。 如果 <xref:System.TimeSpan.Minutes%2A?displayProperty=nameWithType> 属性的值介于 0 和 9 之间，它返回一位数字符串值；如果 <xref:System.TimeSpan.Minutes%2A?displayProperty=nameWithType> 属性的值介于 10 和 59 之间，它返回两位数字符串值。

如果“m”自定义格式说明符单独使用，则指定“%m”，以便它不会错误地解释为标准格式字符串。 下面的示例进行了这方面的演示。

[!code-csharp[Conceptual.TimeSpan.Custom#6](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#6)]
[!code-vb[Conceptual.TimeSpan.Custom#6](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#6)]

通常在分析操作中，只包含单个数字的输入字符串会解释为天数。 可以改为使用“%m”自定义格式说明符将数字字符串解释为分钟数。 下面的示例进行了这方面的演示。

[!code-csharp[Conceptual.TimeSpan.Custom#11](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#11)]
[!code-vb[Conceptual.TimeSpan.Custom#11](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#11)]

下面的示例演示如何使用“m”自定义格式说明符。

[!code-csharp[Conceptual.TimeSpan.Custom#12](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#12)]
[!code-vb[Conceptual.TimeSpan.Custom#12](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#12)]

[返回表首](#table)

## <a name="the-mm-custom-format-specifier"></a><a name="mmSpecifier"></a>“mm”自定义格式说明符

“mm”自定义格式说明符输出 <xref:System.TimeSpan.Minutes%2A?displayProperty=nameWithType> 属性的值，表示时间间隔中不计入小时数或天数的整分钟数。 对于 0 到 9 的值，输出字符串包含一个前导零。

通常在分析操作中，只包含单个数字的输入字符串会解释为天数。 可以改为使用“mm”自定义格式说明符将数字字符串解释为分钟数。 下面的示例进行了这方面的演示。

[!code-csharp[Conceptual.TimeSpan.Custom#13](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#13)]
[!code-vb[Conceptual.TimeSpan.Custom#13](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#13)]

下面的示例演示如何使用“mm”自定义格式说明符。

[!code-csharp[Conceptual.TimeSpan.Custom#14](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#14)]
[!code-vb[Conceptual.TimeSpan.Custom#14](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#14)]

[返回表首](#table)

## <a name="the-s-custom-format-specifier"></a><a name="sSpecifier"></a>“s”自定义格式说明符

“s”自定义格式说明符输出 <xref:System.TimeSpan.Seconds%2A?displayProperty=nameWithType> 属性的值，表示时间间隔中不计入小时数、天数或分钟数的整秒数。 如果 <xref:System.TimeSpan.Seconds%2A?displayProperty=nameWithType> 属性的值介于 0 和 9 之间，它返回一位数字符串值；如果 <xref:System.TimeSpan.Seconds%2A?displayProperty=nameWithType> 属性的值介于 10 和 59 之间，它返回两位数字符串值。

如果“s”自定义格式说明符单独使用，则指定“%s”，以便它不会错误地解释为标准格式字符串。 下面的示例进行了这方面的演示。

[!code-csharp[Conceptual.TimeSpan.Custom#15](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#15)]
[!code-vb[Conceptual.TimeSpan.Custom#15](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#15)]

通常在分析操作中，只包含单个数字的输入字符串会解释为天数。 可以改为使用“%s”自定义格式说明符将数字字符串解释为秒数。 下面的示例进行了这方面的演示。

[!code-csharp[Conceptual.TimeSpan.Custom#17](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#17)]
[!code-vb[Conceptual.TimeSpan.Custom#17](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#17)]

下面的示例演示如何使用“s”自定义格式说明符。

[!code-csharp[Conceptual.TimeSpan.Custom#16](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#16)]
[!code-vb[Conceptual.TimeSpan.Custom#16](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#16)]

[返回表首](#table)

## <a name="the-ss-custom-format-specifier"></a><a name="ssSpecifier"></a>“ss”自定义格式说明符

“ss”自定义格式说明符输出 <xref:System.TimeSpan.Seconds%2A?displayProperty=nameWithType> 属性的值，表示时间间隔中不计入小时数、天数或分钟数的整秒数。 对于 0 到 9 的值，输出字符串包含一个前导零。

通常在分析操作中，只包含单个数字的输入字符串会解释为天数。 可以改为使用“ss”自定义格式说明符将数字字符串解释为秒数。 下面的示例进行了这方面的演示。

[!code-csharp[Conceptual.TimeSpan.Custom#18](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#18)]
[!code-vb[Conceptual.TimeSpan.Custom#18](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#18)]

下面的示例演示如何使用“ss”自定义格式说明符。

[!code-csharp[Conceptual.TimeSpan.Custom#19](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/customexamples1.cs#19)]
[!code-vb[Conceptual.TimeSpan.Custom#19](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/customexamples1.vb#19)]

[返回表首](#table)

## <a name="the-f-custom-format-specifier"></a><a name="fSpecifier"></a>“f”自定义格式说明符

“f”自定义格式说明符输出时间间隔中的十分之几秒。 在格式设置操作中，会截断其余任何小数位数。 在调用 <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> 或 <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType> 方法的分析操作中，输入字符串必须只有一个小数位。

如果“f”自定义格式说明符单独使用，则指定“%f”，以便它不会错误地解释为标准格式字符串。

下面的示例使用 "f" 自定义格式说明符在 <xref:System.TimeSpan> 值中显示秒的十分之几。 “f”首先用作唯一的格式说明符，然后在自定义格式字符串中与“s”说明符结合使用。

[!code-csharp[Conceptual.TimeSpan.Custom#20](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/fspecifiers1.cs#20)]
[!code-vb[Conceptual.TimeSpan.Custom#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/fspecifiers1.vb#20)]

[返回表首](#table)

## <a name="the-ff-custom-format-specifier"></a><a name="ffSpecifier"></a>“ff”自定义格式说明符

“ff”自定义格式说明符输出时间间隔中的百分之几秒。 在格式设置操作中，会截断其余任何小数位数。 在调用 <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> 或 <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType> 方法的分析操作中，输入字符串必须只有两个小数位。

下面的示例使用 "ff" 自定义格式说明符在 <xref:System.TimeSpan> 值中显示秒的百分之几。 “ff”首先用作唯一的格式说明符，然后在自定义格式字符串中与“s”说明符结合使用。

[!code-csharp[Conceptual.TimeSpan.Custom#20](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/fspecifiers1.cs#20)]
[!code-vb[Conceptual.TimeSpan.Custom#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/fspecifiers1.vb#20)]

[返回表首](#table)

## <a name="the-fff-custom-format-specifier"></a><a name="f3Specifier"></a>“fff”自定义格式说明符

“fff”自定义格式说明符（包含三个“f”字符）输出时间间隔中的毫秒。 在格式设置操作中，会截断其余任何小数位数。 在调用 <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> 或 <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType> 方法的分析操作中，输入字符串必须只有三个小数位。

下面的示例使用 "fff" 自定义格式说明符在 <xref:System.TimeSpan> 值中显示毫秒。 “fff”首先用作唯一的格式说明符，然后在自定义格式字符串中与“s”说明符结合使用。

[!code-csharp[Conceptual.TimeSpan.Custom#20](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/fspecifiers1.cs#20)]
[!code-vb[Conceptual.TimeSpan.Custom#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/fspecifiers1.vb#20)]

[返回表首](#table)

## <a name="the-ffff-custom-format-specifier"></a><a name="f4Specifier"></a>“ffff”自定义格式说明符

“ffff”自定义格式说明符（包含四个“f”字符）输出时间间隔中的万分之几秒。 在格式设置操作中，会截断其余任何小数位数。 在调用 <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> 或 <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType> 方法的分析操作中，输入字符串必须只有四个小数位。

下面的示例使用 "ffff" 自定义格式说明符在 <xref:System.TimeSpan> 值中显示秒的万分之几。 “ffff”首先用作唯一的格式说明符，然后在自定义格式字符串中与“s”说明符结合使用。

[!code-csharp[Conceptual.TimeSpan.Custom#20](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/fspecifiers1.cs#20)]
[!code-vb[Conceptual.TimeSpan.Custom#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/fspecifiers1.vb#20)]

[返回表首](#table)

## <a name="the-fffff-custom-format-specifier"></a><a name="f5Specifier"></a>“fffff”自定义格式说明符

“fffff”自定义格式说明符（包含五个“f”字符）输出时间间隔中的十万分之几秒。 在格式设置操作中，会截断其余任何小数位数。 在调用 <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> 或 <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType> 方法的分析操作中，输入字符串必须只有五个小数位。

下面的示例使用 "fffff" 自定义格式说明符在 <xref:System.TimeSpan> 值中显示秒的十万分之几。 “fffff”首先用作唯一的格式说明符，然后在自定义格式字符串中与“s”说明符结合使用。

[!code-csharp[Conceptual.TimeSpan.Custom#20](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/fspecifiers1.cs#20)]
[!code-vb[Conceptual.TimeSpan.Custom#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/fspecifiers1.vb#20)]

[返回表首](#table)

## <a name="the-ffffff-custom-format-specifier"></a><a name="f6Specifier"></a>“ffffff”自定义格式说明符

“ffffff”自定义格式说明符（包含六个“f”字符）输出时间间隔中的百万分之几秒。 在格式设置操作中，会截断其余任何小数位数。 在调用 <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> 或 <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType> 方法的分析操作中，输入字符串必须只有六个小数位。

下面的示例使用 "ffffff" 自定义格式说明符在 <xref:System.TimeSpan> 值中显示秒的百万分之几。 它首先用作唯一的格式说明符，然后在自定义格式字符串中与“s”说明符结合使用。

[!code-csharp[Conceptual.TimeSpan.Custom#20](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/fspecifiers1.cs#20)]
[!code-vb[Conceptual.TimeSpan.Custom#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/fspecifiers1.vb#20)]

[返回表首](#table)

## <a name="the-fffffff-custom-format-specifier"></a><a name="f7Specifier"></a>“fffffff”自定义格式说明符

“fffffff”自定义格式说明符（包含六个“f”字符）输出时间间隔中的千万分之几秒（或时钟周期的小数）。 在调用 <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> 或 <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType> 方法的分析操作中，输入字符串必须只有七个小数位。

下面的示例使用 "fffffff" 自定义格式说明符在 <xref:System.TimeSpan> 值中显示秒的千万分之一。 它首先用作唯一的格式说明符，然后在自定义格式字符串中与“s”说明符结合使用。

[!code-csharp[Conceptual.TimeSpan.Custom#20](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/fspecifiers1.cs#20)]
[!code-vb[Conceptual.TimeSpan.Custom#20](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/fspecifiers1.vb#20)]

[返回表首](#table)

## <a name="the-f-custom-format-specifier"></a><a name="F_Specifier"></a>“F”自定义格式说明符

“F”自定义格式说明符输出时间间隔中的十分之几秒。 在格式设置操作中，会截断其余任何小数位数。 如果时间间隔的十分之几秒的值为零，则它不包含在结果字符串中。 在调用 <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> 或 <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType> 方法的分析操作中，可以视需要选择是否显示秒的十分之几。

如果“F”自定义格式说明符单独使用，则指定“%F”，以便它不会错误地解释为标准格式字符串。

下面的示例使用 "F" 自定义格式说明符在 <xref:System.TimeSpan> 值中显示秒的十分之几。 它还在分析操作中使用此自定义格式说明符。

[!code-csharp[Conceptual.TimeSpan.Custom#21](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/f_specifiers1.cs#21)]
[!code-vb[Conceptual.TimeSpan.Custom#21](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/f_specifiers1.vb#21)]

[返回表首](#table)

## <a name="the-ff-custom-format-specifier"></a><a name="FF_Specifier"></a>“FF”自定义格式说明符

“FF”自定义格式说明符输出时间间隔中的百分之几秒。 在格式设置操作中，会截断其余任何小数位数。 如果存在任何尾随小数零，则它们不包含在结果字符串中。 在调用 <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> 或 <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType> 方法的分析操作中，可以视需要选择是否显示秒的十分之几和百分之几。

下面的示例使用 "FF" 自定义格式说明符在 <xref:System.TimeSpan> 值中显示秒的百分之几。 它还在分析操作中使用此自定义格式说明符。

[!code-csharp[Conceptual.TimeSpan.Custom#22](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/f_specifiers1.cs#22)]
[!code-vb[Conceptual.TimeSpan.Custom#22](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/f_specifiers1.vb#22)]

[返回表首](#table)

## <a name="the-fff-custom-format-specifier"></a><a name="F3_Specifier"></a>“FFF”自定义格式说明符

“FFF”自定义格式说明符（包含三个“F”字符）输出时间间隔中的毫秒。 在格式设置操作中，会截断其余任何小数位数。 如果存在任何尾随小数零，则它们不包含在结果字符串中。 在调用 <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> 或 <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType> 方法的分析操作中，可以视需要选择是否显示秒的十分之几、百分之几和千分之几。

下面的示例使用 "FFF" 自定义格式说明符在 <xref:System.TimeSpan> 值中显示秒的千分之几。 它还在分析操作中使用此自定义格式说明符。

[!code-csharp[Conceptual.TimeSpan.Custom#23](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/f_specifiers1.cs#23)]
[!code-vb[Conceptual.TimeSpan.Custom#23](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/f_specifiers1.vb#23)]

[返回表首](#table)

## <a name="the-ffff-custom-format-specifier"></a><a name="F4_Specifier"></a>“FFFF”自定义格式说明符

“FFFF”自定义格式说明符（包含四个“F”字符）输出时间间隔中的万分之几秒。 在格式设置操作中，会截断其余任何小数位数。 如果存在任何尾随小数零，则它们不包含在结果字符串中。 在调用 <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> 或 <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType> 方法的分析操作中，可以视需要选择是否显示秒的十分之几、百分之几、千分之几和万分之几。

下面的示例使用 "FFFF" 自定义格式说明符在 <xref:System.TimeSpan> 值中显示秒的万分之几。 它还在分析操作中使用“FFFF”自定义格式说明符。

[!code-csharp[Conceptual.TimeSpan.Custom#24](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/f_specifiers1.cs#24)]
[!code-vb[Conceptual.TimeSpan.Custom#24](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/f_specifiers1.vb#24)]

[返回表首](#table)

## <a name="the-fffff-custom-format-specifier"></a><a name="F5_Specifier"></a>“FFFFF”自定义格式说明符

“FFFFF”自定义格式说明符（包含五个“F”字符）输出时间间隔中的十万分之几秒。 在格式设置操作中，会截断其余任何小数位数。 如果存在任何尾随小数零，则它们不包含在结果字符串中。 在调用 <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> 或 <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType> 方法的分析操作中，可以视需要选择是否显示秒的十分之几、百分之几、千分之几、万分之几和十万分之几。

下面的示例使用 "FFFFF" 自定义格式说明符在 <xref:System.TimeSpan> 值中显示秒的十万分之几。 它还在分析操作中使用“FFFFF”自定义格式说明符。

[!code-csharp[Conceptual.TimeSpan.Custom#25](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/f_specifiers1.cs#25)]
[!code-vb[Conceptual.TimeSpan.Custom#25](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/f_specifiers1.vb#25)]

[返回表首](#table)

## <a name="the-ffffff-custom-format-specifier"></a><a name="F6_Specifier"></a>“FFFFFF”自定义格式说明符

“FFFFFF”自定义格式说明符（包含六个“F”字符）输出时间间隔中的百万分之几秒。 在格式设置操作中，会截断其余任何小数位数。 如果存在任何尾随小数零，则它们不包含在结果字符串中。 在调用 <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> 或 <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType> 方法的分析操作中，可以视需要选择是否显示秒的十分之几、百分之几、千分之几、万分之几、十万分之几和百万分之几。

下面的示例使用 "FFFFFF" 自定义格式说明符在 <xref:System.TimeSpan> 值中显示秒的百万分之几。 它还在分析操作中使用此自定义格式说明符。

[!code-csharp[Conceptual.TimeSpan.Custom#26](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/f_specifiers1.cs#26)]
[!code-vb[Conceptual.TimeSpan.Custom#26](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/f_specifiers1.vb#26)]

[返回表首](#table)

## <a name="the-fffffff-custom-format-specifier"></a><a name="F7_Specifier"></a>“FFFFFFF”自定义格式说明符

“FFFFFFF”自定义格式说明符（包含六个“F”字符）输出时间间隔中的千万分之几秒（或时钟周期的小数）。 如果存在任何尾随小数零，则它们不包含在结果字符串中。 在调用 <xref:System.TimeSpan.ParseExact%2A?displayProperty=nameWithType> 或 <xref:System.TimeSpan.TryParseExact%2A?displayProperty=nameWithType> 方法的分析操作中，可以视需要选择是否在输入字符串中显示秒的千万分之一。

下面的示例使用 "FFFFFFF" 自定义格式说明符在 <xref:System.TimeSpan> 值中显示秒的千万分之一。 它还在分析操作中使用此自定义格式说明符。

[!code-csharp[Conceptual.TimeSpan.Custom#27](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/f_specifiers1.cs#27)]
[!code-vb[Conceptual.TimeSpan.Custom#27](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/f_specifiers1.vb#27)]

[返回表首](#table)

## <a name="other-characters"></a>其他字符

格式字符串中的任何其他未转义字符（包括空白字符）都会解释为自定义格式说明符。 大多数情况下，若有其他任何未转义字符，便会导致 <xref:System.FormatException> 抛出。

可通过两种方法在格式字符串中包含文本字符：

- 将它括在单引号（文本字符串分隔符）中。

- 在它前面加上一个反斜杠（“\\”），即可将它解释为转义字符。 这意味着在 C# 中，格式字符串必须是 @-quoted，或者文本字符的前面必须加上额外的反斜杠。

  在某些情况下，可能需要使用条件逻辑才能在格式字符串中包括转义文本。 下面的示例使用条件逻辑包括表示负时间间隔的正负符号。

  [!code-csharp[Conceptual.TimeSpan.Custom#29](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/negativevalues1.cs#29)]
  [!code-vb[Conceptual.TimeSpan.Custom#29](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/negativevalues1.vb#29)]

.NET 没有为时间间隔中的分隔符定义语法。 这意味着在格式字符串中，天与小时、小时与分钟、分钟与秒以及秒与秒的小数之间的分隔符必须全都被视为字符文本。

下面的示例使用转义字符和单引号来定义在输出字符串中包含“minutes”一词的自定义格式字符串。

[!code-csharp[Conceptual.TimeSpan.Custom#28](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.timespan.custom/cs/literal1.cs#28)]
[!code-vb[Conceptual.TimeSpan.Custom#28](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.timespan.custom/vb/literal1.vb#28)]

[返回表首](#table)

## <a name="see-also"></a>请参阅

- [格式设置类型](formatting-types.md)
- [标准 TimeSpan 格式字符串](standard-timespan-format-strings.md)
