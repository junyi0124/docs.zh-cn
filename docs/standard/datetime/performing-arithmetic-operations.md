---
title: 使用日期和时间执行算术运算
ms.date: 04/10/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- times [.NET], arithmetic operations
- dates [.NET], arithmetic operations
- time zones [.NET], arithmetic operations
- arithmetic operations [.NET], dates and times
- dates [.NET], comparing
- DateTime structure, arithmetic operations
- DateTimeOffset structure, arithmetic operations
ms.assetid: 87c7ddf2-f15e-48af-8602-b3642237e6d0
ms.openlocfilehash: af294c45359f6226c4189aabb34fdfc670bbd1c9
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94817765"
---
# <a name="performing-arithmetic-operations-with-dates-and-times"></a>使用日期和时间执行算术运算

尽管 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 结构都提供对其值执行算术运算的成员，但算术运算的结果却非常不同。 本主题将探讨这些差异，将它们与日期和时间数据中的时区识别度关联起来，并讨论如何使用日期和时间数据执行完全时区感知操作。

## <a name="comparisons-and-arithmetic-operations-with-datetime-values"></a>日期时间值的比较和算术运算

<xref:System.DateTime.Kind%2A?displayProperty=nameWithType>属性允许将 <xref:System.DateTimeKind> 值分配给日期和时间，以指示它是表示本地时间、协调世界时 (UTC) 还是未指定时区中的时间。 但是，在对值进行比较或执行日期和时间算法时，会忽略此有限的时区信息 <xref:System.DateTimeKind> 。 以下示例通过比较当前本地时间与当前 UTC 时间，对此进行了阐释。

