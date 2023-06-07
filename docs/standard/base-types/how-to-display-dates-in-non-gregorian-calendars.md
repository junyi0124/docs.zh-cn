---
title: 如何：用非公历日历显示日期
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- formatting [.NET], dates
- dates [.NET], formatting
- calendars [.NET], displaying dates
- displaying date and time data
ms.assetid: ed324eff-4aff-4a76-b6c0-04e6c0d8f5a9
ms.openlocfilehash: 9e8d628f3e15b080b4a5509726ece9da26523269
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95681387"
---
# <a name="how-to-display-dates-in-non-gregorian-calendars"></a>如何：用非公历日历显示日期

<xref:System.DateTime> 和 <xref:System.DateTimeOffset> 类型使用公历作为默认日历。 这意味着，调用日期和时间值的 `ToString` 方法会用公历日历显示该日期和时间的字符串表示形式，即使该日期和时间是使用其他日历创建的。 下面的示例对此进行了展示，虽然使用两种不同的方式创建采用波斯历的日期和时间值，但在调用 <xref:System.DateTime.ToString%2A> 方法时仍采用公历显示这些日期和时间值。 此示例对于用特定日历显示日期，反映了两种常用但不正确的方法。  
  
 [!code-csharp[Formatting.HowTo.Calendar#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.Calendar/cs/Calendar1.cs#1)]
 [!code-vb[Formatting.HowTo.Calendar#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.Calendar/vb/Calendar1.vb#1)]  
  
 两种不同的方法可以用于以特定日历显示日期。 第一种方法要求日历是特定区域性的默认日历。 第二种方法可以与任何日历一起使用。  
  
### <a name="to-display-the-date-for-a-cultures-default-calendar"></a>针对区域性默认日历显示日期  
  
1. 实例化从 <xref:System.Globalization.Calendar> 类派生的日历对象，表示要使用的日历。  
  
2. 实例化 <xref:System.Globalization.CultureInfo> 对象，表示用于显示日期的区域性格式设置。  
  
3. 调用 <xref:System.Array.Exists%2A?displayProperty=nameWithType> 方法，以确定日历对象是否为 <xref:System.Globalization.CultureInfo.OptionalCalendars%2A?displayProperty=nameWithType> 属性返回的数组成员。 这表明日历可用作 <xref:System.Globalization.CultureInfo> 对象的默认日历。 如果它不是数组的成员，请按照“用任何日历显示日期”部分中的说明执行。  
  
4. 将日历对象分配到 <xref:System.Globalization.CultureInfo.DateTimeFormat%2A?displayProperty=nameWithType> 属性返回的 <xref:System.Globalization.DateTimeFormatInfo> 对象的 <xref:System.Globalization.DateTimeFormatInfo.Calendar%2A> 属性。  
  
    > [!NOTE]
    > <xref:System.Globalization.CultureInfo> 类还包含 <xref:System.Globalization.CultureInfo.Calendar%2A> 属性。 不过，它是只读常量，并不会为了反映分配到 <xref:System.Globalization.DateTimeFormatInfo.Calendar%2A?displayProperty=nameWithType> 属性的新默认日历而发生变化。  
  
5. 调用 <xref:System.DateTime.ToString%2A> 或 <xref:System.DateTime.ToString%2A> 方法，并将它传递到在上一步中修改其默认日历的 <xref:System.Globalization.CultureInfo> 对象。  
  
### <a name="to-display-the-date-in-any-calendar"></a>用任何日历显示日期  
  
1. 实例化从 <xref:System.Globalization.Calendar> 类派生的日历对象，表示要使用的日历。  
  
2. 确定应在日期和时间值的字符串表示形式中出现的日期和时间元素。  
  
3. 对于要显示的每个日期和时间元素，调用日历对象的 `Get`... 方法。 有以下方法可用：  
  
    - <xref:System.Globalization.Calendar.GetYear%2A>：采用适当日历显示年份。  
  
    - <xref:System.Globalization.Calendar.GetMonth%2A>：采用适当日历显示月份。  
  
    - <xref:System.Globalization.Calendar.GetDayOfMonth%2A>：采用适当日历显示月份中的多少号。  
  
    - <xref:System.Globalization.Calendar.GetHour%2A>：采用适当日历显示一天中的小时段。  
  
    - <xref:System.Globalization.Calendar.GetMinute%2A>：采用适当日历显示小时中的分钟数。  
  
    - <xref:System.Globalization.Calendar.GetSecond%2A>：采用适当日历显示分钟中的秒数。  
  
    - <xref:System.Globalization.Calendar.GetMilliseconds%2A>：采用适当日历显示分钟中的毫秒数。  
  
## <a name="example"></a>示例  

 该示例使用两个不同日历显示日期。 它在将回历定义为 ar-JO 区域性的默认日历之后显示日期，并使用波斯日历（fa-IR 区域性不支持将它作为可选日历）显示日期。  
  
 [!code-csharp[Formatting.HowTo.Calendar#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.Calendar/cs/Calendar1.cs#2)]
 [!code-vb[Formatting.HowTo.Calendar#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.Calendar/vb/Calendar1.vb#2)]  
  
 每个 <xref:System.Globalization.CultureInfo> 对象都可以支持一个或多个日历（用 <xref:System.Globalization.CultureInfo.OptionalCalendars%2A> 属性表示）。 日历之一被指定为区域性的默认日历，通过只读属性 <xref:System.Globalization.CultureInfo.Calendar%2A?displayProperty=nameWithType> 进行返回。 可以将另一个可选日历指定为默认日历，只需将表示日历的 <xref:System.Globalization.Calendar> 对象分配给 <xref:System.Globalization.CultureInfo.DateTimeFormat%2A?displayProperty=nameWithType> 属性返回的 <xref:System.Globalization.DateTimeFormatInfo.Calendar%2A?displayProperty=nameWithType> 属性即可。 不过，某些日历（如 <xref:System.Globalization.PersianCalendar> 类表示的波斯历）不是任何区域性的可选日历。  
  
 该示例定义了一个可重用的日历实用工具类 `CalendarUtility`，用于处理有关使用特定日历生成日期的字符串表示形式的许多详细信息。 `CalendarUtility` 类包含以下成员：  
  
- 参数化构造函数，其中一个参数是要用来表示日期的 <xref:System.Globalization.Calendar> 对象。 这会分配给类的私有字段。  
  
- `CalendarExists`：返回布尔值的专用方法，用于指明以参数形式传递到方法的 <xref:System.Globalization.CultureInfo> 对象是否支持 `CalendarUtility` 对象表示的日历。 此方法包装对 <xref:System.Array.Exists%2A?displayProperty=nameWithType> 方法的调用，并向其传递 <xref:System.Globalization.CultureInfo.OptionalCalendars%2A?displayProperty=nameWithType> 数组。  
  
- `HasSameName`：专用方法，分配给以参数形式传递到 <xref:System.Array.Exists%2A?displayProperty=nameWithType> 方法的 <xref:System.Predicate%601> 委托。 数组的每个成员都会传递给该方法，直到该方法返回 `true`。 该方法确定可选日历的名称是否与 `CalendarUtility` 对象表示的日历相同。  
  
- `DisplayDate`：重载的公共方法，向它传递下面两个参数：要以 `CalendarUtility` 对象表示的日历表示的 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 值，以及要使用其格式设置规则的区域性。 它在返回日期的字符串表示形式时的行为取决于要使用其格式设置规则的区域性是否支持目标日历。  
  
 无论在此示例中使用哪种日历创建 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 值，相应值通常都表示为公历日期。 这是因为 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 类型不暂留任何日历信息。 它们在内部表示自 0001 年 1 月 1 日午夜以来所经历的时钟周期数。 该数字的解释取决于日历。 对于大多数区域性，默认日历是公历。
