---
ms.openlocfilehash: 87dc93ece10eaedbfbabddb5f857d0bcd12e05c4
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615610"
---
### <a name="only-tls-10-11-and-12-protocols-supported-in-systemnetservicepointmanager-and-systemnetsecuritysslstream"></a>System.Net.ServicePointManager 和 System.Net.Security.SslStream 中仅支持 Tls 1.0、1.1 和 1.2 协议

#### <a name="details"></a>详细信息

从 .NET Framework 4.6 开始，<xref:System.Net.ServicePointManager> 和 <xref:System.Net.Security.SslStream> 类仅可使用以下三种协议之一：Tls1.0、Tls1.1 或 Tls1.2。 不支持 SSL3.0 协议和 RC4 密码。

#### <a name="suggestion"></a>建议

建议的缓解操作是将服务器端应用升级到 Tls1.0、Tls1.1 或 Tls1.2。 如果这不可行或者如果客户端应用被中断，则可以使用 <xref:System.AppContext?displayProperty=fullName> 类并采用如两种方式中的一种来选择退出此功能：

- 以编程方式在 <xref:System.AppContext?displayProperty=fullName> 上设置兼容性开关，如[此处](https://devblogs.microsoft.com/dotnet/net-announcements-at-build-2015/#dotnet46)所述。
- 在 app.config 文件的 `<runtime>` 部分中添加下面的代码行：

```xml
<AppContextSwitchOverrides value="Switch.System.Net.DontEnableSchUseStrongCrypto=true"/>
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.6         |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Net.SecurityProtocolType.Ssl3?displayProperty=nameWithType>
- <xref:System.Security.Authentication.SslProtocols.None?displayProperty=nameWithType>
- <xref:System.Security.Authentication.SslProtocols.Ssl2?displayProperty=nameWithType>
- <xref:System.Security.Authentication.SslProtocols.Ssl3?displayProperty=nameWithType>
