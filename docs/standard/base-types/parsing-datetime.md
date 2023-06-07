---
title: 将字符串转换为 DateTime
description: 了解分析表示日期和时间的字符串以从日期和时间字符串创建 DateTime 的方法。
ms.date: 02/15/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parsing strings, date and time strings
- date and time strings
- ParseExact method
- enumerations [.NET], parsing strings
- base types, parsing strings
- DateTime object
- time strings
ms.openlocfilehash: 557955b5c47695dea686527fd99653ef2efe19bb
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94821914"
---
# <a name="parse-date-and-time-strings-in-net"></a>分析 .NET 中的日期和时间字符串

分析字符串以将其转换为 <xref:System.DateTime> 对象需要你指定有关如何以文本格式表示日期和时间的信息。 不同的区域性所使用的日、月和年的顺序也不尽相同。 某些时间表示方法使用 24 小时制，其他时间表示方法则会指定“AM”和“PM”。 某些应用程序仅需要日期。 其他应用程序仅需要时间。 还有其他应用程序需要同时指定日期和时间。 通过将字符串转换为 <xref:System.DateTime> 对象的方法，你将能够提供有关你预期的格式和你的应用程序需要的日期和时间元素的详细信息。 将文本正确转换为 <xref:System.DateTime> 需要执行三个子任务：

1. 必须指定表示日期和时间的文本的预期格式。
1. 可以指定日期时间格式的区域性。
1. 可以指定如何以日期和时间格式设置文本表示方法中缺少的组成部分。

<xref:System.DateTime.Parse%2A> 和 <xref:System.DateTime.TryParse%2A> 方法可转换日期和时间的多个常见表示方法。 <xref:System.DateTime.ParseExact%2A> 和 <xref:System.DateTime.TryParseExact%2A> 方法可转换符合日期和时间格式字符串指定的模式的字符串表示形式。 （如需详细信息，请参阅有关[标准日期和时间格式字符串](standard-date-and-time-format-strings.md)和[自定义日期和时间格式字符串](custom-date-and-time-format-strings.md)的文章。）

当前的 <xref:System.Globalization.DateTimeFormatInfo> 对象提供对如何将文本解释为日期和时间的更好的控制。 <xref:System.Globalization.DateTimeFormatInfo> 的属性描述了日期和时间分隔符，以及月、日、年代的名称，还有“AM”和“PM”标志的格式。 当前线程区域性提供了表示当前区域性的 <xref:System.Globalization.DateTimeFormatInfo>。 如果你希望使用特定区域性或自定义设置，请指定分析方法的 <xref:System.IFormatProvider> 参数。 对于 <xref:System.IFormatProvider> 参数，应指定表示区域性的 <xref:System.Globalization.CultureInfo> 对象，或指定 <xref:System.Globalization.DateTimeFormatInfo> 对象。

表示日期或时间的文本可能缺少某些信息。 例如，大多数人都会假定“3 月 12 日”这个日期表示当前年份。 同样，“2018 年 3 月”表示年份为 2018，月份为 3 月。 表示时间的文本通常仅包括小时、分钟和 AM/PM 标志。  分析方法通过使用合理的默认值处理此类缺少的信息：

- 当仅存在时间时，日期部分将使用当前日期。
- 当仅存在日期时，时间部分将是午夜。
- 如果日期中未指定年份，则使用当前年份。
- 如果未指定一个月中的第几天，则使用一个月中的第一天。

如果字符串中存在日期，则它必须包括月份、某日或某年。 如果存在时间，则它必须包括小时和分钟或 AM/PM 标志。

