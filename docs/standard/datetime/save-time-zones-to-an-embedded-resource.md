---
title: 如何：将时区保存到嵌入的资源中
ms.date: 04/10/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- time zones [.NET], saving
- time zone objects [.NET], serializing
- time zone objects [.NET], saving
ms.assetid: 3c96d83a-a057-4496-abb0-8f4b12712558
ms.openlocfilehash: 23f86076b2858404f3dbc900d8c40a6509abe8db
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94817596"
---
# <a name="how-to-save-time-zones-to-an-embedded-resource"></a>如何：将时区保存到嵌入的资源中

时区感知应用程序通常要求存在特定时区。 但是，由于单个对象的可用性 <xref:System.TimeZoneInfo> 取决于存储在本地系统的注册表中的信息，因此可能不存在通常可用的时区。 此外，通过使用方法实例化的自定义时区的相关信息 <xref:System.TimeZoneInfo.CreateCustomTimeZone%2A> 不会与注册表中的其他时区信息一起存储。 若要确保这些时区在需要时可用，可以通过序列化它们来保存，然后通过反序列化它们来还原它们。

通常，序列化 <xref:System.TimeZoneInfo> 对象的过程与时区感知应用程序分离。 根据用于保存序列化对象的数据存储区 <xref:System.TimeZoneInfo> ，时区数据可以作为安装或安装例程的一部分进行序列化 (例如，将数据存储在注册表) 的应用程序密钥中，或作为在编译最终应用程序之前运行的实用工具例程的一部分 (例如，当将序列化数据存储在 .NET XML 资源 ( .resx) 文件) 时。

除了使用应用程序编译的资源文件外，还可以使用多个其他数据存储区来存储时区信息。 这些功能包括以下这些：

- 注册表。 请注意，应用程序应使用其自己的应用程序密钥的子项来存储自定义时区数据，而不是使用 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Time Zones 的子项。

- 配置文件。

- 其他系统文件。

### <a name="to-save-a-time-zone-by-serializing-it-to-a-resx-file"></a>通过将时区序列化到 .resx 文件来保存时区

1. 检索现有时区或创建新时区。

   若要检索现有时区，请参阅 [如何：访问预定义的 UTC 和本地时区对象](access-utc-and-local.md) 和 [如何：实例化 TimeZoneInfo 对象](instantiate-time-zone-info.md)。

   若要创建新的时区，请调用方法的重载之一 <xref:System.TimeZoneInfo.CreateCustomTimeZone%2A> 。 有关详细信息，请参阅 [如何：创建不带调整规则的时区](create-time-zones-without-adjustment-rules.md) 和 [如何：创建带有调整规则的](create-time-zones-with-adjustment-rules.md)时区。

2. 调用 <xref:System.TimeZoneInfo.ToSerializedString%2A> 方法以创建包含时区数据的字符串。

3. <xref:System.IO.StreamWriter>通过向 <xref:System.IO.StreamWriter> 类构造函数提供 .resx 文件的名称和（可选）路径来实例化对象。

4. <xref:System.Resources.ResXResourceWriter>通过 <xref:System.IO.StreamWriter> 将对象传递给类构造函数来实例化对象 <xref:System.Resources.ResXResourceWriter> 。

5. 将时区的序列化字符串传递给 <xref:System.Resources.ResXResourceWriter.AddResource%2A?displayProperty=nameWithType> 方法。

6. 调用 <xref:System.Resources.ResXResourceWriter.Generate%2A?displayProperty=nameWithType> 方法。

7. 调用 <xref:System.Resources.ResXResourceWriter.Close%2A?displayProperty=nameWithType> 方法。

8. <xref:System.IO.StreamWriter>通过调用其方法关闭对象 <xref:System.IO.StreamWriter.Close%2A> 。

9. 将生成的 .resx 文件添加到应用程序的 Visual Studio 项目中。

10. 使用 Visual Studio 中的 " **属性** " 窗口，确保将 .resx 文件的 " **生成操作** " 属性设置为 " **嵌入的资源**"。

## <a name="example"></a>示例

下面的示例序列化一个 <xref:System.TimeZoneInfo> 表示中部标准时间的对象，以及一个对象，该对象表示 <xref:System.TimeZoneInfo> Palmer 站，南极洲 Time 为一个名为 SerializedTimeZones 的 .net XML 资源文件。 中央标准时间通常在注册表中定义;Palmer 工作站，南极洲是自定义时区。

[!code-csharp[TimeZone2.Serialization#1](../../../samples/snippets/csharp/VS_Snippets_CLR/TimeZone2.Serialization/cs/SerializeTimeZoneData.cs#1)]
[!code-vb[TimeZone2.Serialization#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/TimeZone2.Serialization/vb/SerializeTimeZoneData.vb#1)]

此示例 <xref:System.TimeZoneInfo> 对对象进行序列化，以便在编译时在资源文件中提供这些对象。

由于 <xref:System.Resources.ResXResourceWriter.Generate%2A?displayProperty=nameWithType> 方法会将完整的标头信息添加到 .NET XML 资源文件中，因此不能将其用于将资源添加到现有文件中。 该示例通过检查 SerializedTimeZones 文件来处理这种情况，如果该文件存在，则将其除两个序列化时区之外的所有资源存储到一个泛型 <xref:System.Collections.Generic.Dictionary%602> 对象。 然后删除现有的文件，并将现有资源添加到新的 SerializedTimeZones 文件。 序列化的时区数据也将添加到此文件中。

资源的键 (或 **名称**) 字段不应包含嵌入的空格。 <xref:System.String.Replace%28System.String%2CSystem.String%29>调用方法以删除时区标识符中的所有嵌入空格，然后将其分配给资源文件。

## <a name="compiling-the-code"></a>编译代码

此示例需要：

- 对 System.Windows.Forms.dll 和 System.Core.dll 的引用将添加到项目。

- 导入以下命名空间：

  [!code-csharp[TimeZone2.Serialization#2](../../../samples/snippets/csharp/VS_Snippets_CLR/TimeZone2.Serialization/cs/SerializeTimeZoneData.cs#2)]
  [!code-vb[TimeZone2.Serialization#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/TimeZone2.Serialization/vb/SerializeTimeZoneData.vb#2)]

## <a name="see-also"></a>另请参阅

- [日期、时间和时区](index.md)
- [时区概述](time-zone-overview.md)
- [如何：从嵌入的资源还原时区](restore-time-zones-from-an-embedded-resource.md)
