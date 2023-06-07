---
title: 撰写流
ms.date: 01/21/2019
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- streams, base streams
- I/O [.NET], composing streams
- Stream class, composing streams
- base streams
- streams, backing stores
ms.assetid: da761658-a535-4f26-a452-b30df47f73d5
ms.openlocfilehash: d848a989378ed40df794554f3a0a9a7f135fbd4e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95732185"
---
# <a name="compose-streams"></a>撰写流

后备存储是磁盘或内存等存储介质  。 各个后备存储都实现自己的流，作为 <xref:System.IO.Stream> 类的实现。

每个流类型对给定的后备存储执行字节读取和写入操作。 连接到后备存储的流称为“基流”  。 基流的构造函数包含将流连接到后备存储所需的参数。 例如，<xref:System.IO.FileStream> 包含指定路径参数的构造函数，此参数指定了进程如何共享文件。  

<xref:System.IO> 类旨在简化流撰写。 可以将基流附加到一个或多个提供所需功能的直通流。 可以将读取器或编写器附加到链的末尾，这样便能轻松读取或编写首选类型。  

下面的代码示例根据现有 MyFile.txt 创建了 FileStream，以便缓冲 MyFile.txt    。 请注意，FileStreams 默认缓冲  。

>[!IMPORTANT]
>这些示例假定名为 MyFile.txt 的文件已存在于与应用相同的文件夹中  。  

## <a name="example-use-streamreader"></a>示例：使用 StreamReader

以下示例将创建从 FileStream 读取字符的 <xref:System.IO.StreamReader>，此读取器作为构造函数参数传递给 StreamReader   。 除非 <xref:System.IO.StreamReader.Peek%2A?displayProperty=nameWithType> 找不到更多字符，否则 <xref:System.IO.StreamReader.ReadLine%2A?displayProperty=nameWithType> 会一直读取字符。  
  
 [!code-csharp[System.IO.StreamReader#20](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.IO.StreamReader/CS/source2.cs#20)]
 [!code-vb[System.IO.StreamReader#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.IO.StreamReader/VB/source2.vb#20)]  
  
## <a name="example-use-binaryreader"></a>示例：使用 BinaryReader

以下示例将创建从 FileStream 读取字节的 <xref:System.IO.BinaryReader>，此读取器作为构造函数参数传递给 BinaryReader   。 除非 <xref:System.IO.BinaryReader.PeekChar%2A> 找不到更多字节，否则 <xref:System.IO.BinaryReader.ReadByte%2A> 会一直读取字节。  
  
 [!code-csharp[System.IO.StreamReader#21](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.IO.StreamReader/CS/source3.cs#21)]
 [!code-vb[System.IO.StreamReader#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.IO.StreamReader/VB/source3.vb#21)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.IO.StreamReader>
- <xref:System.IO.StreamReader.ReadLine%2A?displayProperty=nameWithType>
- <xref:System.IO.StreamReader.Peek%2A?displayProperty=nameWithType>
- <xref:System.IO.FileStream>
- <xref:System.IO.BinaryReader>
- <xref:System.IO.BinaryReader.ReadByte%2A?displayProperty=nameWithType>
- <xref:System.IO.BinaryReader.PeekChar%2A?displayProperty=nameWithType>
