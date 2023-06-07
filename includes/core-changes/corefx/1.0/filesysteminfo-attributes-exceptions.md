---
ms.openlocfilehash: 2ea9abca7578c2ddf92712a1c597f8f1ff4a5c0c
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96031972"
---
### <a name="unauthorizedaccessexception-thrown-by-filesysteminfoattributes"></a>FileSystemInfo.Attributes 引发的 UnauthorizedAccessException

在 .NET Core 中，当调用方尝试设置文件属性值但没有写权限时，将引发 <xref:System.UnauthorizedAccessException>。

#### <a name="change-description"></a>更改描述

在 .NET Framework 中，当调用方尝试在 <xref:System.IO.FileSystemInfo.Attributes?displayProperty=nameWithType> 中设置文件属性值但没有写权限时，将引发 <xref:System.ArgumentException>。 在 .NET Core 中，将改为引发 <xref:System.UnauthorizedAccessException>。 （在 .NET Core 中，如果调用方尝试设置无效的文件属性，则仍会引发 <xref:System.ArgumentException>。）

#### <a name="version-introduced"></a>引入的版本

1.0

#### <a name="recommended-action"></a>建议操作

根据需要修改任何 `catch` 语句不是捕获 <xref:System.ArgumentException>，而是捕获或者除它之外还捕获 <xref:System.UnauthorizedAccessException>。

#### <a name="category"></a>类别

Core .NET 库

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.IO.FileSystemInfo.Attributes?displayProperty=nameWithType>

<!--

#### Affected APIs

- `P:System.IO.FileSystemInfo.Attributes`

-->
