---
ms.openlocfilehash: 36a9db601f7637185bf48dfcbe2233b4489fcdcf
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614330"
---
### <a name="aescryptoserviceprovider-decryptor-provides-a-reusable-transform"></a>AesCryptoServiceProvider 解密器提供了可重用的转换

#### <a name="details"></a>详细信息

从面向 .NET Framework 4.6.2 的应用起，<xref:System.Security.Cryptography.AesCryptoServiceProvider> 解密器提供了可重用的转换。 调用 <xref:System.Security.Cryptography.CryptoAPITransform.TransformFinalBlock(System.Byte[],System.Int32,System.Int32)?displayProperty=fullName> 后，此转换将重新初始化并且可以重用。 对于面向旧版 .NET Framework 的应用，在调用 <xref:System.Security.Cryptography.CryptoAPITransform.TransformFinalBlock(System.Byte[],System.Int32,System.Int32)?displayProperty=fullName> 后尝试通过调用 <xref:System.Security.Cryptography.CryptoAPITransform.TransformBlock(System.Byte[],System.Int32,System.Int32,System.Byte[],System.Int32)?displayProperty=fullName> 重用解密器会引发 <xref:System.Security.Cryptography.CryptographicException> 或导致数据损坏。

#### <a name="suggestion"></a>建议

此更改的影响应该很小，因为这是预期的行为。对于依赖旧行为的应用程序，可通过将以下配置设置添加到应用程序配置文件的 `<runtime>` 部分中，从而选择不用此更改：

```xml
<runtime>
<AppContextSwitchOverrides value="Switch.System.Security.Cryptography.AesCryptoServiceProvider.DontCorrectlyResetDecryptor=true"/>
</runtime>
```

此外，对于面向旧版 .NET framework，但在 .NET Framework 4.6.2 及更高版本下运行的应用程序，可通过将以下配置设置添加到应用程序配置文件的 `<runtime>` 部分，从而选择应用此更改：

```xml
<runtime>
<AppContextSwitchOverrides value="Switch.System.Security.Cryptography.AesCryptoServiceProvider.DontCorrectlyResetDecryptor=false"/>
</runtime>
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.6.2       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Security.Cryptography.AesCryptoServiceProvider.CreateDecryptor?displayProperty=nameWithType>
