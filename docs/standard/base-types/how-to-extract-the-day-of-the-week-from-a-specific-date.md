---
title: 如何：从特定日期中提取星期几
description: 了解如何在 .NET 中确定特定日期的周序数日期。 了解如何显示特定日期的本地化星期名称。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- formatting [.NET], dates
- DateTime.DayOfWeek property
- DateTime.ToString method
- dates [.NET], retrieving week information
- DateTimeOffset.DayOfWeek property
- dates [.NET], day of week
- Weekday function
- day of week [.NET]
- extracting day of week
- weekday names
- WeekdayName function
- numbers [.NET], day of week
- formatting [.NET], time
- DateTimeOffset.ToString method
- full weekday names
ms.assetid: 1c9bef76-5634-46cf-b91c-9b9eb72091d7
ms.openlocfilehash: 9db11146ee9428ce22b08accacf7660137d539c7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726985"
---
# <a name="how-to-extract-the-day-of-the-week-from-a-specific-date"></a>如何：从特定日期中提取星期几

利用 .NET，可以很容易地确定某个特定日期是星期几，以及显示某个特定日期的本地化星期几名称。 指示与特定日期相对应的星期几的枚举值可以从 <xref:System.DateTime.DayOfWeek%2A> 或 <xref:System.DateTimeOffset.DayOfWeek%2A> 属性中获取。 与此不同的是，检索星期几名称是一项格式化操作，可通过调用格式化方法来执行，例如日期和时间值的 `ToString` 方法或 <xref:System.String.Format%2A?displayProperty=nameWithType> 方法。 本主题演示如何执行这些格式化操作。  
  
## <a name="extract-a-number-indicating-the-day-of-the-week"></a>提取指示星期几的数字
  
1. 如果要使用日期的字符串表示形式，请使用静态 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 方法将其转换为 <xref:System.DateTime.Parse%2A?displayProperty=nameWithType> 或 <xref:System.DateTimeOffset.Parse%2A?displayProperty=nameWithType> 值。  
  
2. 使用 <xref:System.DateTime.DayOfWeek%2A?displayProperty=nameWithType> 或 <xref:System.DateTimeOffset.DayOfWeek%2A?displayProperty=nameWithType> 属性检索指示星期几的 <xref:System.DayOfWeek> 值。  
  
