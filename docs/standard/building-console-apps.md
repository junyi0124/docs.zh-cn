---
title: 在 .NET 中创建控制台应用程序
ms.date: 03/30/2017
helpviewer_keywords:
- .NET, creating console applications
- application development [.NET], console
- console applications
ms.assetid: c21fb997-9f0e-40a5-8741-f73bba376bd8
ms.openlocfilehash: 8046b1b8cb50476860fee53654de93c924d23346
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94823967"
---
# <a name="console-apps-in-net"></a>.NET 中的控制台应用

.NET 应用程序可以使用 <xref:System.Console?displayProperty=nameWithType> 类在控制台中读取和写入字符。 读取自控制台的数据是从标准输入流读取的，而写入到控制台的数据将写入标准输出流，并且写入控制台的错误数据将写入标准错误输出流。 应用程序启动时，这些数据流会自动与控制台关联，并分别表示为 <xref:System.Console.In%2A>、<xref:System.Console.Out%2A> 和 <xref:System.Console.Error%2A> 属性。

<xref:System.Console.In%2A?displayProperty=nameWithType> 属性的值是 <xref:System.IO.TextReader?displayProperty=nameWithType> 对象，而 <xref:System.Console.Out%2A?displayProperty=nameWithType> 和 <xref:System.Console.Error%2A?displayProperty=nameWithType> 属性的值都是 <xref:System.IO.TextWriter?displayProperty=nameWithType> 对象。 你可以将这些属性与不表示控制台的流关联，以便可以将该流指向不同的输入位置或输出位置。 例如，你可以通过将 <xref:System.Console.Out%2A?displayProperty=nameWithType> 属性设置为 <xref:System.IO.StreamWriter?displayProperty=nameWithType> 将输出重定向到一个文件，这将通过 <xref:System.Console.SetOut%2A?displayProperty=nameWithType> 方法封装 <xref:System.IO.FileStream?displayProperty=nameWithType>。 <xref:System.Console.In%2A?displayProperty=nameWithType> 和 <xref:System.Console.Out%2A?displayProperty=nameWithType> 属性不需要引用相同流。

> [!NOTE]
> 有关生成控制台应用程序（包括 C#、Visual Basic 和 C++ 中的示例）的详细信息，请参阅 <xref:System.Console> 类文档。

如果不存在控制台（比如在 Windows 窗体应用程序中），写入标准输出流的输出将不可见，因为没有可以将信息写入的控制台。 将信息写入不可访问的控制台不会引发异常。 （例如在 Visual Studio 的项目属性页中，你始终可以将应用程序类型更改为“控制台应用程序”）。

System.Console  类具有从控制台读取单独的字符或整行的方法。 其他方法转换数据和格式字符串，然后将设置了格式的字符串写入控制台。 有关设置字符串格式的详细信息，请参阅[格式设置类型](base-types/formatting-types.md)。

> [!TIP]
> 控制台应用程序缺少在默认情况下启动的消息泵。 因此，控制台调用 Microsoft Win32 计时器时可能会失败。

## <a name="see-also"></a>另请参阅

- <xref:System.Console?displayProperty=nameWithType>
- [格式设置类型](base-types/formatting-types.md)
