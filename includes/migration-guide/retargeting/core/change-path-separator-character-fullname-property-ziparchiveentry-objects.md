---
ms.openlocfilehash: f87d0dbeda6094bd745f5b580772b1161f30d0d5
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2020
ms.locfileid: "86218416"
---
### <a name="change-in-path-separator-character-in-fullname-property-of-ziparchiveentry-objects"></a>ZipArchiveEntry 对象的 FullName 属性中的路径分隔符更改

#### <a name="details"></a>详细信息

对于面向 .NET Framework 4.6.1 及更高版本的应用，在通过重载 <xref:System.IO.Compression.ZipFile.CreateFromDirectory%2A> 方法创建的 <xref:System.IO.Compression.ZipArchiveEntry> 对象的 <xref:System.IO.Compression.ZipArchiveEntry.FullName> 属性中，路径分隔符已从反斜杠（“\\”）改为了正斜杠（“/”）。 该更改使 .NET 实现遵循 [.ZIP 文件格式规范](https://pkware.cachefly.net/webdocs/casestudies/APPNOTE.TXT)的 4.4.17.1 部分，还允许 .ZIP 存档在非 Windows 系统上进行解压缩。<br />对于面向非 Windows 操作系统（如 Macintosh）上旧版 .NET Framework 的应用程序，解压缩其创建的 zip 文件将无法保留目录结构。 例如，在 Macintosh 上，应用程序创建一组文件，它们的文件名与目录路径相连，还与任何反斜杠 ("\\") 字符相连。 因此，不会保留解压缩的文件的目录结构。

#### <a name="suggestion"></a>建议

对于在 Windows 操作系统上由 .NET Framework <xref:System.IO?displayProperty=nameWithType> 命名空间中的 API 解压缩的 .ZIP 文件，此更改造成的影响应该是最小的，因为这些 API 可以无缝地将正斜杠（“/”）或反斜杠（“\\”）当作路径分隔符处理。<br />如果不需要此更改，可在应用程序配置文件的 [`<runtime>`](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md) 部分中添加配置设置，从而选择弃用此更改。 以下示例显示 `<runtime>` 部分和 `Switch.System.IO.Compression.ZipFile.UseBackslash` 选择弃用开关：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.IO.Compression.ZipFile.UseBackslash=true" />
</runtime>
```

此外，对于面向先前版本的 .NET Framework，但在 .NET Framework 4.6.1 及更高版本上运行的应用，可通过将配置设置添加到应用程序配置文件的 [`<runtime>`](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md) 部分来选择启用此行为。 以下展示了 `<runtime>` 部分和 `Switch.System.IO.Compression.ZipFile.UseBackslash` 选择弃用开关。

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.IO.Compression.ZipFile.UseBackslash=false" />
</runtime>
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.6.1       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.IO.Compression.ZipFile.CreateFromDirectory(System.String,System.String)?displayProperty=nameWithType>
- <xref:System.IO.Compression.ZipFile.CreateFromDirectory(System.String,System.String,System.IO.Compression.CompressionLevel,System.Boolean)?displayProperty=nameWithType>
- <xref:System.IO.Compression.ZipFile.CreateFromDirectory(System.String,System.String,System.IO.Compression.CompressionLevel,System.Boolean,System.Text.Encoding)?displayProperty=nameWithType>
