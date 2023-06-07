---
title: 如何：对新建的数据文件进行读取和写入
description: 了解如何使用 System.IO.BinaryReader 和 System.IO.BinaryWriter 类读取和写入 .NET 中新创建的数据文件。
ms.date: 01/21/2019
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- streams, reading and writing data
- BinaryReader class, examples
- I/O [.NET], reading data
- I/O [.NET], writing data
- BinaryWriter class, examples
ms.assetid: e209d949-31e8-44ea-8e38-87f9093f3093
ms.openlocfilehash: 8ec14ef560ddd69e8c9217298271b7ec715fcb9c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734577"
---
# <a name="how-to-read-and-write-to-a-newly-created-data-file"></a>如何：对新建的数据文件进行读取和写入

<xref:System.IO.BinaryWriter?displayProperty=nameWithType> 和 <xref:System.IO.BinaryReader?displayProperty=nameWithType> 类用于写入和读取字符串以外的数据。 下面的示例演示如何创建空文件流，向其写入数据并从中读取数据。

示例将在当前目录中创建名为 Test.data 的数据文件，也就同时创建了相关的 <xref:System.IO.BinaryWriter> 和 <xref:System.IO.BinaryReader> 对象，并且 <xref:System.IO.BinaryWriter> 对象用于向 Test.data 写入整数 0 到 10，这会将文件指针置于文件末尾 。 <xref:System.IO.BinaryReader> 对象将文件指针设置回原始位置并读取指定的内容。  
  
> [!NOTE]
> 如果当前目录中已存在 Test.data，则会引发 <xref:System.IO.IOException> 异常。 使用文件模型选项 <xref:System.IO.FileMode.Create?displayProperty=nameWithType> 而不是 <xref:System.IO.FileMode.CreateNew?displayProperty=nameWithType> 以始终创建新文件，而不引发异常。  
  
## <a name="example"></a>示例  

 [!code-csharp[System.IO.BinaryReaderWriter#7](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.IO.BinaryReaderWriter/CS/source6.cs#7)]
 [!code-vb[System.IO.BinaryReaderWriter#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.IO.BinaryReaderWriter/VB/source6.vb#7)]  
  
## <a name="see-also"></a>请参阅

- <xref:System.IO.BinaryReader>  
- <xref:System.IO.BinaryWriter>  
- <xref:System.IO.FileStream>  
- <xref:System.IO.FileStream.Seek%2A?displayProperty=nameWithType>  
- <xref:System.IO.SeekOrigin>  
- [如何：枚举目录和文件](how-to-enumerate-directories-and-files.md)  
- [如何：打开并追加到日志文件](how-to-open-and-append-to-a-log-file.md)  
- [如何：从文件中读取文本](how-to-read-text-from-a-file.md)  
- [如何：将文本写入文件](how-to-write-text-to-a-file.md)  
- [如何：从字符串中读取字符](how-to-read-characters-from-a-string.md)  
- [如何：向字符串写入字符](how-to-write-characters-to-a-string.md)  
- [文件和流 I/O](index.md)
