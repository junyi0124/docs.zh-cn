---
title: 如何：在 .NET Framework 和 Windows 运行时流之间进行转换（仅限 Windows）
ms.date: 01/14/2019
dev_langs:
- csharp
- vb
ms.assetid: 23a763ea-8348-4244-9f8c-a4280b870b47
ms.openlocfilehash: c2ab384b6d84a14d5e8f4cfe66281a26b14421f4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95726959"
---
# <a name="how-to-convert-between-net-framework-and-windows-runtime-streams-windows-only"></a>如何：在 .NET Framework 和 Windows 运行时流之间进行转换（仅限 Windows）

适用于 UWP 应用的 .NET Framework 是完整的 .NET Framework 的子集。 由于 UWP 应用的安全性和其他要求，你无法使用整套 .NET Framework API 来打开和读取文件。 有关详细信息，请参阅[适用于 UWP 应用的 .NET 概述](/previous-versions/windows/apps/br230302(v=vs.140))。 但是，你可能需要将 .NET Framework API 用于其他流处理操作。 若要操作这些流，可以在 .NET Framework 流类型（如 <xref:System.IO.MemoryStream> 或 <xref:System.IO.FileStream>）和 Windows 运行时流（如 <xref:Windows.Storage.Streams.IInputStream>、<xref:Windows.Storage.Streams.IOutputStream> 或 <xref:Windows.Storage.Streams.IRandomAccessStream>）之间进行转换。

<xref:System.IO.WindowsRuntimeStreamExtensions?displayProperty=nameWithType> 类包含简化这些转换的方法。 但是，.NET Framework 与 Windows 运行时流之间存在一些基本差异，这将影响使用这些方法所获得的结果，如以下部分中所述：

## <a name="convert-from-a-windows-runtime-to-a-net-framework-stream"></a>从 Windows 运行时流转换为 .NET Framework 流

若要从 Windows 运行时流转换为 .NET Framework 流，请使用以下 <xref:System.IO.WindowsRuntimeStreamExtensions?displayProperty=nameWithType> 方法之一：

- <xref:System.IO.WindowsRuntimeStreamExtensions.AsStream%2A?displayProperty=nameWithType> 将 Windows 运行时中的随机访问流转换为 .NET 中适用于 UWP 应用的托管流。
  
- <xref:System.IO.WindowsRuntimeStreamExtensions.AsStreamForWrite%2A?displayProperty=nameWithType> 将 Windows 运行时中的输出流转换为 .NET 中为 UWP 应用的托管流。
  
- <xref:System.IO.WindowsRuntimeStreamExtensions.AsStreamForRead%2A?displayProperty=nameWithType> 将 Windows 运行时中的输入流转换为 .NET 中为 UWP 应用的托管流。

Windows 运行时提供支持只读、只写或读写的流类型。 如果将 Windows 运行时流转换为 .NET Framework 流，这些功能将保留。 此外，如果你在 Windows 运行时流与 .NET Framework 流之间转换，则将取回原始 Windows 运行时实例。

最佳做法是使用与要转换的 Windows 运行时流的功能匹配的转换方法。 但是，由于 <xref:Windows.Storage.Streams.IRandomAccessStream> 是可读写的（它同时实现了 <xref:Windows.Storage.Streams.IOutputStream> 和 <xref:Windows.Storage.Streams.IInputStream>），因此转换方法将保留原始流的功能。 例如，使用 <xref:System.IO.WindowsRuntimeStreamExtensions.AsStreamForRead%2A?displayProperty=nameWithType> 转换 <xref:Windows.Storage.Streams.IRandomAccessStream> 不会限制转换的 .NET Framework 流的可读性。 它也是可写的。

## <a name="example-convert-windows-runtime-random-access-to-net-framework-stream"></a>示例：将 Windows 运行时随机访问流转换为 .NET Framework 流

若要从 Windows 运行时随机访问流转换为 .NET Framework 流，请使用 <xref:System.IO.WindowsRuntimeStreamExtensions.AsStream%2A?displayProperty=nameWithType> 方法。

下面的代码示例会提示用户选择文件，使用 Windows 运行时 API 将其打开，然后将其转换为 .NET Framework 流。 它可读取流，并将其输出为文本块。 在输出结果之前，通常使用 .NET Framework API 操作流。

