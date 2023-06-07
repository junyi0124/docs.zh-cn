---
ms.openlocfilehash: ef3114a4eb9f62030c3ec36d3b463d07ccd59f6d
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496999"
---
### <a name="webutilityhtmldecode-no-longer-decodes-invalid-input-sequences"></a>WebUtility.HtmlDecode 不再对无效的输入序列进行解码

#### <a name="details"></a>详细信息

默认情况下，解码方法不再将无效的输入序列解码为无效的 UTF-16 字符串。 相反，它们将返回原始的输入。

#### <a name="suggestion"></a>建议

仅当你存储二进制数据而不是字符串中的 UTF-16 数据时，解码器输出中的更改才会起作用。 要显式控制此行为，请将 [appSettings](~/docs/framework/configure-apps/file-schema/appsettings/index.md) 元素的 <code>aspnet:AllowRelaxedUnicodeDecoding</code> 特性设置为 <code>true</code> 以启用旧行为，或设置为 <code>false</code> 以启用当前行为。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |次要|
|Version|4.5|
|类型|运行时

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Net.WebUtility.HtmlDecode(System.String)?displayProperty=nameWithType>
- <xref:System.Net.WebUtility.HtmlDecode(System.String,System.IO.TextWriter)?displayProperty=nameWithType>
- <xref:System.Net.WebUtility.UrlDecode(System.String)?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Net.WebUtility.HtmlDecode(System.String)`
- `M:System.Net.WebUtility.HtmlDecode(System.String,System.IO.TextWriter)`
- `M:System.Net.WebUtility.UrlDecode(System.String)`

-->
