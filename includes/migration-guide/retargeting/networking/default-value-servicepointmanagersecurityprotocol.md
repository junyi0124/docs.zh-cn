---
ms.openlocfilehash: 8aff4b1aa329d6fdfebf3b62e9279e9dfe5de0a4
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606405"
---
### <a name="default-value-of-servicepointmanagersecurityprotocol-is-securityprotocoltypesystemdefault"></a>ServicePointManager.SecurityProtocol 的默认值为 SecurityProtocolType.System.Default

#### <a name="details"></a>详细信息

从面向 .NET Framework 4.7 的应用开始，<xref:System.Net.ServicePointManager.SecurityProtocol?displayProperty=nameWithType> 属性的默认值为 <xref:System.Net.SecurityProtocolType.SystemDefault?displayProperty=nameWithType>。 此更改允许基于 SslStream 的 .NET Framework 网络 API（例如 FTP、HTTPS 和 SMTP）从操作系统继承默认安全协议，而不是使用 .NET Framework 定义的硬编码值。 默认值因操作系统和系统管理员执行的任何自定义配置而异。 有关 Windows 操作系统各版本中默认 SChannel 协议的信息，请参阅 [Protocols in TLS/SSL (Schannel SSP)](/windows/desktop/SecAuthN/protocols-in-tls-ssl--schannel-ssp-)（TLS/SSL (Schannel SSP) 中的协议）。</p>对于面向 .NET Framework 早期版本的应用程序，<xref:System.Net.ServicePointManager.SecurityProtocol?displayProperty=nameWithType> 属性的默认值取决于所面向的 .NET Framework 版本。 请参阅[“针对 .NET Framework 4.5.2 到 4.6 迁移的重定目标更改”中的“网络”部分](~/docs/framework/migration-guide/retargeting/4.5.2-4.6.md#networking)，了解详细信息。

#### <a name="suggestion"></a>建议

此更改会影响面向 .NET Framework 4.7 或更高版本的应用程序。 如果希望使用定义协议，而不是依赖于系统默认协议，可显式设置 <xref:System.Net.ServicePointManager.SecurityProtocol?displayProperty=nameWithType> 属性的值。 如果不需要此更改，可在应用程序配置文件的 [\<runtime>](~/docs/framework/configure-apps/file-schema/runtime/runtime-element.md) 部分中添加配置设置，从而选择弃用此更改。 以下示例显示 `<runtime>` 部分和 `Switch.System.Net.DontEnableSystemDefaultTlsVersions` 选择弃用开关：

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.Net.DontEnableSystemDefaultTlsVersions=true" />
</runtime>
```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.7         |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Net.ServicePointManager.SecurityProtocol?displayProperty=nameWithType>
