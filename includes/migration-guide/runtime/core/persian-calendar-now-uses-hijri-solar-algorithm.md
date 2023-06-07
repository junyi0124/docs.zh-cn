---
ms.openlocfilehash: 14581b193fc000c7f805a0602e191cad688c014a
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496166"
---
### <a name="persian-calendar-now-uses-the-hijri-solar-algorithm"></a>波斯日历现在使用回历太阳算法

#### <a name="details"></a>详细信息

自 .NET Framework 4.6 起，<xref:System.Globalization.PersianCalendar?displayProperty=fullName> 类使用回历太阳算法。 自 .NET Framework 4.6 起，对于早于 1800 年或晚于 2023 年（公历）的日期，在 <xref:System.Globalization.PersianCalendar?displayProperty=fullName> 和其他日历之间转换日期所得结果可能略微不同。此外，<xref:System.Globalization.PersianCalendar.MinSupportedDateTime?displayProperty=nameWithType> 现在为 <code>March 22, 0622</code>，而不是 <code>March 21, 0622</code>。

#### <a name="suggestion"></a>建议

请注意，在 .NET Framework 4.6 中使用 PersianCalendar 时，某些较早或较晚的日期可能略微不同。 另外，当在不同 .NET Framework 版本上运行的进程之间序列化日期时，请勿将它们存储为 PersianCalendar 日期字符串（因为这些值可能不相同）。

| 名称    | 值       |
|:--------|:------------|
| 范围   |次要|
|Version|4.6|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Globalization.PersianCalendar?displayProperty=nameWithType>

<!--

#### Affected APIs

- `T:System.Globalization.PersianCalendar`

-->
