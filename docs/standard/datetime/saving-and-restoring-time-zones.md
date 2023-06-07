---
title: 保存和还原时区
ms.date: 04/10/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- restoring time zones
- deserialization [.NET], time zones
- serialization [.NET], time zones
- time zone objects [.NET], restoring
- saving time zones
- time zone objects [.NET], deserializing
- time zones [.NET], saving
- time zones [.NET], restoring
- time zone objects [.NET], serializing
- time zone objects [.NET], saving
ms.assetid: 4028b310-e7ce-49d4-a646-1e83bfaf6f9d
ms.openlocfilehash: 0658bad6da078b6e44695a92b6cb2b22576f6424
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94817570"
---
# <a name="saving-and-restoring-time-zones"></a>保存和还原时区

<xref:System.TimeZoneInfo>类依赖于注册表来检索预定义的时区数据。 但注册表为动态结构。 此外，操作系统使用的时区信息主要用于处理当前年份的时间调整和转换。 对于依赖于准确时区数据的应用程序，这有两个主要含义：

- 可能不会在注册表中定义应用程序所需的时区，也可能已对其进行了重命名或删除。

- 在注册表中定义的时区可能缺少有关历史时区转换所需的特定调整规则的信息。

<xref:System.TimeZoneInfo>类通过对序列化的支持来解决这些限制 (保存) 并反序列化， (还原) 的时区数据。

## <a name="time-zone-serialization-and-deserialization"></a>时区序列化和反序列化

通过序列化和反序列化时区数据来保存和还原时区只涉及两个方法调用：

- 可以 <xref:System.TimeZoneInfo> 通过调用对象的方法来序列化对象 <xref:System.TimeZoneInfo.ToSerializedString%2A> 。 方法不采用任何参数，并返回包含时区信息的字符串。

- 可以 <xref:System.TimeZoneInfo> 通过将字符串传递到 `static` `Shared` Visual Basic) 方法中的 (，将对象从序列化字符串反序列化 <xref:System.TimeZoneInfo.FromSerializedString%2A?displayProperty=nameWithType> 。

## <a name="serialization-and-deserialization-scenarios"></a>序列化和反序列化方案

能够将对象) 对象保存 (或序列化 <xref:System.TimeZoneInfo> 并还原 (或反序列化) 以供以后使用，同时增加了实用工具和类的灵活性 <xref:System.TimeZoneInfo> 。 本部分介绍了序列化和反序列化最有用的一些情况。

### <a name="serializing-and-deserializing-time-zone-data-in-an-application"></a>序列化和反序列化应用程序中的时区数据

当需要时，可以从字符串还原序列化时区。 如果从注册表检索的时区无法正确转换特定日期范围内的日期和时间，则应用程序可能会执行此操作。 例如，Windows XP 注册表中的时区数据支持单个调整规则，而 Windows Vista 注册表中定义的时区通常提供有关两个调整规则的信息。 这意味着，历史时间转换可能不准确。 时区数据的序列化和反序列化可处理此限制。

在下面的示例中，不带调整规则的自定义 <xref:System.TimeZoneInfo> 类定义为表示从 1883 年到 1917 年（在引入夏时制之前）的美国东部标准时区。 自定义时区在具有全局作用域的变量中序列化。 时区转换方法 `ConvertUtcTime` 会将协调世界时 (UTC) 时间传递给转换。 如果日期和时间出现在1917或更早版本中，则从序列化的字符串还原自定义的东部标准时区，并替换从注册表中检索到的时区。

[!code-csharp[System.TimeZone2.Serialization.1#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Serialization.1/cs/Serialization.cs#1)]
[!code-vb[System.TimeZone2.Serialization.1#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Serialization.1/vb/Serialization.vb#1)]

### <a name="handling-time-zone-exceptions"></a>处理时区异常

由于注册表是动态结构，其内容可能会被意外或有意修改。 这意味着应该在注册表中定义的时区以及成功执行应用程序所需的时区可能不存在。 如果不支持时区序列化和反序列化，则只需进行很少的选择，而 <xref:System.TimeZoneNotFoundException> 通过结束应用程序来处理所生成的。 但是，通过使用时区序列化和反序列化，您可以 <xref:System.TimeZoneNotFoundException> 通过从序列化字符串还原所需的时区来处理意外错误，应用程序将继续运行。

下面的示例创建并序列化自定义中部标准时区。 然后，它会尝试从注册表检索中部标准时区。 如果检索操作引发 <xref:System.TimeZoneNotFoundException> 或 <xref:System.InvalidTimeZoneException> ，则异常处理程序将反序列化时区。

[!code-csharp[System.TimeZone2.Serialization.2#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Serialization.2/cs/Serialization2.cs#1)]
[!code-vb[System.TimeZone2.Serialization.2#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Serialization.2/vb/Serialization2.vb#1)]

### <a name="storing-a-serialized-string-and-restoring-it-when-needed"></a>存储序列化字符串并在需要时对其进行还原

前面的示例将时区信息存储在字符串变量中，并在需要时对其进行还原。 但是，包含序列化的时区信息的字符串本身可以存储在某个存储介质中，例如外部文件、嵌入在应用程序中的资源文件或注册表。  (请注意，有关自定义时区的信息应与注册表中系统的时区密钥分开存储。 ) 

以这种方式存储序列化时区字符串还可以将时区创建例程与应用程序本身隔开。 例如，时区创建例程可执行并创建包含应用程序可使用的历史时区信息的数据文件。 然后，可以在应用程序中安装数据文件，并在应用程序需要时，可以打开该文件并对其一个或多个时区进行反序列化。

有关使用嵌入资源存储序列化的时区数据的示例，请参阅 [如何：将时区保存到嵌入的资源](save-time-zones-to-an-embedded-resource.md) 和 [如何：从嵌入的资源还原时区](restore-time-zones-from-an-embedded-resource.md)。

## <a name="see-also"></a>另请参阅

- [日期、时间和时区](index.md)
