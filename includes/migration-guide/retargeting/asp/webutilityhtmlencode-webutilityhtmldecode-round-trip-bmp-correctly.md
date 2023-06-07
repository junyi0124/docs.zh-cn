---
ms.openlocfilehash: acb5b467fc8f0692d8fa1b3b8263fd27308cc124
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617118"
---
### <a name="webutilityhtmlencode-and-webutilityhtmldecode-round-trip-bmp-correctly"></a>WebUtility.HtmlEncode 和 WebUtility.HtmlDecode 正确往返 BMP

#### <a name="details"></a>详细信息

对于面向 .NET Framework 4.5 的应用程序，当基本多语言平面 (BMP) 外的字符传递给 <xref:System.Net.WebUtility.HtmlDecode(System.String)> 方法时，这些字符可正确往返。

#### <a name="suggestion"></a>建议

此更改不应影响当前应用程序，但要还原原始行为，请将 `<httpRuntime>` 元素的 `targetFramework` 属性设置为“4.5”以外的字符串。 还可以设置 `unicodeEncodingConformance` 配置元素的 `unicodeDecodingConformance` 和 `<webUtility>` 特性以单独控制 .NET Framework 的目标版本的行为。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.5         |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Net.WebUtility.HtmlEncode(System.String)?displayProperty=nameWithType>
- <xref:System.Net.WebUtility.HtmlEncode(System.String,System.IO.TextWriter)?displayProperty=nameWithType>
