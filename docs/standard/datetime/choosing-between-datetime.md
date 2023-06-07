---
title: 比较 DateTime、DateTimeOffset、TimeSpan 和 TimeZoneInfo
description: 了解 DateTime、DateTimeOffset、TimeSpan 和 TimeZoneInfo 类型之间的差异，以表示 .NET 中的日期和时间信息。
ms.date: 04/10/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DateTimeOffset structure
- TimeZoneInfo class
- time zones [.NET], common uses
- date and time classes [.NET]
- time zones [.NET], type options
- DateTime structure
ms.assetid: 07f17aad-3571-4014-9ef3-b695a86f3800
ms.openlocfilehash: 23c846dfd634e476b60ffd867519a60a0ae6b6cf
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94818090"
---
# <a name="choose-between-datetime-datetimeoffset-timespan-and-timezoneinfo"></a>在 DateTime、DateTimeOffset、TimeSpan 和 TimeZoneInfo 之间进行选择

.NET 应用程序可以通过多种方式使用日期和时间信息。 日期和时间信息的更常见用途包括：

- 仅反映日期，而时间信息不重要。

- 仅反映时间，而日期信息不重要。

- 反映未绑定到特定时间和位置的抽象的日期和时间（例如，国际上大多数连锁店在工作日上午 9:00 开张）。

- 若要从 .NET 以外的源中检索日期和时间信息，通常是在简单的数据类型中存储日期和时间信息。

- 唯一、明确地标识单个时间点。 某些应用程序要求只有主机系统上的日期和时间是明确的。 其他应用需要在系统中不明确 (也就是说，在一个系统上序列化的日期可以在世界各地的其他系统上有意义地反序列化和使用) 。

- 保留多个相关时间（例如请求程序的本地时间和服务器接收 Web 请求的时间）。

- 执行日期和时间算法，可能致使唯一、明确标识单个时间点。

.Net 包括 <xref:System.DateTime> 、 <xref:System.DateTimeOffset> 、 <xref:System.TimeSpan> 和 <xref:System.TimeZoneInfo> 类型，所有这些类型都可用于生成处理日期和时间的应用程序。

> [!NOTE]
> 本主题不讨论， <xref:System.TimeZone> 因为其功能几乎完全融入在 <xref:System.TimeZoneInfo> 类中。 请尽可能使用类， <xref:System.TimeZoneInfo> 而不使用 <xref:System.TimeZone> 类。

## <a name="the-datetimeoffset-structure"></a>DateTimeOffset 结构

<xref:System.DateTimeOffset> 结构表示日期和时间值，以及指示此值与 UTC 的差异程度的偏移量。 因此，此值始终明确地标识单个时间点。

<xref:System.DateTimeOffset> 类型包括 <xref:System.DateTime> 类型的所有功能以及时区感知功能。 这使它适用于以下应用程序：

- 唯一、明确地标识单个时间点。 <xref:System.DateTimeOffset> 类型可用于明确定义“现在”的含义、记录事务时间、记录系统或应用程序事件时间以及记录创建和修改时间。

- 执行常规日期和时间算法。

- 保留多个相关时间，只要这些时间存储为两个单独的值或结构中的两个成员。

> [!NOTE]
> <xref:System.DateTimeOffset> 值的使用频率比 <xref:System.DateTime> 值的更高。 因此，请考虑 <xref:System.DateTimeOffset> 将其用作应用程序开发的默认日期和时间类型。

<xref:System.DateTimeOffset>值不与特定时区相关联，但可以来自各种时区。 下面的示例列出了若干 <xref:System.DateTimeOffset> 值 (包括) 可以属于的本地太平洋标准时间的时区。

