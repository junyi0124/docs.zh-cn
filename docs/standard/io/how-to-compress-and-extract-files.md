---
title: 如何：压缩和解压缩文件
description: 使用 System.IO.Compression 压缩和提取文件。 请参阅使用 ZipFile、ZipArchive、ZipArchiveEntry、DeflateStream 和 GZipStream 的示例。
ms.date: 01/14/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- I/O [.NET], compression
- compression
- compress files
ms.assetid: e9876165-3c60-4c84-a272-513e47acf579
ms.openlocfilehash: a1077c7277e0aa54e3c8883cfc27d93926485b8e
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94830852"
---
# <a name="how-to-compress-and-extract-files"></a>如何：压缩和解压缩文件

<xref:System.IO.Compression> 命名空间包含以下类型来对文件和流进行压缩或解压缩。 还可以使用这些类型来读取和修改压缩文件的内容。

- <xref:System.IO.Compression.ZipFile>
- <xref:System.IO.Compression.ZipArchive>
- <xref:System.IO.Compression.ZipArchiveEntry>
- <xref:System.IO.Compression.DeflateStream>
- <xref:System.IO.Compression.GZipStream>

下面的示例演示使用压缩文件可以执行的某些操作。 这些示例需要将以下 NuGet 包添加到项目中：

- [System.IO.Compression](https://www.nuget.org/packages/System.IO.Compression)
- [System.IO.Compression.ZipFile](https://www.nuget.org/packages/System.IO.Compression.ZipFile)

如果你使用的是 .NET Framework，请将对这两个库的引用添加到项目中：

- `System.IO.Compression`
- `System.IO.Compression.FileSystem`

## <a name="example-1-create-and-extract-a-zip-file"></a>示例 1：创建和提取 .zip 文件

以下示例演示如何使用 <xref:System.IO.Compression.ZipFile> 类创建和提取压缩的 .zip 文件。 该示例将文件夹的内容压缩为一个新的 .zip 文件，然后将该 .zip 提取到一个新文件夹。

若要运行示例，请在程序文件夹中创建 start 文件夹，然后在其中放入要压缩的文件。

[!code-csharp[System.IO.Compression.ZipFile#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.compression.zipfile/cs/program1.cs#1)]
[!code-vb[System.IO.Compression.ZipFile#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.compression.zipfile/vb/program1.vb#1)]

## <a name="example-2-extract-specific-file-extensions"></a>示例 2：提取特定文件扩展名

下一示例循环访问现有 .zip 文件的内容并提取扩展名为 .txt 的文件 。 它使用 <xref:System.IO.Compression.ZipArchive> 类访问 zip，使用 <xref:System.IO.Compression.ZipArchiveEntry> 类检查各个条目。 适用于 <xref:System.IO.Compression.ZipFileExtensions.ExtractToFile%2A> 对象的扩展方法 <xref:System.IO.Compression.ZipArchiveEntry> 可以在 <xref:System.IO.Compression.ZipFileExtensions?displayProperty=nameWithType> 类中使用。

若要运行示例，请将名为 result.zip 的 .zip 文件放到程序文件夹中 。 出现提示时，提供要提取到的文件夹名称。

> [!IMPORTANT]
> 在解压缩文件时，必须查找可以转义出你想要解压缩到的目录的恶意文件路径。 这被称为“路径遍历攻击”。 下面的示例演示如何检查恶意文件路径，并提供一种安全的解压缩方法。

[!code-csharp[System.IO.Compression.ZipArchive#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.compression.ziparchive/cs/program1.cs#1)]
[!code-vb[System.IO.Compression.ZipArchive#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.compression.ziparchive/vb/program1.vb#1)]

## <a name="example-3-add-a-file-to-an-existing-zip"></a>示例 3：将文件添加到现有 zip

以下示例使用 <xref:System.IO.Compression.ZipArchive> 类访问现有的 .zip 文件，然后向其添加文件。 当将其添加到现有的 zip 时，会对新文件进行压缩。

[!code-csharp[System.IO.Compression.ZipArchiveMode#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.compression.ziparchivemode/cs/program1.cs#1)]
[!code-vb[System.IO.Compression.ZipArchiveMode#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.compression.ziparchivemode/vb/program1.vb#1)]

## <a name="example-4-compress-and-decompress-gz-files"></a>示例 4：压缩和解压缩 .gz 文件

您也可以使用 <xref:System.IO.Compression.GZipStream> 和 <xref:System.IO.Compression.DeflateStream> 类压缩和解压缩数据。 它们使用相同的压缩算法。 可以使用许多常用工具解压缩写入 .gz 文件的 <xref:System.IO.Compression.GZipStream> 对象。 以下示例展示了如何使用 <xref:System.IO.Compression.GZipStream> 类压缩和解压缩文件目录：

[!code-csharp[IO.Compression.GZip1#1](../../../samples/snippets/csharp/VS_Snippets_CLR/IO.Compression.GZip1/CS/gziptest.cs#1)]
[!code-vb[IO.Compression.GZip1#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/IO.Compression.GZip1/VB/gziptest.vb#1)]

## <a name="see-also"></a>请参阅

- <xref:System.IO.Compression.ZipArchive>  
- <xref:System.IO.Compression.ZipFile>  
- <xref:System.IO.Compression.ZipArchiveEntry>  
- <xref:System.IO.Compression.DeflateStream>  
- <xref:System.IO.Compression.GZipStream>  
- [文件和流 I/O](index.md)
