---
ms.openlocfilehash: 0d38e2177377e7e9ea911071eb65aa13aa1f5900
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496167"
---
### <a name="dataannotationsdatatypeattributedisableregex-app-setting-is-on-by-default-in-net-framework-472"></a>.NET Framework 4.7.2 中默认启用 "dataAnnotations:dataTypeAttribute:disableRegEx" 应用设置

#### <a name="details"></a>详细信息

.NET Framework 4.6.1 中引入了应用设置 (<code>&quot;dataAnnotations:dataTypeAttribute:disableRegEx&quot;</code>)，允许用户在数据类型属性（如 <xref:System.ComponentModel.DataAnnotations.EmailAddressAttribute?displayProperty=nameWithType>、<xref:System.ComponentModel.DataAnnotations.UrlAttribute?displayProperty=nameWithType> 和 <xref:System.ComponentModel.DataAnnotations.PhoneAttribute?displayProperty=nameWithType>）中禁用正则表达式。 这有助于减少安全漏洞，如可避免使用特定正则表达式进行拒绝服务攻击的可能性。<br/>在 .NET Framework 4.6.1 中，禁用 RegEx 使用这项应用设置默认设置为 <code>false</code>。 从 .NET Framework 4.7.2 开始，此配置开关默认设置为 <code>true</code>，以进一步减少面向 .NET Framework 4.7.2 及更高版本的 Web 应用程序的安全漏洞。

#### <a name="suggestion"></a>建议

如果发现升级到 .NET Framework 4.7.2 后 Web 应用程序中的正则表达式不起作用，则可将 <code>&quot;dataAnnotations:dataTypeAttribute:disableRegEx&quot;</code> 设置的值更新为 <code>false</code>，从而还原到之前的行为。<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;appSettings&gt;&#13;&#10;...&#13;&#10;&lt;add key=&quot;dataAnnotations:dataTypeAttribute:disableRegEx&quot; value=&quot;false&quot;/&gt;&#13;&#10;...&#13;&#10;&lt;/appSettings&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |次要|
|版本|4.7.2|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