[!code-csharp[System.DateTimeOffset.Conceptual#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/cs/Conceptual2.cs#2)]
[!code-vb[System.DateTimeOffset.Conceptual#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/vb/Conceptual2.vb#2)]

<xref:System.DateTime.CompareTo%28System.DateTime%29> 方法报告的本地时间比 UTC 时间早（或小），并且减法运算指示，对于美国太平洋标准时区内的系统，其 UTC 时间和本地时间之间的时差是 7 小时。 但由于这两个值提供单个时间点的不同表示形式，因此在这种情况下，时间间隔完全是由本地时区与 UTC 之间的时差所构成。

通常情况下， <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> 属性不会影响由 <xref:System.DateTime.Kind> 比较和算术方法返回的结果 (因为两个相同时间点的比较表明) ，尽管它可能会影响对这些结果的解释。 例如：

- 对两个日期和时间值执行的任何算术运算的结果 <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> 都相等，这两个值的属性都 <xref:System.DateTimeKind> 反映了两个值之间的实际时间间隔。 同样，对两个日期和时间值进行比较可精确反映出时间之间的关系。

- 对两个日期和时间值执行的算术或比较运算的结果，其属性在两个日期和时间值 <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> 上都相等， <xref:System.DateTimeKind> 两个日期和时间值具有不同的 <xref:System.DateTime.Kind%2A?displayProperty=nameWithType> 属性值，这反映了两个值之间的时钟时间差异。

- 对本地日期和时间值执行的算术或比较运算不考虑某个特定值是否不明确或无效，也不考虑任何调整规则（因本地时区与夏令时的来回转换）的影响。

- 任何比较或计算 UTC 与本地时间之差的运算所得出的结果中都包含一个时间间隔，它等于本地时区与 UTC 之间的时差。

- 任何比较或计算未指定时间与 UTC 或本地时间的运算都反映简单的时钟时间。 时区差异未纳入考虑范围，且结果不会反映是否应用了时区调整规则。

- 任何比较或计算两个未指定时间之差的运算都可能包含一个未知间隔，它反映两个不同时区内的时间之差。

在许多情况下，时区差异不会影响日期和时间计算 (对其中某些部分进行讨论，请参阅在 [DateTime、DateTimeOffset、TimeSpan 和 TimeZoneInfo 之间进行选择](choosing-between-datetime.md)) 或日期和时间数据的上下文定义比较或算术运算的含义。

## <a name="comparisons-and-arithmetic-operations-with-datetimeoffset-values"></a>具有 DateTimeOffset 值的比较和算术运算

<xref:System.DateTimeOffset>值不仅包括日期和时间，还包括明确定义相对于 UTC 的日期和时间的偏移量。 这样就可以定义与值稍有不同的相等性 <xref:System.DateTimeOffset> 。 <xref:System.DateTime>当值相同时，如果它们具有相同的日期和时间值，则值相等; <xref:System.DateTimeOffset> 如果两个值都引用同一时间点，则值相等。 这使得在 <xref:System.DateTimeOffset> 比较中使用的值和大多数算术运算（用于确定两个日期和时间之间的间隔）的情况下，该值更准确且更少。 下面的示例（ <xref:System.DateTimeOffset> 相当于比较了本地值和 UTC 值的上一个示例 <xref:System.DateTimeOffset> ）演示了这种行为上的差异。

[!code-csharp[System.DateTimeOffset.Conceptual#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/cs/Conceptual3.cs#3)]
[!code-vb[System.DateTimeOffset.Conceptual#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/vb/Conceptual3.vb#3)]

在此示例中， <xref:System.DateTimeOffset.CompareTo%2A> 方法指示当前本地时间和当前 UTC 时间相等，并且值的减法 <xref:System.DateTimeOffset.CompareTo(System.DateTimeOffset)> 指示这两个时间之间的差异是 <xref:System.TimeSpan.Zero?displayProperty=nameWithType> 。

<xref:System.DateTimeOffset>在日期和时间算法中使用值的主要限制在于，虽然 <xref:System.DateTimeOffset> 值具有一定的时区感知，但并不是完全时区感知。 尽管在第一次为 <xref:System.DateTimeOffset> 变量分配值时值的偏移量反映了时区与 UTC 的偏移量 <xref:System.DateTimeOffset> ，但此后它将与时区解除关联。 由于不再直接与可识别时间关联，日期和时间间隔的相加和相减将不会考虑时区调整规则。

为举例说明，美国中部标准时区中的夏令时转换将在凌晨2:00 进行。 两个半小时的代码。 这意味着，向中部标准时间 2008 年 3 月 9 日凌晨 1:30 增加两个半小时的间隔， 得出的日期和时间应为  两个半小时的代码。 但是，如下面的示例所示，加法运算得出的结果却是  两个半小时的代码。 请注意，此操作的结果确实表示正确的时间点，尽管它不是我们感兴趣 (的时区中的时间，但它没有) 预期的时区偏移量。

[!code-csharp[System.DateTimeOffset.Conceptual#4](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/cs/Conceptual4.cs#4)]
[!code-vb[System.DateTimeOffset.Conceptual#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/vb/Conceptual4.vb#4)]

## <a name="arithmetic-operations-with-times-in-time-zones"></a>时间区域中时间的算术运算

<xref:System.TimeZoneInfo>类包括多种转换方法，这些方法在将时间从一个时区转换到另一个时区时自动应用调整。 这些功能包括以下这些：

- <xref:System.TimeZoneInfo.ConvertTime%2A>和 <xref:System.TimeZoneInfo.ConvertTimeBySystemTimeZoneId%2A> 方法，用于在任意两个时区之间转换时间。

- <xref:System.TimeZoneInfo.ConvertTimeFromUtc%2A>和 <xref:System.TimeZoneInfo.ConvertTimeToUtc%2A> 方法，用于将特定时区中的时间转换为 utc，或将 UTC 转换为特定时区中的时间。

有关详细信息，请参阅 [在时区之间转换时间](converting-between-time-zones.md)。

在 <xref:System.TimeZoneInfo> 执行日期和时间算术运算时，类不提供任何可自动应用调整规则的方法。 但是可以通过将某一时区内的时间转换为 UTC，执行算术运算，然后再将 UTC 转换回该时区内的时间，来实现此目的。 有关详细信息，请参阅 [如何：在日期和时间算法中使用时区](use-time-zones-in-arithmetic.md)。

例如，以下代码类似于之前向 2008 年 3 月 9 日凌晨 2:00 增加 两个半小时的代码。 但是，由于其在执行日期和时间算术前将中部标准时间转换为 UTC，然后将 UTC 结果转换回中部标准时间，所以得出的时间反映的是中部标准时区转换为夏令时。

[!code-csharp[System.DateTimeOffset.Conceptual#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/cs/Conceptual5.cs#5)]
[!code-vb[System.DateTimeOffset.Conceptual#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/vb/Conceptual5.vb#5)]

## <a name="see-also"></a>另请参阅

- [日期、时间和时区](index.md)
- [如何：在日期和时间算术中使用时区](use-time-zones-in-arithmetic.md)
