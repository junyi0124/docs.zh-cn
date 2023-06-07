---
title: 如何：从文件中读取文本
description: 在本文中，查看有关如何使用适用于桌面应用的 .NET 中的 StreamReader 类以同步或异步方式从文本文件中读取文本。
ms.date: 01/03/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- streams, reading text from files
- reading text files
- reading data, text files
- data streams, reading text from files
- I/O [.NET], reading text from files
ms.assetid: ed180baa-dfc6-4c69-a725-46e87edafb27
ms.openlocfilehash: 48b862ff77bf4ace48a5481fe9bedcf354b5654b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95725334"
---
# <a name="how-to-read-text-from-a-file"></a>如何：从文件中读取文本

下面的示例演示如何使用适用于桌面应用的 .NET 以异步方式和同步方式从文本文件中读取文本。 在这两个示例中，当你创建 <xref:System.IO.StreamReader> 类的实例时，你会提供文件的绝对路径或相对路径。
  
> [!NOTE]
> 这些代码示例不适用于通用 Windows (UWP) 应用，因为 Windows 运行时提供了对文件进行读写操作的不同流类型。 有关演示如何在 UWP 应用中读取文本的示例，请参阅[快速入门：对文件执行读取和写入操作](/previous-versions/windows/apps/hh758325(v=win.10))。 有关演示如何在 .NET Framework 流和 Windows 运行时流之间进行转换的示例，请参阅[如何：在 .NET Framework 流和 Windows 运行时流之间进行转换](how-to-convert-between-dotnet-streams-and-winrt-streams.md)。  
  
## <a name="example-synchronous-read-in-a-console-app"></a>示例：控制台应用中的同步读取  

以下示例演示控制台应用中的同步读取操作。 此示例使用流读取器打开文本文件，将内容复制到字符串并将字符串输出到控制台。  
  
> [!IMPORTANT]
> 该示例假定名为 TestFile.txt 的文件已存在于与应用相同的文件夹中。  

:::code language="csharp" source="snippets/how-to-read-text-from-a-file/csharp/sync-console/Program.cs":::
:::code language="vb" source="snippets/how-to-read-text-from-a-file/vb/sync-console/Program.vb":::
  
## <a name="example-asynchronous-read-in-a-wpf-app"></a>示例：WPF 应用中的异步读取

 以下示例演示 Windows Presentation Foundation (WPF) 应用中的异步读取操作。  
  
> [!IMPORTANT]
> 该示例假定名为 TestFile.txt 的文件已存在于与应用相同的文件夹中。  

:::code language="csharp" source="snippets/how-to-read-text-from-a-file/csharp/async-wpf/MainWindow.xaml.cs":::
:::code language="vb" source="snippets/how-to-read-text-from-a-file/vb/async-wpf/MainWindow.xaml.vb":::
  
## <a name="see-also"></a>请参阅

- <xref:System.IO.StreamReader>  
- <xref:System.IO.File.OpenText%2A?displayProperty=nameWithType>  
- <xref:System.IO.StreamReader.ReadLine%2A?displayProperty=nameWithType>  
- [异步文件 I/O](asynchronous-file-i-o.md)  
- [如何：创建目录列表](/previous-versions/dotnet/netframework-4.0/5cf8zcfh(v=vs.100))  
- [快速入门：读取和写入文件](/previous-versions/windows/apps/hh758325(v=win.10))  
- [如何：在 .NET Framework 流和 Windows 运行时流之间进行转换](how-to-convert-between-dotnet-streams-and-winrt-streams.md)  
- [如何：对新建的数据文件进行读取和写入](how-to-read-and-write-to-a-newly-created-data-file.md)  
- [如何：打开并追加到日志文件](how-to-open-and-append-to-a-log-file.md)  
- [如何：将文本写入文件](how-to-write-text-to-a-file.md)  
- [如何：从字符串中读取字符](how-to-read-characters-from-a-string.md)  
- [如何：向字符串写入字符](how-to-write-characters-to-a-string.md)  
- [文件和流 I/O](index.md)
