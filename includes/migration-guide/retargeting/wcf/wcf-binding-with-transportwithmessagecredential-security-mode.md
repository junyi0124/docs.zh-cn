---
ms.openlocfilehash: 0e949cbdeda99dd7b94e919b903a21171a57f527
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614354"
---
### <a name="wcf-binding-with-the-transportwithmessagecredential-security-mode"></a>与 TransportWithMessageCredential 安全模式绑定的 WCF

#### <a name="details"></a>详细信息

从 .NET Framework 4.6.1 开始，可将使用 TransportWithMessageCredential 安全模式的 WCF 绑定设置为接收带有非对称安全密钥的未签名&quot;to&quot;标头的消息。默认情况下，.NET Framework 4.6.1 中将继续拒绝未签名&quot;to&quot;标头。 仅当应用程序使用 Switch.System.ServiceModel.AllowUnsignedToHeader 配置开关选择启用此新操作模式时，才会接受它们。

#### <a name="suggestion"></a>建议

由于这是一项可以选择使用的功能，因此它不应影响现有应用的行为。<br/>要控制是否使用新行为，请使用以下配置设置：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.ServiceModel.AllowUnsignedToHeader=true" />
</runtime>
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 透明 |
| Version | 4.6.1       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.ServiceModel.BasicHttpSecurityMode.TransportWithMessageCredential?displayProperty=nameWithType>
- <xref:System.ServiceModel.BasicHttpsSecurityMode.TransportWithMessageCredential?displayProperty=nameWithType>
- <xref:System.ServiceModel.SecurityMode.TransportWithMessageCredential?displayProperty=nameWithType>
- <xref:System.ServiceModel.WSFederationHttpSecurityMode.TransportWithMessageCredential?displayProperty=nameWithType>
