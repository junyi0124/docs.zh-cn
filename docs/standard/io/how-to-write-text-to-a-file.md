---
title: 如何：将文本写入文件
description: 了解将文本写入或追加到 .NET 应用文件的方式。 使用 StreamWriter 或文件类中的方法以同步方式或异步方式写入文本。
ms.date: 01/04/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- writing text to files
- I/O [.NET], writing text to files
- streams, writing text to files
- data streams, writing text to files
ms.assetid: 060cbe06-2adf-4337-9e7b-961a5c840208
ms.openlocfilehash: bd9abc2aac4b02f71e41f71e518a76b2477420da
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94829708"
---
# <a name="how-to-write-text-to-a-file"></a>如何：将文本写入文件

本主题介绍将文本写入 .NET 应用文件的不同方法。

下面的类和方法通常用于将文本写入文件：  
  
- <xref:System.IO.StreamWriter> 包含同步写入文件的方法（<xref:System.IO.StreamWriter.Write%2A> 或 <xref:System.IO.TextWriter.WriteLine%2A>）或者异步写入文件的方法（<xref:System.IO.StreamWriter.WriteAsync%2A> 和 <xref:System.IO.StreamWriter.WriteLineAsync%2A>）。  
  
- <xref:System.IO.File> 提供了将文本写入文件的静态方法（例如，<xref:System.IO.File.WriteAllLines%2A> 和 <xref:System.IO.File.WriteAllText%2A>），或者向文件中追加文本的静态方法（例如，<xref:System.IO.File.AppendAllLines%2A>、<xref:System.IO.File.AppendAllText%2A> 和 <xref:System.IO.File.AppendText%2A>）。  
  
- <xref:System.IO.Path> 适用于包含文件或目录路径信息的字符串。 它包含 <xref:System.IO.Path.Combine%2A> 方法，在 .NET Core 2.1 及更高版本中包含 <xref:System.IO.Path.Join%2A> 和 <xref:System.IO.Path.TryJoin%2A> 方法，允许串联字符串以生成文件或目录路径。

> [!NOTE]
> 以下示例仅显示所需的最少代码量。 实际的应用通常提供更可靠的错误检查和异常处理。  
  
## <a name="example-synchronously-write-text-with-streamwriter"></a>示例：使用 StreamWriter 同步写入文本

以下示例演示如何使用 <xref:System.IO.StreamWriter> 类，一次一行同步地将文本写入新文件。 因为在 <xref:System.IO.StreamWriter> 语句中已声明并实例化 `using` 对象，所以会调用自动刷新并关闭流的 <xref:System.IO.StreamWriter.Dispose%2A> 方法。  

[!code-csharp[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.basicio.textfiles/cs/write.cs)]
[!code-vb[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.basicio.textfiles/vb/write.vb)]  

[!INCLUDE [localized code comments](../../../includes/code-comments-loc.md)]

## <a name="example-synchronously-append-text-with-streamwriter"></a>示例：使用 StreamWriter 同步追加文本

以下示例演示如何使用 <xref:System.IO.StreamWriter> 类以同步方式将文本追加到第一个示例中创建的文本文件。

[!code-csharp[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.basicio.textfiles/cs/append.cs)]
[!code-vb[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.basicio.textfiles/vb/append.vb)]  

## <a name="example-asynchronously-write-text-with-streamwriter"></a>示例：使用 StreamWriter 异步写入文本

下面的示例演示如何使用 <xref:System.IO.StreamWriter> 类异步地将文本写入新文件。 要调用 <xref:System.IO.StreamWriter.WriteAsync%2A> 方法，方法调用必须在 `async` 方法内。 C# 示例需要 C# 7.1或更高版本，这会在对程序入口点上增加对 `async` 修饰符的支持。

[!code-csharp[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.basicio.textfiles/cs/async.cs)]
[!code-vb[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.basicio.textfiles/vb/async.vb)]  

## <a name="example-write-and-append-text-with-the-file-class"></a>示例：使用文件类编写和追加文本

下面的示例演示如何使用 <xref:System.IO.File> 类将文本写入新文件并将新的文本行追加到同一文件。 <xref:System.IO.File.WriteAllText%2A> 和 <xref:System.IO.File.AppendAllLines%2A> 方法会自动打开和关闭文件。 如果提供给 <xref:System.IO.File.WriteAllText%2A> 方法的路径已存在，则覆盖该文件。  

[!code-csharp[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.basicio.textfiles/cs/file.cs)]
[!code-vb[Conceptual.BasicIO.TextFiles#WriteLine](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.basicio.textfiles/vb/file.vb)]  

## <a name="see-also"></a>请参阅

- <xref:System.IO.StreamWriter>
- <xref:System.IO.Path>
- <xref:System.IO.File.CreateText%2A?displayProperty=nameWithType>
- [如何：枚举目录和文件](how-to-enumerate-directories-and-files.md)
- [如何：对新建的数据文件进行读取和写入](how-to-read-and-write-to-a-newly-created-data-file.md)
- [如何：打开并追加到日志文件](how-to-open-and-append-to-a-log-file.md)
- [如何：从文件中读取文本](how-to-read-text-from-a-file.md)
- [文件和流 I/O](index.md)