3. 如有必要，可将 <xref:System.DayOfWeek> 值强制转换（在 C# 中）或转换（在 Visual Basic 中）为整数。  
  
 下面的示例将显示一个整数，用于表示特定日期的星期几。  
  
 [!code-csharp[Formatting.Howto.WeekdayName#7](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/weekdaynumber7.cs#7)]
 [!code-vb[Formatting.Howto.WeekdayName#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/weekdaynumber7.vb#7)]  
  
## <a name="extract-the-abbreviated-weekday-name"></a>提取缩写的工作日名称
  
1. 如果要使用日期的字符串表示形式，请使用静态 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 方法将其转换为 <xref:System.DateTime.Parse%2A?displayProperty=nameWithType> 或 <xref:System.DateTimeOffset.Parse%2A?displayProperty=nameWithType> 值。  
  
2. 你可以提取当前区域性或特定区域性的缩写的星期几名称：  
  
    1. 若要提取当前区域性的缩写的星期几名称，请调用日期和时间值的 <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType> 或 <xref:System.DateTimeOffset.ToString%28System.String%29?displayProperty=nameWithType> 实例方法，并以 `format` 参数的形式传递字符串“ddd”。 下面的示例演示 <xref:System.DateTime.ToString%28System.String%29> 方法的调用。  
  
         [!code-csharp[Formatting.Howto.WeekdayName#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/abbrname1.cs#1)]
         [!code-vb[Formatting.Howto.WeekdayName#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/abbrname1.vb#1)]  
  
    2. 若要提取特定区域性的缩写的星期几名称，请调用日期和时间值的 <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> 或 <xref:System.DateTimeOffset.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> 实例方法。 同时以 `format` 参数形式传递字符串“ddd”。 以 <xref:System.Globalization.CultureInfo> 参数的形式传递表示要检索其星期几名称的区域性的 <xref:System.Globalization.DateTimeFormatInfo> 或 `provider` 对象。 下面的代码阐释如何使用表示 fr-FR 区域性的 <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29> 对象调用 <xref:System.Globalization.CultureInfo> 方法。  
  
         [!code-csharp[Formatting.Howto.WeekdayName#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/abbrname2.cs#2)]
         [!code-vb[Formatting.Howto.WeekdayName#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/abbrname2.vb#2)]  
  
## <a name="extract-the-full-weekday-name"></a>提取完整的工作日名称
  
1. 如果要使用日期的字符串表示形式，请使用静态 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 方法将其转换为 <xref:System.DateTime.Parse%2A?displayProperty=nameWithType> 或 <xref:System.DateTimeOffset.Parse%2A?displayProperty=nameWithType> 值。  
  
2. 你可以提取当前区域性或特定区域性的完整的星期几名称：  
  
    1. 若要提取当前区域性的缩写的星期几名称，请调用日期和时间值的 <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType> 或 <xref:System.DateTimeOffset.ToString%28System.String%29?displayProperty=nameWithType> 实例方法，并以 `format` 参数的形式传递字符串“dddd”。 下面的示例演示 <xref:System.DateTime.ToString%28System.String%29> 方法的调用。  
  
         [!code-csharp[Formatting.Howto.WeekdayName#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/fullname4.cs#4)]
         [!code-vb[Formatting.Howto.WeekdayName#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/fullname4.vb#4)]  
  
    2. 若要提取特定区域性的星期几名称，请调用日期和时间值的 <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> 或 <xref:System.DateTimeOffset.ToString%28System.String%2CSystem.IFormatProvider%29?displayProperty=nameWithType> 实例方法。 同时以 `format` 参数形式传递字符串“dddd”。 以 <xref:System.Globalization.CultureInfo> 参数的形式传递表示要检索其星期几名称的区域性的 <xref:System.Globalization.DateTimeFormatInfo> 或 `provider` 对象。 下面的代码阐释如何使用表示 es-ES 区域性的 <xref:System.DateTime.ToString%28System.String%2CSystem.IFormatProvider%29> 对象调用 <xref:System.Globalization.CultureInfo> 方法。  
  
         [!code-csharp[Formatting.Howto.WeekdayName#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/fullname5.cs#5)]
         [!code-vb[Formatting.Howto.WeekdayName#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/fullname5.vb#5)]  
  
## <a name="example"></a>示例  

 该示例演示如何调用 <xref:System.DateTime.DayOfWeek%2A?displayProperty=nameWithType> 和 <xref:System.DateTimeOffset.DayOfWeek%2A?displayProperty=nameWithType> 属性以及 <xref:System.DateTime.ToString%2A?displayProperty=nameWithType> 和 <xref:System.DateTimeOffset.ToString%2A?displayProperty=nameWithType> 方法，以检索特定日期中表示星期几的数字、缩写的星期几名称和完整的星期几名称。  
  
 [!code-csharp[Formatting.Howto.WeekdayName#6](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/example6.cs#6)]
 [!code-vb[Formatting.Howto.WeekdayName#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/example6.vb#6)]  
  
 个别语言可能提供与 .NET 所提供的功能相同或互为补充的功能。 例如，Visual Basic 包括这样的两个函数：  
  
- `Weekday`，它返回指示特定日期中表示星期几的数字。 此函数将一周中第一天的序数值视为一，而 <xref:System.DateTime.DayOfWeek%2A?displayProperty=nameWithType> 属性却将其视为零。  
  
- `WeekdayName`，它返回当前区域性中与特定星期几相对应的周的名称。  
  
 下面的示例演示了 Visual Basic `Weekday` 和 `WeekdayName` 函数的用法。  
  
 [!code-vb[Formatting.HowTo.WeekdayName#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/example9.vb#9)]  
  
 也可以使用 <xref:System.DateTime.DayOfWeek%2A?displayProperty=nameWithType> 属性返回的值检索特定日期的星期几名称。 此过程只需对 <xref:System.Enum.ToString%2A> 属性返回的值调用 <xref:System.DayOfWeek> 方法。 但是，此技术并不生成当前区域性的本地化星期几名称，如下面的示例所示。  
  
 [!code-csharp[Formatting.HowTo.WeekdayName#8](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/cs/Howto1.cs#8)]
 [!code-vb[Formatting.HowTo.WeekdayName#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.WeekdayName/vb/Howto1.vb#8)]

## <a name="see-also"></a>请参阅

- [标准日期和时间格式字符串](standard-date-and-time-format-strings.md)
- [自定义日期和时间格式字符串](custom-date-and-time-format-strings.md)
