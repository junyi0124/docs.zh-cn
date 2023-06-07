---
ms.openlocfilehash: d3e0a61601f60a389b662f6f74934b6e6dc6e663
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614407"
---
### <a name="the-default-hash-algorithm-for-wpf-packagedigitalsignaturemanager-is-now-sha256"></a>WPF PackageDigitalSignatureManager 现在的默认哈希算法是 SHA256

#### <a name="details"></a>详细信息

`System.IO.Packaging.PackageDigitalSignatureManager` 提供与 WPF 包相关的数字签名功能。  在 .NET Framework 4.7 和更早版本中，包的签名部分使用的默认算法 (<xref:System.IO.Packaging.PackageDigitalSignatureManager.DefaultHashAlgorithm?displayProperty=nameWithType>) 是 SHA1。  由于 SHA1 最近的安全问题，从 .NET Framework 4.7.1 起，此默认算法已更改为 SHA256。  此更改会影响所有包签名，包括 XPS 文档。

#### <a name="suggestion"></a>建议

面向 .NET Framework 4.7.1 以下框架版本并需要利用此更改的开发人员或者面向 .NET Framework 4.7.1 或更高版本并需要之前功能的开发人员可以正确设置以下 AppContext 标记。  值为 true 将导致 SHA1 作为默认算法使用；而 false 则导致 SHA256 作为默认算法使用。<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.MS.Internal.UseSha1AsDefaultHashAlgorithmForDigitalSignatures=true&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.7.1       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.IO.Packaging.PackageDigitalSignatureManager.DefaultHashAlgorithm?displayProperty=nameWithType>
