---
ms.openlocfilehash: 53d74db1a77e62cc64250658281fd3e4706fe494
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614322"
---
### <a name="allow-unicode-bidirectional-control-characters-in-uris"></a>允许在 URI 中使用 Unicode 双向控制字符

#### <a name="details"></a>详细信息

Unicode 指定数个特殊控制字符，用于指定文本方向。 在 .NET Framework 先前版本中，即使这些字符已经以百分比编码形式出现，但还是会被错误地从所有 URI 中去除。 为了更好地遵守 [RFC 3987](https://tools.ietf.org/html/rfc3987)，现在，我们允许在 URI 中使用这些字符。 如果在 URI 中出现未编码的字符，则对其进行百分比编码。 如果出现百分比编码的字符，则原样保留。

#### <a name="suggestion"></a>建议

对于面向 .NET Framework 4.7.2 及更高版本的应用程序，默认启用对 Unicode 双向字符的支持。 如果不需要此更改，可通过将以下 [AppContextSwitchOverrides](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) 开关添加到应用程序配置文件的 `<runtime>` 部分，从而禁用更改：

```xml
<runtime>
<AppContextSwitchOverrides value="Switch.System.Uri.DontKeepUnicodeBidiFormattingCharacters=true" />
</runtime>
```

对于面向 .NET Framework 早期版本但在 .NET Framework 4.7.2 及更高版本下运行的应用程序，默认禁用对 Unicode 双向字符的支持。 可通过将以下 [AppContextSwitchOverrides](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) 开关添加到应用程序配置文件的 `<runtime>` 部分，进行启用：

```xml
<runtime>
<AppContextSwitchOverrides value="Switch.System.Uri.DontKeepUnicodeBidiFormattingCharacters=false" />
</runtime>
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| 版本 | 4.7.2       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Uri?displayProperty=nameWithType>
