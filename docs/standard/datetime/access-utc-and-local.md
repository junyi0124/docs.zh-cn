---
title: 如何：访问预定义 UTC 和本地时区对象
ms.date: 04/10/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- time zones [.NET], local
- predefined time zones
- UTC times, predefined
- local time zone access
- time zones [.NET], retrieving
- time zones [.NET], UTC
ms.assetid: 961fb70b-83f0-4dab-a042-cb5fcd817cf5
ms.openlocfilehash: b92ac812ed0bd0e80bb3a6ab03b52746eb15db97
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94818103"
---
# <a name="how-to-access-the-predefined-utc-and-local-time-zone-objects"></a>如何：访问预定义 UTC 和本地时区对象

<xref:System.TimeZoneInfo>类提供了两个属性： <xref:System.TimeZoneInfo.Utc%2A> 和，可 <xref:System.TimeZoneInfo.Local%2A> 让你的代码访问预定义的时区对象。 本主题介绍如何访问这些属性返回的 <xref:System.TimeZoneInfo> 对象。

### <a name="to-access-the-coordinated-universal-time-utc-timezoneinfo-object"></a>访问协调世界时 (UTC) TimeZoneInfo 对象

1. 使用 `static` `Shared` Visual Basic) 属性中的 (<xref:System.TimeZoneInfo.Utc%2A?displayProperty=nameWithType> 访问协调世界时。

2. 不是将 <xref:System.TimeZoneInfo> 属性返回的对象分配给对象变量，而是继续通过属性访问协调世界时 <xref:System.TimeZoneInfo.Utc%2A?displayProperty=nameWithType> 。

### <a name="to-access-the-local-time-zone"></a>访问本地时区

1. 使用 `static` `Shared` Visual Basic) 属性中的 (<xref:System.TimeZoneInfo.Local%2A?displayProperty=nameWithType> 访问本地系统时区。

2. 不是将 <xref:System.TimeZoneInfo> 属性返回的对象分配给对象变量，而是继续通过属性访问本地时区 <xref:System.TimeZoneInfo.Local%2A?displayProperty=nameWithType> 。

## <a name="example"></a>示例

下面的代码使用 <xref:System.TimeZoneInfo.Local%2A?displayProperty=nameWithType> 和 <xref:System.TimeZoneInfo.Utc%2A?displayProperty=nameWithType> 属性转换美国和加拿大东部标准时区的时间，并将时区名称显示到控制台。

[!code-csharp[System.TimeZone2.Concepts#13](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.TimeZone2.Concepts/CS/TimeZone2Concepts.cs#13)]
[!code-vb[System.TimeZone2.Concepts#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.TimeZone2.Concepts/VB/TimeZone2Concepts.vb#13)]

应始终通过属性访问本地时区， <xref:System.TimeZoneInfo.Local%2A?displayProperty=nameWithType> 而不是将本地时区分配给 <xref:System.TimeZoneInfo> 对象变量。 同样，应始终通过属性访问协调世界时， <xref:System.TimeZoneInfo.Utc%2A?displayProperty=nameWithType> 而不是将 UTC 时区分配给 <xref:System.TimeZoneInfo> 对象变量。 这可防止 <xref:System.TimeZoneInfo> 对象变量被调用 <xref:System.TimeZoneInfo.ClearCachedData%2A?displayProperty=nameWithType> 方法无效。

## <a name="compiling-the-code"></a>编译代码

此示例需要：

- <xref:System>要导入的命名空间与 `using` c # 代码) 中 (必需的语句一起导入。

## <a name="see-also"></a>另请参阅

- [日期、时间和时区](index.md)
- [查找本地系统上定义的时区](finding-the-time-zones-on-local-system.md)
- [如何：实例化 TimeZoneInfo 对象](instantiate-time-zone-info.md)
