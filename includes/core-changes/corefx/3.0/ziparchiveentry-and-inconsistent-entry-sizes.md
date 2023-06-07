---
ms.openlocfilehash: 8c8e87c885c99d28aa9a7a5d5a2b48c80d40d7db
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032042"
---
### <a name="ziparchiveentry-no-longer-handles-archives-with-inconsistent-entry-sizes"></a>ZipArchiveEntry 不再处理具有不一致条目大小的存档

Zip 存档在中央目录和本地标头中列出压缩的大小和未压缩的大小。  条目数据本身还指示其大小。  在 .NET Core 2.2 及更早版本中，永远不会对这些值进行一致性检查。 从 .NET Core 3.0 开始，对它们进行一致性检查。

#### <a name="change-description"></a>更改描述

在 .NET Core 2.2 及更早版本中，即使本地标头与 zip 文件的中央标头不一致，<xref:System.IO.Compression.ZipArchiveEntry.Open?displayProperty=nameWithType> 也会成功。 即使数据长度超出了中央目录/本地标头中列出的未压缩文件大小，数据也会一直进行解压缩，直到达到压缩流的末尾。

从 .NET Core 3.0 开始，<xref:System.IO.Compression.ZipArchiveEntry.Open?displayProperty=nameWithType> 方法会检查本地标头和中央标头是否在条目的压缩大小和未压缩大小方面保持一致。  如果不是这样，则该方法在存档的本地标头和/或数据描述符列表大小与 zip 文件的中央目录不一致时会引发 <xref:System.IO.InvalidDataException>。 当读取条目时，解压缩的数据将被截断为标头中列出的未压缩文件大小。

进行此更改是为了确保 <xref:System.IO.Compression.ZipArchiveEntry> 正确表示其数据的大小并且只读取该数据量。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="recommended-action"></a>建议操作

对出现这些问题的所有 zip 存档重新打包。

#### <a name="category"></a>类别

CoreFx

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.IO.Compression.ZipArchiveEntry.Open?displayProperty=nameWithType>
- <xref:System.IO.Compression.ZipFileExtensions.ExtractToDirectory%2A?displayProperty=nameWithType>
- <xref:System.IO.Compression.ZipFileExtensions.ExtractToFile%2A?displayProperty=nameWithType>
- <xref:System.IO.Compression.ZipFile.ExtractToDirectory%2A?displayProperty=nameWithType>

<!--

#### Affected APIs

`M:System.IO.Compression.ZipArchiveEntry.Open`
`Overload:System.IO.Compression.ZipFileExtensions.ExtractToDirectory%2A`
`Overload:System.IO.Compression.ZipFileExtensions.ExtractToFile%2A`
`Overload:System.IO.Compression.ZipFile.ExtractToDirectory%2A`

-->
