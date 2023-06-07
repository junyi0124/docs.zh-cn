---
title: 如何：从嵌入的资源还原时区
ms.date: 04/10/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- time zones [.NET], deserializing
- time zones [.NET], restoring
ms.assetid: 6b7b4de9-da07-47e3-8f4c-823f81798ee7
ms.openlocfilehash: a1302f595a0907103bae2695e703e5af6da660a7
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94817661"
---
# <a name="how-to-restore-time-zones-from-an-embedded-resource"></a>如何：从嵌入的资源还原时区

本主题介绍如何还原已保存到资源文件中的时区。 有关保存时区的信息和说明，请参阅 [如何：将时区保存到嵌入的资源](save-time-zones-to-an-embedded-resource.md)。

### <a name="to-deserialize-a-timezoneinfo-object-from-an-embedded-resource"></a>反序列化嵌入资源中的 TimeZoneInfo 对象

1. 如果要检索的时区不是自定义时区，请尝试使用方法对其进行实例化 <xref:System.TimeZoneInfo.FindSystemTimeZoneById%2A> 。

2. <xref:System.Resources.ResourceManager>通过传递嵌入资源文件的完全限定名和对包含资源文件的程序集的引用来实例化对象。

   如果无法确定嵌入资源文件的完全限定名称，请使用 [Ildasm.exe (IL 拆装器) ](../../framework/tools/ildasm-exe-il-disassembler.md) 检查程序集的清单。 一 `.mresource` 项标识资源。 在此示例中，该资源的完全限定名为 `SerializeTimeZoneData.SerializedTimeZones` 。

   如果资源文件嵌入到包含时区实例化代码的同一程序集中，则可以通过调用 `static` `Shared` Visual Basic) 方法中的 (来检索对它的引用 <xref:System.Reflection.Assembly.GetExecutingAssembly%2A> 。

3. 如果对方法的调用 <xref:System.TimeZoneInfo.FindSystemTimeZoneById%2A> 失败，或者如果要实例化自定义时区，则通过调用方法来检索包含序列化时区的字符串 <xref:System.Resources.ResourceManager.GetString%2A?displayProperty=nameWithType> 。

4. 通过调用方法反序列化时区数据 <xref:System.TimeZoneInfo.FromSerializedString%2A> 。

## <a name="example"></a>示例

下面的示例对 <xref:System.TimeZoneInfo> 嵌入的 .NET XML 资源文件中存储的对象进行反序列化。

[!code-csharp[TimeZone2.Serialization#3](../../../samples/snippets/csharp/VS_Snippets_CLR/TimeZone2.Serialization/cs/SerializeTimeZoneData.cs#3)]
[!code-vb[TimeZone2.Serialization#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/TimeZone2.Serialization/vb/SerializeTimeZoneData.vb#3)]

此代码演示异常处理，以确保 <xref:System.TimeZoneInfo> 应用程序所需的对象存在。 它首先尝试 <xref:System.TimeZoneInfo> 使用方法通过从注册表中检索来实例化对象 <xref:System.TimeZoneInfo.FindSystemTimeZoneById%2A> 。 如果无法实例化时区，则代码会从嵌入的资源文件中检索该时区。

由于自定义时区的数据 (使用方法) 实例化的时区 <xref:System.TimeZoneInfo.CreateCustomTimeZone%2A> 未存储在注册表中，因此代码不会调用 <xref:System.TimeZoneInfo.FindSystemTimeZoneById%2A> 来实例化 Palmer，南极洲的时区。 相反，它会在调用方法之前，立即查看嵌入的资源文件以检索包含时区数据的字符串 <xref:System.TimeZoneInfo.FromSerializedString%2A> 。

## <a name="compiling-the-code"></a>编译代码

此示例需要：

- 对 System.Windows.Forms.dll 和 System.Core.dll 的引用将添加到项目。

- 导入以下命名空间：

  [!code-csharp[TimeZone2.Serialization#2](../../../samples/snippets/csharp/VS_Snippets_CLR/TimeZone2.Serialization/cs/SerializeTimeZoneData.cs#2)]
  [!code-vb[TimeZone2.Serialization#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/TimeZone2.Serialization/vb/SerializeTimeZoneData.vb#2)]

## <a name="see-also"></a>另请参阅

- [日期、时间和时区](index.md)
- [时区概述](time-zone-overview.md)
- [如何：将时区保存到嵌入的资源](save-time-zones-to-an-embedded-resource.md)
