---
ms.openlocfilehash: e2ae329d027d605e6331afe422e550990fab1042
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614379"
---
### <a name="default-signedxml-and-signedxms-algorithms-changed-to-sha256"></a>默认的 SignedXML 和 SignedXMS 算法已更改为 SHA256

#### <a name="details"></a>详细信息

在 .NET Framework 4.7 及早期版本中，SignedXML 和 SignedCMS 默认为 SHA1 以执行某些操作。从 .NET Framework 4.7.1 开始，默认情况下启用 SHA256 来执行这些操作。 此更改是必需的，因为 SHA1 不再是安全的。

#### <a name="suggestion"></a>建议

有两个新的上下文切换值，用于控制默认情况下使用 SHA1（不安全）还是 SHA256：

- Switch.System.Security.Cryptography.Xml.UseInsecureHashAlgorithms
- Switch.System.Security.Cryptography.Pkcs.UseInsecureHashAlgorithms 对于面向 .NET Framework 4.7.1 及更高版本的应用程序，如果不希望使用 SHA256，则可通过将以下配置开关添加到应用配置文件的[运行时](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md)部分，将默认值还原为 SHA1：

```xml
<AppContextSwitchOverrides value="Switch.System.Security.Cryptography.Xml.UseInsecureHashAlgorithms=true;Switch.System.Security.Cryptography.Pkcs.UseInsecureHashAlgorithms=true" />
```

对于面向 .NET Framework 4.7 及更高版本的应用程序，可通过将以下配置开关添加到应用配置文件的[运行时](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md)部分，来选择使用此更改：

```xml
<AppContextSwitchOverrides value="Switch.System.Security.Cryptography.Xml.UseInsecureHashAlgorithms=false;Switch.System.Security.Cryptography.Pkcs.UseInsecureHashAlgorithms=false" />
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.7.1       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Security.Cryptography.Pkcs.CmsSigner?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.Xml.SignedXml?displayProperty=nameWithType>
- <xref:System.Security.Cryptography.Xml.Reference?displayProperty=nameWithType>
