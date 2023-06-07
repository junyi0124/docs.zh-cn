---
ms.openlocfilehash: e0cdcce9b8c7d591925b08635e3354dadaf22b7b
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556017"
---
### <a name="envelopedcms-defaults-to-aes-256-encryption"></a>EnvelopedCms 默认为 AES-256 加密

`EnvelopedCms` 使用的默认对称加密算法已由 TripleDES 改为 AES-256。

#### <a name="change-description"></a>更改描述

在以前的版本中，如果 <xref:System.Security.Cryptography.Pkcs.EnvelopedCms> 用于对数据加密但未通过构造函数重载指定对称加密算法，则使用 TripleDES/3DES/3DEA/DES3-EDE 算法对数据加密。

从 .NET Core 3.0 开始（通过 [System.Security.Cryptography.Pkcs](https://www.nuget.org/packages/System.Security.Cryptography.Pkcs/) NuGet 包的 4.6.0 版本），默认算法已改为 AES-256 以实现算法现代化并提高默认选项的安全性。 如果消息收件人证书具有（非 EC）Diffie-Hellman 公钥，则由于底层平台的限制，加密操作可能会失败并显示 <xref:System.Security.Cryptography.CryptographicException>。

在下面的示例代码中，如果在 .NET Core 2.2 或更早版本上运行，则使用 TripleDES 对数据加密。 如果在.NET Core 3.0 或更高版本上运行，则使用 AES-256 对数据加密。

```csharp
EnvelopedCms cms = new EnvelopedCms(content);
cms.Encrypt(recipient);
return cms.Encode();
```

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="recommended-action"></a>建议操作

如果该更改有负面影响，则可以通过在包含 <xref:System.Security.Cryptography.Pkcs.AlgorithmIdentifier> 类型参数的 <xref:System.Security.Cryptography.Pkcs.EnvelopedCms> 构造函数中显式指定加密算法标识符来还原 TripleDES 加密，例如：

```csharp
Oid tripleDesOid = new Oid("1.2.840.113549.3.7", null);
AlgorithmIdentifier tripleDesIdentifier = new AlgorithmIdentifier(tripleDesOid);
EnvelopedCms cms = new EnvelopedCms(content, tripleDesIdentifier);

cms.Encrypt(recipient);
return cms.Encode()l
```

#### <a name="category"></a>类别

密码

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Security.Cryptography.Pkcs.EnvelopedCms.%23ctor>
- <xref:System.Security.Cryptography.Pkcs.EnvelopedCms.%23ctor(System.Security.Cryptography.Pkcs.ContentInfo)>
- <xref:System.Security.Cryptography.Pkcs.EnvelopedCms.%23ctor(System.Security.Cryptography.Pkcs.SubjectIdentifierType,System.Security.Cryptography.Pkcs.ContentInfo)>

<!--

#### Affected APIs

- `M:System.Security.Cryptography.Pkcs.EnvelopedCms.#ctor`
- `M:System.Security.Cryptography.Pkcs.EnvelopedCms.#ctor(System.Security.Cryptography.Pkcs.ContentInfo)`
- `M:System.Security.Cryptography.Pkcs.EnvelopedCms.%23ctor(System.Security.Cryptography.Pkcs.SubjectIdentifierType,System.Security.Cryptography.Pkcs.ContentInfo)`

-->
