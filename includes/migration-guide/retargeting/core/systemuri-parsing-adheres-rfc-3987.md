---
ms.openlocfilehash: 9c3c0bf7fbd8d45eba84eaa8634fd2d834195702
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617124"
---
### <a name="systemuri-parsing-adheres-to-rfc-3987"></a>System.Uri 分析符合 RFC 3987

#### <a name="details"></a>详细信息

在 .NET Framework 4.5 中，对 URI 分析做了几方面的更改。 但请注意，这些更改仅影响面向 .NET Framework 4.5 的代码。 如果二进制文件面向 .NET Framework 4.0，则将遵守旧行为。 .NET Framework 4.5 中对 URI 分析所做的更改包括：

- URI 分析将根据 RFC 3987 中的最新 IRI 规则执行规范化和字符检查。
- 将仅对 URI 的主机部分执行 Unicode 范式 C。
- 无效 mailto：URI 现在会抛出异常。
- 现在将保留路径段末尾的尾随点。
- `file://` URI 不会转义 `?` 字符。
- 不支持从 `U+0080` 到 `U+009F` 的 Unicode 控制字符。
- 逗号字符 `,` 或 `%2c` 不会自动取消转义。

#### <a name="suggestion"></a>建议

如果需要使用旧的 .NET Framework 4.0 URI 分析语义（通常不需要），可通过面向 .NET Framework 4.0 使用它们。 这可通过在程序集上使用 <xref:System.Runtime.Versioning.TargetFrameworkAttribute?displayProperty=fullName> 来实现，也可以通过“项目属性”页中 Visual Studio 的项目系统 UI 来实现。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 主要       |
| Version | 4.5         |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Uri.%23ctor(System.String)>
- <xref:System.Uri.%23ctor(System.String,System.Boolean)>
- <xref:System.Uri.%23ctor(System.String,System.UriKind)>
- <xref:System.Uri.%23ctor(System.Uri,System.String)>
- <xref:System.Uri.TryCreate(System.String,System.UriKind,System.Uri@)?displayProperty=nameWithType>
- <xref:System.Uri.TryCreate(System.Uri,System.String,System.Uri@)?displayProperty=nameWithType>
- <xref:System.Uri.TryCreate(System.Uri,System.Uri,System.Uri@)?displayProperty=nameWithType>
