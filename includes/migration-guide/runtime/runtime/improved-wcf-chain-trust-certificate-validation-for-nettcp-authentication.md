---
ms.openlocfilehash: f6553444e13416850a398ae5bcb6574f2a69bd2d
ms.sourcegitcommit: 721c3e4bdbb1ea0bb420818ec944c538fe5c513a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96477161"
---
### <a name="improved-wcf-chain-trust-certificate-validation-for-nettcp-certificate-authentication"></a>改进了 Net.Tcp 证书身份验证的 WCF 链信任证书验证

#### <a name="details"></a>详细信息

.NET Framework 4.7.2 改进了通过 WCF 对传输安全性使用证书身份验证时的链信任证书验证。 由于此改进，用于对服务器进行身份验证的客户端证书必须为客户端身份验证进行配置。  同样，用于对服务器进行身份验证的服务器证书必须为服务器身份验证进行配置。 由于此更改，如果禁用根证书，证书链验证将失败。 相同的更改还通过 Windows 安全汇总应用于 .NET Framework 3.5 及更高版本。 可在[此处](https://support.microsoft.com/help/4055269/security-only-update-for-net-framework-3-5-1-4-5-2-4-6-4-6-1-4-6-2-4-7)了解详细信息。此更改默认开启，可通过配置设置关闭。

#### <a name="suggestion"></a>建议

<ul><li>验证服务器和客户端证书是否具有所需的 EKU OID。 如果没有，请更新证书。</li><li>验证根证书是否无效。 如果是这样，请更新根证书。</li><li>如何选择退出更改：如果不能更新证书，可以暂时使用以下配置设置来躲开重大更改。但是，选择弃用更改将导致系统易遭受安全性问题。</li></ul><pre><code class="lang-xml">&lt;appSettings&gt;&#13;&#10;&lt;add key=&quot;wcf:useLegacyCertificateUsagePolicy&quot; value=&quot;true&quot; /&gt;&#13;&#10;&lt;/appSettings&gt;&#13;&#10;</code></pre>

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
