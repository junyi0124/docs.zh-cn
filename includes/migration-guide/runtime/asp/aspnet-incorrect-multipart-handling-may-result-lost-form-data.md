---
ms.openlocfilehash: d61a3b3dd855d783d7bff7cb74e5b84969e60860
ms.sourcegitcommit: 97405ed212f69b0a32faa66a5d5fae7e76628b68
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/01/2020
ms.locfileid: "91608038"
---
### <a name="aspnet-incorrect-multipart-handling-may-result-in-lost-form-data"></a>ASP.NET 多部分处理不正确可能会导致丢失窗体数据。

#### <a name="details"></a>详细信息

在面向 .NET Framework 4.7.2 及更低版本的应用程序中，ASP.NET 可能会错误地解析多部分边界值，从而导致窗体数据在请求执行期间不可用。 面向 .NET Framework 4.8 或更高版本的应用程序正确解析多部分数据，因此在请求执行期间可以使用窗体值。

#### <a name="suggestion"></a>建议

从运行 .NET Framework 4.8 的应用程序开始，在面向 .NET Framework 4.8 或更高版本时使用 <code>targetFrameworkVersion</code> 元素，默认行为将更改为竖线分隔符。 在面向上述框架版本或不使用 <code>targetFrameworkVersion</code> 时，仍会返回一些值的尾随分隔符。此行为也可以通过 <code>appSetting</code> 显式控制：<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;appSettings&gt;&#13;&#10;...&#13;&#10;&lt;add key=&quot;aspnet:UseLegacyMultiValueHeaderHandling&quot;  value=&quot;true&quot;/&gt;&#13;&#10;...&#13;&#10;&lt;/appSettings&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |未知|
|Version|4.8|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Web.HttpRequest.Form?displayProperty=nameWithType>
- <xref:System.Web.HttpRequest.Files?displayProperty=nameWithType>
- <xref:System.Web.HttpRequest.ContentEncoding?displayProperty=nameWithType>

<!--

#### Affected APIs

- `P:System.Web.HttpRequest.Form`
- `P:System.Web.HttpRequest.Files`
- `P:System.Web.HttpRequest.ContentEncoding`

-->
