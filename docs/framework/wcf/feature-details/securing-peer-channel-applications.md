---
title: 保护对等通道应用程序
ms.date: 03/30/2017
ms.assetid: d4a0311d-3f78-4525-9c4b-5c93c4492f28
ms.openlocfilehash: 21bec1279d0f0172aa0e8acbcc05adf30a97c5b9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96288419"
---
# <a name="securing-peer-channel-applications"></a>保护对等通道应用程序

与 WinFX 下的其他绑定一样， `NetPeerTcpBinding` 默认情况下已启用安全性，并提供基于传输和消息的安全 (或两者) 。 本主题讨论这两种类型的安全。 安全类型由绑定规范中的安全模式标记指定 (<xref:System.ServiceModel.NetPeerTcpBinding.Security%2A>`Mode`)。  
  
## <a name="transport-based-security"></a>基于传输的安全  

 对等通道支持两种类型的用于保护传输安全的身份验证凭据，这两种类型都要求设置关联 `ClientCredentialSettings.Peer` 上的 `ChannelFactory` 属性：  
  
- Password。 客户端使用有关一个机密密码的信息对连接进行身份验证。 如果使用此凭据类型，则 `ClientCredentialSettings.Peer.MeshPassword` 必须携带一个有效的密码，另外还可以携带一个 `X509Certificate2` 实例。  
  
- 证书。 使用特定的应用程序身份验证。 如果使用此凭据类型，则必须在 <xref:System.IdentityModel.Selectors.X509CertificateValidator> 中使用 `ClientCredentialSettings.Peer.PeerAuthentication` 的一个具体实现。  
  
## <a name="message-based-security"></a>基于消息的安全  

 使用消息安全时，应用程序可以对传出的消息进行签名，以便所有接收方都可以验证此消息是由受信任方发送的并且此消息没有被篡改。 目前，对等通道仅支持 X.509 凭据消息签名。  
  
## <a name="best-practices"></a>最佳实践  
  
- 本节讨论保护对等通道应用程序安全的最佳做法。  
  
### <a name="enable-security-with-peer-channel-applications"></a>为对等通道应用程序启用安全性  

 由于对等通道协议的分布式性质，很难在未受保护的网格中实施网格成员资格审查以及确保保密性和私密性。 另外，还需要记住保护客户端和解析程序服务之间的通信安全。 按照对等名称解析协议 (PNRP) 的要求，应使用安全的名称来避免欺骗和其他常见的攻击。 通过在客户端用来联系解析程序服务的连接上启用安全（包括基于消息的安全和基于传输的安全），可以保护自定义解析程序服务的安全。  
  
### <a name="use-the-strongest-possible-security-model"></a>尽可能使用最强大的安全模型  

 例如，如果需要对网格中的每个成员单独进行标识，请使用基于证书的身份验证模型。 如果不能这样做，请按照当前建议，使用基于密码的身份验证来保证网格成员的安全。 这包括只与受信任方共享密码、使用安全媒体传输密码、经常更改密码以及确保密码为强密码（长度至少为 8 个字符，其中至少包括一个大写字母和一个小写字母、一个数字和一个特殊字符）。  
  
### <a name="never-accept-self-signed-certificates"></a>切勿接受自签名证书  

 切勿接受基于主题名称的证书凭据。 请注意，任何人都可以创建证书，并且任何人都可以选择您正在验证的名称。 若要避免发生欺骗的可能性，请基于证书颁发机构凭据（一个受信任的颁发机构或一个根证书颁发机构）验证证书。  
  
### <a name="use-message-authentication"></a>使用消息身份验证  

 使用消息身份验证来验证消息来自受信任的源，并且消息在传输期间没有被篡改。 如果不使用消息身份验证，则恶意客户端很容易实施欺骗或篡改网格中的消息。  
  
## <a name="peer-channel-code-examples"></a>对等通道代码示例  

 [对等通道方案](peer-channel-scenarios.md)  
  
## <a name="see-also"></a>另请参阅

- [对等通道安全性](peer-channel-security.md)
- [生成对等通道应用程序](building-a-peer-channel-application.md)
