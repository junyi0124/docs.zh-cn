---
ms.openlocfilehash: b7b16e39fa5df9732fa769f2bcd3696dff3b2a49
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497815"
---
### <a name="rsacngverifyhash-now-returns-false-for-any-verification-failure"></a>RSACng.VerifyHash 现在为任意验证失败返回 False

#### <a name="details"></a>详细信息

自 .NET Framework 4.6.2 起，如果签名本身格式不正确，则此方法返回 False  。 现在为任意验证失败返回 false。在 .NET Framework 4.6 和 4.6.1 中，如果签名格式错误，则此方法引发 <xref:System.Security.Cryptography.CryptographicException?displayProperty=fullName>。

#### <a name="suggestion"></a>建议

如果验证失败且此方法返回 False，应改为执行依赖处理 <xref:System.Security.Cryptography.CryptographicException?displayProperty=fullName> 而实现执行的任意代码。

| 名称    | 值       |
|:--------|:------------|
| 范围   |次要|
|Version|4.6.2|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Security.Cryptography.RSACng.VerifyHash(System.Byte[],System.Byte[],System.Security.Cryptography.HashAlgorithmName,System.Security.Cryptography.RSASignaturePadding)?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Security.Cryptography.RSACng.VerifyHash(System.Byte[],System.Byte[],System.Security.Cryptography.HashAlgorithmName,System.Security.Cryptography.RSASignaturePadding)`

-->
