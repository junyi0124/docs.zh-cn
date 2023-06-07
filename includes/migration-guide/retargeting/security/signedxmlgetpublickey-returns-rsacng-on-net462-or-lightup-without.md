---
ms.openlocfilehash: 23e278d38d6904d8afe927e6b54c388d443e41f5
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85616026"
---
### <a name="signedxmlgetpublickey-returns-rsacng-on-net462-or-lightup-without-retargeting-change"></a>SignedXml.GetPublicKey 在 net462（或 lightup）上返回 RSACng 而不重定向更改

#### <a name="details"></a>详细信息

从 .NET Framework 4.6.2 开始，<xref:System.Security.Cryptography.Xml.SignedXml.GetPublicKey%2A?displayProperty=nameWithType> 方法所返回对象的具体类型从 CryptoServiceProvider 实现更改为 Cng 实现（不奇怪）。 这是因为实现已从使用 `certificate.PublicKey.Key` 更改为使用内部 `certificate.GetAnyPublicKey`（将转到 <xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPublicKey%2A?displayProperty=nameWithType>）。

#### <a name="suggestion"></a>建议

从在 .NET Framework 4.7.1 上运行的应用开始，可通过将以下配置开关添加到应用配置文件的[运行时](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md)部分，使用 .NET Framework 4.6.1 和早期版本中默认使用的 CryptoServiceProvider 实现：

```xml
<AppContextSwitchOverrides value="Switch.System.Security.Cryptography.Xml.SignedXmlUseLegacyCertificatePrivateKey=true" />
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.6.2       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Security.Cryptography.Xml.SignedXml.CheckSignatureReturningKey(System.Security.Cryptography.AsymmetricAlgorithm@)?displayProperty=nameWithType>