你可以指定 <xref:System.Globalization.DateTimeStyles.NoCurrentDateDefault> 常量，以覆盖这些默认值。 使用该常量时，任何缺少的年、月或天属性将设置为值 `1`。 使用 <xref:System.DateTime.Parse%2A> 的[最后一个示例](#styles-example)对此行为进行了演示。

除了日期和时间组成部分，日期和时间的字符串表示形式还可以包含指示时间与协调世界时 (UTC) 相差多少的偏移量。 例如，字符串“2/14/2007 5:32:00 -7:00”定义比 UTC 早七个小时的时间。 如果在时间的字符串表示形式中省略了偏移，分析方法返回 <xref:System.DateTime> 对象，它的 <xref:System.DateTime.Kind%2A> 属性设置为 <xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType>。 如果指定了偏移，分析方法返回 <xref:System.DateTime> 对象，它的 <xref:System.DateTime.Kind%2A> 属性设置为 <xref:System.DateTimeKind.Local?displayProperty=nameWithType>，且值调整为采用计算机的本地时区。 可以通过结合使用 <xref:System.Globalization.DateTimeStyles> 值和分析方法来修改此行为。

格式提供程序还用于解释不明确的数字日期。 不清楚字符串“02/03/04”所表示的日期的哪些组成部分是月、日和年。 组成部分根据格式提供程序中相似日期格式的顺序进行解释。

## <a name="parse"></a>Parse

下面的示例说明了如何使用 <xref:System.DateTime.Parse%2A?displayProperty=nameWithType> 方法将 `string` 转换为 <xref:System.DateTime>。 此示例使用与当前线程关联的区域性。 如果与当前区域性关联的 <xref:System.Globalization.CultureInfo> 无法分析输入字符串，则会抛出 <xref:System.FormatException>。

> [!TIP]
> 本文中的所有 C# 示例均在你的浏览器中运行。 按“运行”按钮查看输出。 你还可以对其进行编辑以自行实验。

> [!NOTE]
> 这些示例可在适用于 [C#](https://github.com/dotnet/docs/tree/master/samples/snippets/csharp/how-to/conversions) 和 [Visual Basic](https://github.com/dotnet/docs/tree/master/samples/snippets/visualbasic/how-to/conversions) 的 GitHub 文档存储库中获取。

[!code-csharp-interactive[Parsing.DateAndTime#1](../../../samples/snippets/csharp/how-to/conversions/StringToDateTime.cs#1)]
[!code-vb[Parsing.DateAndTime#1](../../../samples/snippets/visualbasic/how-to/conversions/Program.vb#1)]

你也可以显式定义分析字符串时将使用其格式设置约定的区域性。 指定 <xref:System.Globalization.CultureInfo.DateTimeFormat%2A?displayProperty=nameWithType> 属性返回的一个标准 <xref:System.Globalization.DateTimeFormatInfo> 对象。 下面的示例使用格式提供程序将德语字符串分析为 <xref:System.DateTime>。 它创建了一个表示 `de-DE` 区域性的 <xref:System.Globalization.CultureInfo>。 `CultureInfo` 对象可以确保成功分析此特定的字符串。 这会排除处于 <xref:System.Threading.Thread.CurrentThread> 的 <xref:System.Threading.Thread.CurrentCulture> 中的任何设置。

[!code-csharp-interactive[Parsing.DateAndTime#2](../../../samples/snippets/csharp/how-to/conversions/StringToDateTime.cs#2)]
[!code-vb[Parsing.DateAndTime#2](../../../samples/snippets/visualbasic/how-to/conversions/Program.vb#2)]

不过，虽然可以使用 <xref:System.DateTime.Parse%2A> 方法重载指定自定义格式提供程序，但此方法不支持分析非标准格式。 若要分析非标准格式的日期和时间，请改用 <xref:System.DateTime.ParseExact%2A> 方法。

<a name="styles-example"></a>下面的示例使用 <xref:System.Globalization.DateTimeStyles> 枚举，指定不得将当前日期和时间信息添加到未指定字段的 <xref:System.DateTime>。

[!code-csharp-interactive[Parsing.DateAndTime#3](../../../samples/snippets/csharp/how-to/conversions/StringToDateTime.cs#3)]
[!code-vb[Parsing.DateAndTime#3](../../../samples/snippets/visualbasic/how-to/conversions/Program.vb#3)]

## <a name="parseexact"></a>ParseExact

<xref:System.DateTime.ParseExact%2A?displayProperty=nameWithType> 方法将符合其中一个指定字符串模式的字符串转换为 <xref:System.DateTime> 对象。 将未采用其中一种指定格式的字符串传递给此方法时，会引发 <xref:System.FormatException>。 可以指定一种标准日期和时间格式说明符或自定义格式说明符的组合。 使用自定义格式说明符可以构造自定义识别字符串。 有关说明符的说明，请参见有关[标准日期和时间格式字符串](standard-date-and-time-format-strings.md)和[自定义日期和时间格式字符串](custom-date-and-time-format-strings.md)的主题。

在下面的示例中，向 <xref:System.DateTime.ParseExact%2A?displayProperty=nameWithType> 方法传递了一个要分析的字符串对象，后跟一个格式说明符，再后跟一个 <xref:System.Globalization.CultureInfo> 对象。 此 <xref:System.DateTime.ParseExact%2A> 方法只能分析在 `en-US` 区域性中遵循长日期模式的字符串。

[!code-csharp-interactive[Parsing.DateAndTime#4](../../../samples/snippets/csharp/how-to/conversions/StringToDateTime.cs#4)]
[!code-vb[Parsing.DateAndTime#4](../../../samples/snippets/visualbasic/how-to/conversions/Program.vb#4)]

<xref:System.DateTime.Parse%2A> 和 <xref:System.DateTime.ParseExact%2A> 方法的每个重载还包含 <xref:System.IFormatProvider> 参数，用于提供有关字符串格式设置的区域性专用信息。 此 <xref:System.IFormatProvider> 对象为 <xref:System.Globalization.CultureInfo> 对象，表示标准区域性或 <xref:System.Globalization.CultureInfo.DateTimeFormat%2A?displayProperty=nameWithType> 属性返回的 <xref:System.Globalization.DateTimeFormatInfo> 对象。  <xref:System.DateTime.ParseExact%2A> 还使用定义一个或多个自定义日期和时间格式的其他字符串或字符串数组参数。

## <a name="see-also"></a>请参阅

- [分析字符串](parsing-strings.md)
- [格式设置类型](formatting-types.md)
- [.NET 中的类型转换](type-conversion.md)
- [标准日期和时间格式](standard-date-and-time-format-strings.md)
- [自定义日期和时间格式字符串](custom-date-and-time-format-strings.md)
