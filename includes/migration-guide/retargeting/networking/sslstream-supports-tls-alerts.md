---
ms.openlocfilehash: 5b566dd89801caff7a253abc2fb62c5fd79591f7
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606890"
---
### <a name="sslstream-supports-tls-alerts"></a>SslStream 支持 TLS 警报

#### <a name="details"></a>详细信息

TLS 握手失败后，第一个 I/O 读取/写入操作将引发带有内部 <xref:System.ComponentModel.Win32Exception?displayProperty=fullName> 异常的 <xref:System.IO.IOException?displayProperty=fullName>。 可以使用 [TLS 和 SSL 警报的 Schannel 错误代码](/windows/desktop/SecAuthN/schannel-error-codes-for-tls-and-ssl-alerts)将 <xref:System.ComponentModel.Win32Exception?displayProperty=fullName> 的 <xref:System.ComponentModel.Win32Exception.NativeErrorCode?displayProperty=fullName> 代码从远程参与方映射到 TLS 警报。有关详细信息，请参阅 [RFC 2246：第 7.2.2 节错误警报](https://tools.ietf.org/html/rfc2246#section-7.2.2)。 <br/>.NET Framework 4.6.2 及更早版本中的行为是：如果另一方握手失败然后立即拒绝连接，则传输通道（通常为 TCP 连接）将在写入或读取时超时。

#### <a name="suggestion"></a>建议

调用网络 I/O API（例如 <xref:System.IO.Stream.Read(System.Byte[],System.Int32,System.Int32)>/<xref:System.IO.Stream.Write(System.Byte[],System.Int32,System.Int32)>）的应用程序应处理 <xref:System.IO.IOException> 或 <xref:System.TimeoutException?displayProperty=fullName>。<br/>从 .NET Framework 4.7 开始，TLS 警报功能将默认启用。 在 .NET Framework 4.7 或更高系统上运行的面向 .NET Framework 4.0 到 4.6.2 版本的应用程序将禁用该功能以保留兼容性。 <br/>以下配置 API 用于为在 .NET Framework 4.7 或更高版本上运行的 .NET Framework 4.6 和更高版本应用程序启用或禁用该功能。

- 以编程方式： 必须是应用程序执行的第一件事，因为 ServicePointManager 将只初始化一次：

    ```csharp
    AppContext.SetSwitch("TestSwitch.LocalAppContext.DisableCaching", true);

    // Set to 'false' to enable the feature in .NET Framework 4.6 - 4.6.2.
    AppContext.SetSwitch("Switch.System.Net.DontEnableTlsAlerts", true);
    ```

- AppConfig：

    ```xml
    <runtime>
      <AppContextSwitchOverrides value="Switch.System.Net.DontEnableTlsAlerts=true"/>
      <!-- Set to 'false' to enable the feature in .NET Framework 4.6 - 4.6.2. -->
    </runtime>
    ```

- 注册表项（计算机全局）： 将值设置为 `false` 以在 .NET Framework 4.6 - 4.6.2 中启用该功能。

    ```ini
    Key: HKLM\SOFTWARE\Wow6432Node\Microsoft\.NETFramework\AppContext\Switch.System.Net.DontEnableTlsAlerts
    - Type: String
    - Value: "true"
    ```

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.7         |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Net.Security.SslStream?displayProperty=nameWithType>
- <xref:System.Net.WebRequest?displayProperty=nameWithType>
- <xref:System.Net.HttpWebRequest?displayProperty=nameWithType>
- <xref:System.Net.FtpWebRequest?displayProperty=nameWithType>
- <xref:System.Net.Mail.SmtpClient?displayProperty=nameWithType>
- <xref:System.Net.Http?displayProperty=nameWithType>
