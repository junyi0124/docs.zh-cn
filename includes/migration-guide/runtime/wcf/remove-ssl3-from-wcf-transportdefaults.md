---
ms.openlocfilehash: a5f4047d70276a90c9d72918a2559fd795feb26e
ms.sourcegitcommit: 261e0c98a111357692b3b63c596edf0cacf72991
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/18/2020
ms.locfileid: "90770908"
---
### <a name="remove-ssl3-from-the-wcf-transportdefaults"></a>从 WCF TransportDefaults 删除 Ssl3

#### <a name="details"></a>详细信息

结合使用 NetTcp 与传输安全性和凭据类型证书时，SSL 3 协议不再是用于协商安全连接的默认协议。 在大多数情况下，应该不会对现有应用程序造成任何影响，因为 TLS 1.0 始终包含在 NetTcp 协议列表中。 所有现有客户端应该至少能够使用 TLS 1.0 来协商连接。

#### <a name="suggestion"></a>建议

如果 Ssl3 必需，则使用以下配置机制之一将 Ssl3 添加到协商协议的列表。

- <xref:System.ServiceModel.Channels.SslStreamSecurityBindingElement.SslProtocols>
- <xref:System.ServiceModel.TcpTransportSecurity.SslProtocols>
- [\<netTcpBinding> 的 \<transport>](../../../../docs/framework/configure-apps/file-schema/wcf/transport-of-nettcpbinding.md)
- [\<sslStreamSecurity>](../../../../docs/framework/configure-apps/file-schema/wcf/sslstreamsecurity.md)

| 名称    | 值   |
|:--------|:--------|
| 范围   | 边缘    |
| Version | 4.6.2   |
| 类型    | 运行时 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.ServiceModel.Channels.SslStreamSecurityBindingElement.SslProtocols?displayProperty=nameWithType>
- <xref:System.ServiceModel.TcpTransportSecurity.SslProtocols?displayProperty=nameWithType>

<!--

#### Affected APIs

- `P:System.ServiceModel.Channels.SslStreamSecurityBindingElement.SslProtocols`
- `P:System.ServiceModel.TcpTransportSecurity.SslProtocols`

-->