[!code-csharp[System.DateTimeOffset.Conceptual#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/cs/Conceptual1.cs#1)]
[!code-vb[System.DateTimeOffset.Conceptual#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual/vb/Conceptual1.vb#1)]

输出显示，此示例中的每个日期和时间值至少可以属于三个不同时区。 <xref:System.DateTimeOffset>6/10/2007 的值显示，如果日期和时间值表示夏令时，则其相对于 utc 的偏移量甚至不一定对应于原始时区的基本 UTC 偏移量或其显示名称中找到的相对于 utc 的偏移量。 由于单个值与其时区 <xref:System.DateTimeOffset> 不紧密耦合，因此无法反映时区与夏令时的来回转换。 当使用日期和时间算法操作值时，这可能会出现问题 <xref:System.DateTimeOffset> 。 有关如何以考虑时区调整规则的方式执行日期和时间算术运算的讨论，请参阅[执行日期和时间算术运算](performing-arithmetic-operations.md)。

## <a name="the-datetime-structure"></a>DateTime 结构

<xref:System.DateTime> 值定义特定日期和时间。 它包括一个 <xref:System.DateTime.Kind%2A> 属性，该属性提供有关日期和时间所属时区的有限信息。 由 <xref:System.DateTimeKind> 属性返回的 <xref:System.DateTime.Kind%2A> 值指示 <xref:System.DateTime> 值是表示本地时间 (<xref:System.DateTimeKind.Local?displayProperty=nameWithType>)、协调世界时 (UTC) (<xref:System.DateTimeKind.Utc?displayProperty=nameWithType>) 还是未指定的时间 (<xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType>)。

此 <xref:System.DateTime> 结构适用于具有以下一种或多种特征的应用程序：

- 仅使用日期。

- 只使用时间。

- 使用抽象的日期和时间。

- 使用缺少时区信息的日期和时间。

- 只使用 UTC 日期和时间。

- 从 .NET 外部的源（如 SQL 数据库）中检索日期和时间信息。 通常，这些源按与 <xref:System.DateTime> 结构兼容的简单格式存储日期和时间信息。

- 执行日期和时间算法，但不关注常规结果。 例如，在向特定日期和时间添加六个月的加法运算中，是否将结果调整为夏令时通常并不重要。

除非特定的 <xref:System.DateTime> 值表示 UTC，否则日期和时间值通常不明确的或其可移植性受限。 例如，如果 <xref:System.DateTime> 值表示本地时间，则在该本地时区内可移植（即，如果在其他系统的相同时区上反序列化此值，它仍然明确标识单个时间点）。 在本地时区之外， <xref:System.DateTime> 值可以具有多个解释。 如果值的 <xref:System.DateTime.Kind%2A> 属性是 <xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType>，则它的可移植性更低：此时它在相同时区内（可能甚至在首次序列化的同一系统上）是不明确的。 仅当 <xref:System.DateTime> 值表示 UTC 时，无论使用此值的系统和时区如何，它都会明确标识单个时间点。

> [!IMPORTANT]
> 在保存或共享 <xref:System.DateTime> 数据时，应使用 UTC 并将 <xref:System.DateTime> 值的 <xref:System.DateTime.Kind%2A> 属性设置为 <xref:System.DateTimeKind.Utc?displayProperty=nameWithType>。

## <a name="the-timespan-structure"></a>TimeSpan 结构

<xref:System.TimeSpan> 结构表示时间间隔。 它的两个典型用途是：

- 反映两个日期和时间值之间的时间间隔。 例如，两个 <xref:System.DateTime> 值相减将返回 <xref:System.TimeSpan> 值。

- 测量运行时间。 例如， <xref:System.Diagnostics.Stopwatch.Elapsed%2A?displayProperty=nameWithType> 属性返回一个 <xref:System.TimeSpan> 值，该值反映自调用 <xref:System.Diagnostics.Stopwatch> 开始测量运行时间的其中一个方法以来经过的时间间隔。

值 <xref:System.TimeSpan> 还可用于替换值（ <xref:System.DateTime> 当该值反映不引用特定日期的时间）。 此用法与 <xref:System.DateTime.TimeOfDay%2A?displayProperty=nameWithType> 和 <xref:System.DateTimeOffset.TimeOfDay%2A?displayProperty=nameWithType> 属性类似，后者返回一个 <xref:System.TimeSpan> 值，该值表示不引用日期的时间。 例如， <xref:System.TimeSpan> 结构可用于反映商店每天的开张或打烊时间，还可用来表示任何常规事件发生的时间。

以下示例定义了 `StoreInfo` 结构，其中包含表示商店开张或打烊时间的 <xref:System.TimeSpan> 对象，以及表示商店所在时区的 <xref:System.TimeZoneInfo> 对象。 此结构还包含两个方法（`IsOpenNow` 和 `IsOpenAt`），用于指示假定用户处于本地时区时其所指定的时间商店是否开张。

[!code-csharp[Conceptual.ChoosingDates#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.choosingdates/cs/datetimereplacement1.cs#1)]
[!code-vb[Conceptual.ChoosingDates#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.choosingdates/vb/datetimereplacement1.vb#1)]

随后， `StoreInfo` 结构可由客户端代码按如下方式使用。

[!code-csharp[Conceptual.ChoosingDates#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.choosingdates/cs/datetimereplacement1.cs#2)]
[!code-vb[Conceptual.ChoosingDates#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.choosingdates/vb/datetimereplacement1.vb#2)]

## <a name="the-timezoneinfo-class"></a>TimeZoneInfo 类

<xref:System.TimeZoneInfo> class represents any of the Earth's time zones, and enables the conversion of any date and time in one time zone to its equivalent in another time zone. 借助 <xref:System.TimeZoneInfo> 类，即可处理日期和时间，以使任何日期和时间值均明确标识单个时间点。 <xref:System.TimeZoneInfo> 类也可扩展。 虽然它依赖于为 Windows 系统提供且在注册表中定义的时区信息，但仍然支持创建自定义时区。 它还支持时区信息的序列化和反序列化。

在某些情况下，可能需要进一步的开发工作才可以充分利用 <xref:System.TimeZoneInfo> 类。 如果日期和时间值不与其所属的时区紧密耦合，则需要进一步的工作。 除非您的应用程序提供某种机制来将日期和时间与其关联的时区链接在一起，否则特定日期和时间值很容易与其时区无关。 链接此信息的一种方法是，定义一个同时包含日期和时间值及其关联时区对象的类或结构。

若要利用 .NET 中的时区支持，您必须知道日期和时间值在实例化日期和时间对象时所属于的时区。 时区通常是未知的，尤其是在 web 或网络应用中。

## <a name="see-also"></a>另请参阅

- [日期、时间和时区](index.md)
