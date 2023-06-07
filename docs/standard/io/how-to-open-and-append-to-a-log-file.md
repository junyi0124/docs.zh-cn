---
title: 如何：打开并追加到日志文件
description: 使用 .NET 中的 StreamWriter 和 StreamReader 类打开并追加到日志文件，此操作会将字符写入流并从流中读取字符。
ms.date: 01/21/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- log files, opening
- streams, opening and appending to log file
- log files, appending to
- I/O [.NET], log files
ms.assetid: 74423362-1721-49cb-aa0a-e04005f72a06
ms.openlocfilehash: be4cacee8d0a529730c66c5850f42330520ba2d3
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734603"
---
# <a name="how-to-open-and-append-to-a-log-file"></a>如何：打开并追加到日志文件

<xref:System.IO.StreamWriter> 和 <xref:System.IO.StreamReader> 对流执行字符写入和读取操作。 下面的代码示例打开 log.txt 文件以供输入，或创建该文件（如果尚无文件的话），并将日志信息追加到文件末尾。 然后，示例将文件内容写入标准输出以供显示。

作为此示例的替换方法，可以将信息存储为一个字符串或字符串数组，并使用 <xref:System.IO.File.WriteAllText%2A?displayProperty=nameWithType> 或 <xref:System.IO.File.WriteAllLines%2A?displayProperty=nameWithType> 方法实现相同的功能。  
  
> [!NOTE]
> Visual Basic 用户可以选择使用 <xref:Microsoft.VisualBasic.Logging.Log> 类或 <xref:Microsoft.VisualBasic.FileIO.FileSystem> 类提供的方法和属性，以创建或写入日志文件。  
  
## <a name="example"></a>示例  

 [!code-csharp[Conceptual.BasicIO.TextFiles#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.basicio.textfiles/cs/source2.cs#2)]
 [!code-vb[Conceptual.BasicIO.TextFiles#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.basicio.textfiles/vb/source2.vb#2)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.IO.StreamWriter>  
- <xref:System.IO.StreamReader>  
- <xref:System.IO.File.AppendText%2A?displayProperty=nameWithType>  
- <xref:System.IO.File.OpenText%2A?displayProperty=nameWithType>  
- <xref:System.IO.StreamReader.ReadLine%2A?displayProperty=nameWithType>  
- [如何：枚举目录和文件](how-to-enumerate-directories-and-files.md)  
- [如何：对新建的数据文件进行读取和写入](how-to-read-and-write-to-a-newly-created-data-file.md)  
- [如何：从文件中读取文本](how-to-read-text-from-a-file.md)  
- [如何：将文本写入文件](how-to-write-text-to-a-file.md)  
- [如何：从字符串中读取字符](how-to-read-characters-from-a-string.md)  
- [如何：向字符串写入字符](how-to-write-characters-to-a-string.md)  
- [文件和流 I/O](index.md)
