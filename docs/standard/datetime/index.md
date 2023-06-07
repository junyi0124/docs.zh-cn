---
title: 日期、时间和时区
ms.date: 04/10/2017
helpviewer_keywords:
- time zone objects [.NET]
- date and time data [.NET]
- time zones [.NET]
- times [.NET], time zones
- time [.NET], time zones
ms.assetid: 295c16e0-641b-4771-94b3-39c1ffa98c13
ms.openlocfilehash: b9dbbfa30c3398a984432fb0ecb8d0052c425f51
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94817895"
---
# <a name="dates-times-and-time-zones"></a>日期、时间和时区

除基本 <xref:System.DateTime> 结构外，.NET 还提供以下支持处理时区的类型：

* <xref:System.TimeZone>

  使用此类处理系统的本地时区和协调世界时 (UTC) 区域。 类的功能在 <xref:System.TimeZone> 很大程度上由 <xref:System.TimeZoneInfo> 类取代。

* <xref:System.TimeZoneInfo>

  此类可用于处理系统上预定义的任何时区、创建新时区，以及将日期和时间从一个时区轻松转换成另一个时区。 对于新的开发，使用 <xref:System.TimeZoneInfo> 类而不使用 <xref:System.TimeZone> 类。

* <xref:System.DateTimeOffset>

  使用此结构处理已知 UTC 偏移量（或差值）的日期和时间。 <xref:System.DateTimeOffset> 结构结合了日期和时间值与该时间的 UTC 偏移量。 由于其与 UTC 间的关系，单独的日期和时间值可以明确地识别单个时间点。 这使 <xref:System.DateTimeOffset> 值比 <xref:System.DateTime> 值更容易从一台计算机移植到另一台计算机。

文档的此部分提供处理时区和创建时区识别应用程序所需的信息，时区识别程序可将日期和时间从一个时区转换到另一时区。

## <a name="in-this-section"></a>在此部分中

[时区概述](time-zone-overview.md)讨论了创建时区识别应用程序时涉及的术语、概念以及问题。

[在 DateTime、DateTimeOffset、TimeSpan 和 TimeZoneInfo 之间进行选择](choosing-between-datetime.md) 讨论在 <xref:System.DateTime> <xref:System.DateTimeOffset> <xref:System.TimeZoneInfo> 处理日期和时间数据时何时使用、和类型。

[查找本地系统上定义的时区](finding-the-time-zones-on-local-system.md)介绍如何枚举在本地系统上找到的时区。

[如何：枚举计算机上存在的时区](enumerate-time-zones.md)举例说明枚举在计算机注册表中定义的时区，以及允许用户从列表选择一个预定义时区。

[如何：访问预定义的 UTC 和本地时区对象](access-utc-and-local.md)介绍如何访问协调世界时和本地时区。

[如何：实例化 TimeZoneInfo 对象](instantiate-time-zone-info.md)介绍如何实例化本地系统注册表中的 <xref:System.TimeZoneInfo> 对象。

[实例化 DateTimeOffset 对象](instantiating-a-datetimeoffset-object.md)讨论实例化 <xref:System.DateTimeOffset> 对象的方式，以及可以将 <xref:System.DateTime> 值转化为 <xref:System.DateTimeOffset> 值的方法。

[如何：创建不含调整规则的时区](create-time-zones-without-adjustment-rules.md)介绍如何创建不支持夏令时转换规则的自定义时区。

[如何：创建含调整规则的时区](create-time-zones-with-adjustment-rules.md)介绍如何创建支持一个或多个夏令时转换规则的自定义时区。

[保存和还原时区](saving-and-restoring-time-zones.md)介绍 <xref:System.TimeZoneInfo> 提供的时区数据序列化和反序列化支持，并通过一些应用场景介绍了如何使用这些功能。

[如何：将时区保存到嵌入的资源中](save-time-zones-to-an-embedded-resource.md)介绍如何创建自定义时区，并将其信息保存到资源文件中。

[如何：从嵌入的资源还原时区](restore-time-zones-from-an-embedded-resource.md)介绍如何实例化已保存到嵌入的资源文件中的自定义时区。

[使用日期和时间执行算术运算](performing-arithmetic-operations.md)讨论加上，减去和比较 <xref:System.DateTime> 与 <xref:System.DateTimeOffset> 值时会出现的问题。

[如何：在日期和时间算法中使用时区](use-time-zones-in-arithmetic.md)讨论如何执行反映时区调整规则的日期和时间算术。

[在 DateTime 与 DateTimeOffset 之间进行转换](converting-between-datetime-and-offset.md)介绍如何在 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 值间进行转换。

[在各时区之间转换时间](converting-between-time-zones.md)介绍如何将时间从一个时区转换到另一个时区。

[如何：解决不明确时间](resolve-ambiguous-times.md)介绍如何通过将不明确时间映射到时区标准时间解决该时间。

[如何：让用户解决不明确时间](let-users-resolve-ambiguous-times.md)介绍如何让用户确定不明确本地时间与协调世界时之间的映射。

## <a name="reference"></a>参考

<xref:System.TimeZoneInfo?displayProperty=nameWithType>
