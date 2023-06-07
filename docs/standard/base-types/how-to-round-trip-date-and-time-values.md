---
title: 如何：往返行程日期和时间值
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- round-trip date and time values
- dates [.NET], round-trip values
- time zones [.NET], round-trip date and time values
- time [.NET], round-trip values
- formatting strings [.NET], round-trip values
ms.assetid: b609b277-edc6-4c74-b03e-ea73324ecbdb
ms.openlocfilehash: 243ed98972f834c207331770f9d0202ddb60d4e6
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94821965"
---
# <a name="how-to-round-trip-date-and-time-values"></a>如何：往返行程日期和时间值

在许多应用程序中，日期和时间值旨在明确标识单个时间点。 本文演示了如何保存和还原 <xref:System.DateTime> 值、<xref:System.DateTimeOffset> 值以及包含时区信息的日期和时间值，以便还原后的值与保存的值标识的时间相同。

## <a name="round-trip-a-datetime-value"></a>往返 DateTime 值

1. 通过调用包含 "o" 格式说明符的 <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType> 方法，将 <xref:System.DateTime> 值转换为字符串表示形式。

2. 将 <xref:System.DateTime> 值的字符串表示形式保存到文件中，或跨进程、应用域或计算机边界传递它。

3. 检索表示 <xref:System.DateTime> 值的字符串。

4. 调用 <xref:System.DateTime.Parse%28System.String%2CSystem.IFormatProvider%2CSystem.Globalization.DateTimeStyles%29?displayProperty=nameWithType> 方法，并以 `styles` 参数值的形式传递 <xref:System.Globalization.DateTimeStyles.RoundtripKind?displayProperty=nameWithType>。

下面的示例展示了如何往返 <xref:System.DateTime> 值。

[!code-csharp[Formatting.HowTo.RoundTrip#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/cs/RoundTrip.cs#1)]
[!code-vb[Formatting.HowTo.RoundTrip#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/vb/RoundTrip.vb#1)]

往返 <xref:System.DateTime> 值时，此方法成功暂留所有本地时间和世界时间。 例如，如果本地 <xref:System.DateTime> 值保存在采用美国太平洋标准时区的系统会，并在位于美国中部标准时区的系统上还原，则还原的日期和时间会比原始时间晚两个小时，这反映了两个时区之间的时差。 但是，此方法对于未指定时间不一定准确。 所有 <xref:System.DateTime.Kind%2A> 属性为 <xref:System.DateTimeKind.Unspecified> 的 <xref:System.DateTime> 值都会被视为本地时间。 如果不是本地时间，则 <xref:System.DateTime> 不会成功标识正确的时间点。 针对此限制的解决方法是将日期和时间值与其时区紧密耦合，以便进行保存和还原操作。

## <a name="round-trip-a-datetimeoffset-value"></a>往返 DateTimeOffset 值

1. 通过调用包含 "o" 格式说明符的 <xref:System.DateTimeOffset.ToString%28System.String%29?displayProperty=nameWithType> 方法，将 <xref:System.DateTimeOffset> 值转换为字符串表示形式。

2. 将 <xref:System.DateTimeOffset> 值的字符串表示形式保存到文件中，或跨进程、应用域或计算机边界传递它。

3. 检索表示 <xref:System.DateTimeOffset> 值的字符串。

4. 调用 <xref:System.DateTimeOffset.Parse%28System.String%2CSystem.IFormatProvider%2CSystem.Globalization.DateTimeStyles%29?displayProperty=nameWithType> 方法，并以 `styles` 参数值的形式传递 <xref:System.Globalization.DateTimeStyles.RoundtripKind?displayProperty=nameWithType>。

下面的示例展示了如何往返 <xref:System.DateTimeOffset> 值。

[!code-csharp[Formatting.HowTo.RoundTrip#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/cs/RoundTrip.cs#2)]
[!code-vb[Formatting.HowTo.RoundTrip#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/vb/RoundTrip.vb#2)]

此方法始终将 <xref:System.DateTimeOffset> 值明确标识为单个时间点。 然后，可以调用 <xref:System.DateTimeOffset.ToUniversalTime%2A?displayProperty=nameWithType> 方法，将此值转换为协调世界时 (UTC)；也可以调用 <xref:System.DateTimeOffset.ToOffset%2A?displayProperty=nameWithType> 或 <xref:System.TimeZoneInfo.ConvertTime%28System.DateTimeOffset%2CSystem.TimeZoneInfo%29?displayProperty=nameWithType> 方法，将此值转换为特定时区时间。 此方法的主要限制在于，如果对表示特定时区时间的 <xref:System.DateTimeOffset> 值执行日期和时间算术，生成的结果对于相应时区来说可能并不准确。 这是因为 <xref:System.DateTimeOffset> 值在实例化时就与时区解除关联。 因此，执行日期和时间计算时，无法再应用该时区的调整规则。 可以通过定义包含日期和时间值以及其随附时区的自定义类型，来解决此问题。

## <a name="round-trip-a-date-and-time-value-with-its-time-zone"></a>往返包含时区信息的日期和时间值的具体步骤

1. 定义包含两个字段的类或结构。 第一个字段是 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 对象，第二个字段是 <xref:System.TimeZoneInfo> 对象。 下面的示例展示了这种类型的简单版本。

    [!code-csharp[Formatting.HowTo.RoundTrip#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/cs/RoundTrip.cs#3)]
    [!code-vb[Formatting.HowTo.RoundTrip#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/vb/RoundTrip.vb#3)]

2. 使用 <xref:System.SerializableAttribute> 属性标记类。

3. 使用 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Serialize%2A?displayProperty=nameWithType> 方法串行化对象。

4. 使用 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A> 方法还原对象。

5. 将反串行化的对象强制转换（在 C# 中）或转换（在 Visual Basic 中）为相应类型的对象。

下面的示例展示了如何往返同时存储时区以及日期和时间信息的对象。

[!code-csharp[Formatting.HowTo.RoundTrip#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/cs/RoundTrip.cs#4)]
[!code-vb[Formatting.HowTo.RoundTrip#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/vb/RoundTrip.vb#4)]

此方法应始终在保存和还原时间前后明确反映正确的时间点，前提是组合的日期和时间及时区对象的实现不允许日期值与时区值不同步。

## <a name="compile-the-code"></a>编译代码

这些示例需要：

- 使用 C# `using` 指令或 Visual Basic `Imports` 语句导入下列命名空间：

  - <xref:System>（仅限 C#）

  - <xref:System.Globalization?displayProperty=nameWithType>

  - <xref:System.IO?displayProperty=nameWithType>

  - <xref:System.Runtime.Serialization?displayProperty=nameWithType>

  - <xref:System.Runtime.Serialization.Formatters.Binary?displayProperty=nameWithType>

- 每个代码示例（`DateInTimeZone` 除外）都添加到类或 Visual Basic 模块中，且被包装到方法中，并通过 `Main` 进行调用。

## <a name="see-also"></a>请参阅

- [在 DateTime、DateTimeOffset、TimeSpan 和 TimeZoneInfo 之间进行选择](../datetime/choosing-between-datetime.md)
- [标准日期和时间格式字符串](standard-date-and-time-format-strings.md)
