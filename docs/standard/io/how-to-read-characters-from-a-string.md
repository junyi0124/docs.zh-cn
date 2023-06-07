---
title: 如何：从字符串中读取字符
description: 了解如何在 .NET 中从字符串读取字符。 查看同步和异步读取字符的示例。
ms.date: 01/21/2019
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- reading characters from strings
- data streams, reading characters from string
- I/O [.NET], reading characters from strings
- reading data, strings
- streams, reading characters from string
ms.assetid: 27ea5e52-6db8-42d8-980a-50bcfc7fd270
ms.openlocfilehash: beb3f4f38a5c28d19eff6fece5a6bb3c4e7a9c48
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734564"
---
# <a name="how-to-read-characters-from-a-string"></a>如何：从字符串中读取字符

下面的代码示例展示了如何从字符串中异步或同步读取字符。  
  
## <a name="example-read-characters-synchronously"></a>示例：同步读取字符

 此示例从字符串中同步读取 13 个字符，将它们存储到数组中，并显示这些字符。 然后，示例将读取字符串中的剩余字符，将它们存储到数组中（从第六个元素开始），并显示数组的内容。  
  
 [!code-csharp[Conceptual.StringReader#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.stringreader/cs/source.cs#1)]
 [!code-vb[Conceptual.StringReader#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.stringreader/vb/source.vb#1)]  
  
## <a name="example-read-characters-asynchronously"></a>示例：异步读取字符  

 下一个示例是 WPF 应用背后的代码。 在窗口加载时，示例从 <xref:System.Windows.Controls.TextBox> 控件异步读取所有字符，并将其存储在数组中。 随后，它以异步方式将每个字母或空格字符写入单独的 <xref:System.Windows.Controls.TextBlock> 控件行。  
  
 [!code-csharp[Conceptual.StringReader#2](../../../samples/snippets/csharp/VS_Snippets_Wpf/StringReaderWriter/MainWindow.xaml.cs)]
 [!code-vb[Conceptual.StringReader#2](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/StringReaderWriter/MainWindow.xaml.vb)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.IO.StringReader>  
- <xref:System.IO.StringReader.Read%2A?displayProperty=nameWithType>  
- [异步文件 I/O](asynchronous-file-i-o.md)  
- [如何：创建目录列表](/previous-versions/dotnet/netframework-4.0/5cf8zcfh(v=vs.100))  
- [如何：对新建的数据文件进行读取和写入](how-to-read-and-write-to-a-newly-created-data-file.md)  
- [如何：打开并追加到日志文件](how-to-open-and-append-to-a-log-file.md)  
- [如何：从文件中读取文本](how-to-read-text-from-a-file.md)  
- [如何：将文本写入文件](how-to-write-text-to-a-file.md)  
- [如何：向字符串写入字符](how-to-write-characters-to-a-string.md)  
- [文件和流 I/O](index.md)
