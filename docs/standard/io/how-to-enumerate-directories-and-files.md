---
title: 如何：枚举目录和文件
description: 了解如何使用可枚举集合来枚举目录和文件，可枚举集合在 .NET 中的性能优于数组。
ms.date: 12/27/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- I/O [.NET], enumerating directories and files
ms.assetid: 86b69a08-3bfa-4e5f-b4e1-3b7cb8478215
ms.openlocfilehash: 7f565531bc3ed44370da8859ed210e0e25027d78
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734655"
---
# <a name="how-to-enumerate-directories-and-files"></a>如何：枚举目录和文件

在处理目录和文件的大型集合时，可枚举的集合能够比数组提供更好的性能。 要枚举目录和文件，请使用可返回目录和文件名的可枚举集合的方法或其 <xref:System.IO.DirectoryInfo>、<xref:System.IO.FileInfo> 或 <xref:System.IO.FileSystemInfo> 对象。  
  
如果只想搜索并返回目录名称或文件名，请使用 <xref:System.IO.Directory> 类的枚举方法。 若要搜索并返回目录或文件的其他属性，请使用 <xref:System.IO.DirectoryInfo> 和 <xref:System.IO.FileSystemInfo> 类。  
  
可以使用这些方法中的可枚举集合作为集合类（如 <xref:System.Collections.Generic.List%601>）的构造函数的 <xref:System.Collections.Generic.IEnumerable%601> 参数。  
  
下表总结了返回可枚举的文件和目录集合的方法：  
  
|搜索并返回|使用方法|  
|------------------|-------------------------------------|-------------------|  
|目录名称|<xref:System.IO.Directory.EnumerateDirectories%2A?displayProperty=nameWithType>|  
|目录信息 (<xref:System.IO.DirectoryInfo>)|<xref:System.IO.DirectoryInfo.EnumerateDirectories%2A?displayProperty=nameWithType>|  
|文件名|<xref:System.IO.Directory.EnumerateFiles%2A?displayProperty=nameWithType>|  
|文件信息 (<xref:System.IO.FileInfo>)|<xref:System.IO.DirectoryInfo.EnumerateFiles%2A?displayProperty=nameWithType>|  
|文件系统条目名称|<xref:System.IO.Directory.EnumerateFileSystemEntries%2A?displayProperty=nameWithType>|  
|文件系统条目信息 (<xref:System.IO.FileSystemInfo>)|<xref:System.IO.DirectoryInfo.EnumerateFileSystemInfos%2A?displayProperty=nameWithType>|  
|目录和文件名称 |<xref:System.IO.Directory.EnumerateFileSystemEntries%2A?displayProperty=nameWithType>|  

> [!NOTE]
> 虽然可以使用可选的 <xref:System.IO.SearchOption> 枚举的 <xref:System.IO.SearchOption.AllDirectories> 选项迅速枚举父目录的子目录中的所有文件，但 <xref:System.UnauthorizedAccessException> 错误可能会使枚举不完整。 可以通过先枚举目录，然后枚举文件来捕获这些异常。  
  
## <a name="examples-use-the-directory-class"></a>示例：使用目录类  
  
以下示例使用 <xref:System.IO.Directory.EnumerateDirectories%28System.String%29?displayProperty=nameWithType> 方法获取指定路径中顶级目录名称的列表。  

[!code-csharp[System.IO.EnumDirs1#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.enumdirs1/cs/program.cs#1)]
[!code-vb[System.IO.EnumDirs1#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.enumdirs1/vb/program.vb#1)]  

以下示例使用 <xref:System.IO.Directory.EnumerateFiles%28System.String%2CSystem.String%2CSystem.IO.SearchOption%29?displayProperty=nameWithType> 方法递归枚举目录中的所有文件名以及与特定模式匹配的子目录。 然后它读取每个文件的每一行，并显示包含指定字符串的行及其文件名和路径。

[!code-csharp[System.IO.Directory.EnumerateFiles#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.directory.enumeratefiles/cs/program.cs#1)]
[!code-vb[System.IO.Directory.EnumerateFiles#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.directory.enumeratefiles/vb/program.vb#1)]  
  
## <a name="examples-use-the-directoryinfo-class"></a>示例：使用 DirectoryInfo 类  
  
下面的示例使用 <xref:System.IO.DirectoryInfo.EnumerateDirectories%2A?displayProperty=nameWithType> 方法列出顶级目录的集合，这些顶级目录的 <xref:System.IO.FileSystemInfo.CreationTimeUtc> 早于某个 <xref:System.DateTime> 值。  

[!code-csharp[System.IO.DirectoryInfo.EnumDirs#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.directoryinfo.enumdirs/cs/program.cs)]
[!code-vb[System.IO.DirectoryInfo.EnumDirs#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.directoryinfo.enumdirs/vb/module1.vb)]  
  
下例使用 <xref:System.IO.DirectoryInfo.EnumerateFiles%2A?displayProperty=nameWithType> 方法列出 <xref:System.IO.FileInfo.Length> 超过 10MB 的所有文件。 此示例先枚举顶级目录以捕获可能的未授权访问异常，再枚举文件。  

[!code-csharp[System.IO.DirectoryInfo.EnumerateDirectories#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.directoryinfo.enumeratedirectories/cs/program.cs#1)]
[!code-vb[System.IO.DirectoryInfo.EnumerateDirectories#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.directoryinfo.enumeratedirectories/vb/program.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [文件和流 I/O](index.md)
