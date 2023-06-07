---
ms.openlocfilehash: afcb9b950d4c47b4251dcc8ab0cf9cfc702005c8
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496957"
---
### <a name="wcf-services-that-use-nettcp-with-ssl-security-and-md5-certificate-authentication"></a>使用带有 SSL 安全和 MD5 证书身份验证的 NETTCP 的 WCF 服务

#### <a name="details"></a>详细信息

.NET Framework 4.6 将 TLS 1.1 和 TLS 1.2 添加到 WCF SSL 默认协议列表。 客户端和服务器计算机都安装了 .NET Framework 4.6 或更高版本时，TLS 1.2 用于协商。TLS 1.2 不支持 MD5 证书身份验证。 因此，如果客户使用 MD5 证书，WCF 客户端将无法连接到 WCF 服务。

#### <a name="suggestion"></a>建议

可以通过执行下列任一操作来解决此问题，以便 WCF 客户端可以连接 WCF 服务器：

- 将证书更新为不使用 MD5 算法。 建议采用此解决方案。
- 如果未在源代码中动态配置绑定，将应用程序的配置文件更新为使用 TLS 1.1 或更低版本的协议。 这样便可以继续将证书和 MD5 哈希算法结合使用。

> [!WARNING]
> 不建议采用此解决方法，因为使用 MD5 哈希算法的证书被视为不安全。

下面的配置文件就采用了此解决方法：

```xml
<configuration>
  <system.serviceModel>
    <bindings>
      <netTcpBinding>
        <binding>
          <security mode= "None/Transport/Message/TransportWithMessageCredential" >
            <transport clientCredentialType="None/Windows/Certificate"
                       protectionLevel="None/Sign/EncryptAndSign"
                       sslProtocols="Ssl3/Tls1/Tls11">
            </transport>
          </security>
        </binding>
      </netTcpBinding>
    </bindings>
  </system.ServiceModel>
</configuration>
```

- 如果在源代码中动态配置了绑定，将 <xref:System.ServiceModel.TcpTransportSecurity.SslProtocols?displayProperty=nameWithType> 属性更新为在源代码中使用 TLS 1.1 (<xref:System.Security.Authentication.SslProtocols.Tls11?displayProperty=nameWithType>) 或更早版本的协议。

> [!WARNING]
> 不建议采用此解决方法，因为使用 MD5 哈希算法的证书被视为不安全。

| 名称    | 值   |
|:--------|:--------|
| 范围   | 次要   |
| Version | 4.6     |
| 类型    | 运行时 |

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