若要运行此示例，请创建包含一个名为 `TextBlock1` 的文本块和一个名为 `Button1` 的按钮的 UWP XAML 应用。 将按钮单击事件与此示例中所示的 `button1_Click` 方法关联。

  [!code-csharp[System.IO.WindowsRuntimeStreamExtensionsEx#Imports](~/samples/snippets/csharp/VS_Snippets_CLR_System/system.io.windowsruntimestreamextensionsex/cs/mainpage1.xaml.cs)]
  [!code-vb[System.IO.WindowsRuntimeStreamExtensionsEx#Imports](~/samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.windowsruntimestreamextensionsex/vb/mainpage1.xaml.vb)]

## <a name="convert-from-a-net-framework-to-a-windows-runtime-stream"></a>从 .NET Framework 转换为 Windows 运行时流

若要从.NET Framework 流转换为 Windows 运行时流，请使用下列任一 <xref:System.IO.WindowsRuntimeStreamExtensions?displayProperty=nameWithType> 方法：

- <xref:System.IO.WindowsRuntimeStreamExtensions.AsInputStream%2A?displayProperty=nameWithType> 将 .NET 中适用于为 UWP 应用的托管流转换为 Windows 运行时中的输入流。
  
- <xref:System.IO.WindowsRuntimeStreamExtensions.AsOutputStream%2A?displayProperty=nameWithType> 将 .NET 中适用于 UWP 应用的托管流转换为 Windows 运行时中的输出流。
  
- <xref:System.IO.WindowsRuntimeStreamExtensions.AsRandomAccessStream%2A?displayProperty=nameWithType> 将适用于 UWP 应用的 .NET 中的托管流转换为 Windows 运行时可用于读取或写入的随机访问流。

在将 .NET Framework 流转换为 Windows 运行时流时，转换后的流的功能将取决于原始流。 例如，如果原始流支持读取和写入，并且你调用 <xref:System.IO.WindowsRuntimeStreamExtensions.AsInputStream%2A?displayProperty=nameWithType> 来转换流，则返回的类型为 `IRandomAccessStream`。 `IRandomAccessStream` 可实现 `IInputStream` 和 `IOutputStream`，且支持读取和写入。

.NET Framework 流不支持克隆，即使转换后也是如此。 如果将 .NET Framework 流转换为 Windows 运行时流，并调用 <xref:Windows.Storage.Streams.InMemoryRandomAccessStream.GetInputStreamAt%2A> 或 <xref:Windows.Storage.Streams.IRandomAccessStream.GetOutputStreamAt%2A>（这会直接调用 <xref:Windows.Storage.Streams.RandomAccessStreamOverStream.CloneStream%2A> 或 <xref:Windows.Storage.Streams.RandomAccessStreamOverStream.CloneStream%2A>），则会引发异常。

## <a name="example-convert-net-framework-to-windows-runtime-random-access-stream"></a>示例：将 .NET Framework 转换为 Windows 运行时随机访问流

要从 .NET Framework 流转换为 Windows 运行时随机访问流，请使用 <xref:System.IO.WindowsRuntimeStreamExtensions.AsRandomAccessStream%2A> 方法，如以下示例所示：

> [!IMPORTANT]
> 确保所使用的 .NET Framework 流支持查找或将其复制到支持查找的流。 可使用 <xref:System.IO.Stream.CanSeek%2A?displayProperty=nameWithType> 属性来确定这一点。

若要运行此示例，请创建一个面向 .NET Framework 4.5.1 的且包含一个名为 `TextBlock2` 的文本块和一个名为 `Button2`的按钮的 UWP XAML 应用。 将按钮单击事件与此示例中所示的 `button2_Click` 方法关联。

  [!code-csharp[System.IO.WindowsRuntimeStreamExtensionsEx#Imports](~/samples/snippets/csharp/VS_Snippets_CLR_System/system.io.windowsruntimestreamextensionsex/cs/mainpage2.xaml.cs)]
  [!code-vb[System.IO.WindowsRuntimeStreamExtensionsEx#Imports](~/samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.windowsruntimestreamextensionsex/vb/mainpage2.xaml.vb)]

## <a name="see-also"></a>请参阅

- [快速入门：读取和写入文件 (Windows)](/previous-versions/windows/apps/hh464978(v=win.10))  
- [适用于 Microsoft Store 应用的 .NET 概述](/previous-versions/windows/apps/br230302(v=vs.140))  
- [适用于 Windows 应用商店应用的 .NET API](/previous-versions/br230232(v=vs.120))
