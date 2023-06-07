---
ms.openlocfilehash: c996dafcf65b1fd428be908be346de603ead6e0b
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615608"
---
### <a name="certificate-eku-oid-validation"></a>证书 EKU OID 验证

#### <a name="details"></a>详细信息

从 .NET Framework 4.6 开始，<xref:System.Net.Security.SslStream> 或 <xref:System.Net.ServicePointManager> 类执行增强型密钥使用 (EKU) 对象标识符 (OID) 验证。 增强型密钥使用 (EKU) 扩展是指示使用密钥的应用程序的对象标识符 (OID) 的集合。 EKU OID 验证使用远程证书回叫来确保远程证书具有用于预期目的的正确 OID。

#### <a name="suggestion"></a>建议

如果无需此更改，则可通过将以下开关添加到应用配置文件的 [`](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md) 中的 [\<AppContextSwitchOverrides>](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) 来禁用证书 EKU OID 验证：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.Net.DontCheckCertificateEKUs=true" />
</runtime>
```

> [!IMPORTANT]
> 此设置仅用于后向兼容性。 否则不建议使用它。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.6         |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Net.Security.SslStream?displayProperty=nameWithType>
- <xref:System.Net.ServicePointManager?displayProperty=nameWithType>
- <xref:System.Net.Http.HttpClient?displayProperty=nameWithType>
- <xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType>
- <xref:System.Net.HttpWebRequest?displayProperty=nameWithType>
- <xref:System.Net.FtpWebRequest?displayProperty=nameWithType>
