---
ms.openlocfilehash: f7dcf9c4c3dc7ea536ddc847769a1a30f1298bb2
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617125"
---
### <a name="systemuriiswellformeduristring-method-returns-false-for-relative-uris-with-a-colon-char-in-first-segment"></a>System.Uri.IsWellFormedUriString 方法为第一个段中具有冒号字符型的相对 URI 返回 false

#### <a name="details"></a>详细信息

从 .NET Framework 4.5 开始，<xref:System.Uri.IsWellFormedUriString(System.String,System.UriKind)> 会将第一个段中带有 `:` 的相对 URI 视为格式不正确。 这是 .NET Framework 4.0 中 <xref:System.Uri.IsWellFormedUriString(System.String,System.UriKind)?displayProperty=fullName> 行为的更改，以符合 RFC3986。

#### <a name="suggestion"></a>建议

此更改（与许多其他 URI 更改一样）仅影响以 .NET Framework 4.5（或更高版本）为目标的应用程序。 要继续使用旧行为，请使应用以 .NET Framework 4.0 为目标。 或者，调用 <xref:System.Uri.IsWellFormedUriString(System.String,System.UriKind)?displayProperty=fullName> 前扫描 URI，查找出于验证目的想要删除的 `:` 字符（如果旧行为是可取的）。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.5         |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Uri.IsWellFormedUriString(System.String,System.UriKind)?displayProperty=nameWithType>
