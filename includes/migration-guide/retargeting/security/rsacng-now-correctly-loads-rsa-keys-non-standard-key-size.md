---
ms.openlocfilehash: c1e85941c8c6c31c7d937d0671ad955fa6490783
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614327"
---
### <a name="rsacng-now-correctly-loads-rsa-keys-of-non-standard-key-size"></a>RSACng 现在可正确加载非标准密钥大小的 RSA 密钥

#### <a name="details"></a>详细信息

在 .NET Framework 4.6.2 之前的版本中，使用非标准密钥大小的 RSA 证书的客户无法通过 <xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPublicKey(System.Security.Cryptography.X509Certificates.X509Certificate2)?displayProperty=fullName> 和 <xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPrivateKey(System.Security.Cryptography.X509Certificates.X509Certificate2)?displayProperty=fullName> 扩展方法访问这些密钥。  引发 <xref:System.Security.Cryptography.CryptographicException?displayProperty=fullName>，并附带“不支持请求的密钥大小”消息。 .NET Framework 4.6.2 中已修复此问题。 同样，<xref:System.Security.Cryptography.RSA.ImportParameters(System.Security.Cryptography.RSAParameters)> 和 <xref:System.Security.Cryptography.RSACng.ImportParameters(System.Security.Cryptography.RSAParameters)> 现在可以处理非标准密钥大小，而不会引发 <xref:System.Security.Cryptography.CryptographicException?displayProperty=fullName>。

#### <a name="suggestion"></a>建议

如果处理依赖于先前行为的逻辑时有任何异常 - 使用非标准密钥大小时引发 <xref:System.Security.Cryptography.CryptographicException?displayProperty=fullName>，请考虑删除该逻辑。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.6.2       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Security.Cryptography.RSA.ImportParameters(System.Security.Cryptography.RSAParameters)?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.RSACng.ImportParameters(System.Security.Cryptography.RSAParameters)?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPrivateKey(System.Security.Cryptography.X509Certificates.X509Certificate2)?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.X509Certificates.RSACertificateExtensions.GetRSAPublicKey(System.Security.Cryptography.X509Certificates.X509Certificate2)?displayProperty=nameWithType>
