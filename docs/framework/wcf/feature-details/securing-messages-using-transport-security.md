---
title: 使用传输安全保护消息
ms.date: 03/30/2017
ms.assetid: 9029771a-097e-448a-a13a-55d2878330b8
ms.openlocfilehash: b3931d594b585a176be7a9a2e2779df5d844c56b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96288589"
---
# <a name="securing-messages-using-transport-security"></a>使用传输安全保护消息

本节讨论消息队列 (MSMQ) 传输安全，您可将其用于保护发送到队列的消息。  
  
> [!NOTE]
> 在阅读本主题之前，建议你阅读 [安全概念](security-concepts.md)。  
  
 下图提供了使用 Windows Communication Foundation (WCF) 进行排队通信的概念模型。 此插图和术语用于说明传输安全概念。  
  
 ![排队应用程序关系图](media/distributed-queue-figure.jpg "分布式队列图")  
  
 使用 WCF 发送排队消息时 <xref:System.ServiceModel.NetMsmqBinding> ，wcf 消息将附加为 MSMQ 消息的正文。 传输安全可以保护全部 MSMQ 消息（MSMQ 消息头或属性和消息正文）的安全。 由于它是 MSMQ 消息的正文，因此使用传输安全性还能保护 WCF 消息。  
  
 传输安全的关键概念在于，客户端必须满足安全要求，才能使消息进入目标队列。 这与消息安全不同。在消息安全中，针对接收消息的应用程序来保护消息。  
  
 使用 <xref:System.ServiceModel.NetMsmqBinding> 和 <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> 的传输安全影响着在传输队列和目标队列之间进行传输时，对 MSMQ 消息进行保护的方式，其中，保护是指：  
  
- 对消息进行签名，以确保该消息未经篡改。  
  
- 对消息进行加密，以确保无法查看或篡改该消息。 此操作是可选的，但建议执行该操作。  
  
- 目标队列管理器对消息发送方进行标识以确保不可否认性。  
  
 在 MSMQ 中，目标队列独立于身份验证，具有一个访问控制列表 (ACL)，用于检查客户端是否有权将消息发送到目标队列， 同时还将检查接收应用程序是否有从目标队列接收消息的权限。  
  
## <a name="wcf-msmq-transport-security-properties"></a>WCF MSMQ 传输安全属性  

 MSMQ 使用 Windows 安全性进行身份验证。 MSMQ 使用 Windows 安全标识符 (SID) 来标识客户端，并将 Active Directory 目录服务用作对客户端进行身份验证时的证书颁发机构。 这要求 MSMQ 与 Active Directory 集成一起安装。 由于使用 Windows 域 SID 标识客户端，因此仅当客户端和服务都属于相同的 Windows 域时，此安全选项才有意义。  
  
 MSMQ 还能够将证书附加到未向 Active Directory 注册的消息中。 在这种情况下，它可以确保该消息已使用附加的证书进行签名。  
  
 WCF 提供了这两个选项作为 MSMQ 传输安全的一部分，它们是用于传输安全的密钥透视。  
  
 默认情况下将启用传输安全。  
  
 了解这些基本知识后，以下各节将详细介绍与 <xref:System.ServiceModel.NetMsmqBinding> 和 <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> 绑定的传输安全属性。  
  
#### <a name="msmq-authentication-mode"></a>MSMQ 身份验证模式  

 <xref:System.ServiceModel.MsmqTransportSecurity.MsmqAuthenticationMode%2A> 指示是使用 Windows 域安全还是基于外部证书的安全来保护消息。 在这两种身份验证模式下，WCF 排队传输通道都使用 `CertificateValidationMode` 服务配置中指定的。 证书验证模式指定用于检查证书有效性的机制。  
  
 启用传输安全时，默认的设置为 <xref:System.ServiceModel.MsmqAuthenticationMode.WindowsDomain>。  
  
#### <a name="windows-domain-authentication-mode"></a>Windows 域身份验证模式  

 如果选择使用 Windows 安全，则需要 Active Directory 集成。 <xref:System.ServiceModel.MsmqAuthenticationMode.WindowsDomain> 是默认的传输安全模式。 设置此设置后，WCF 通道会将 Windows SID 附加到 MSMQ 消息，并使用从 Active Directory 获取的内部证书。 MSMQ 将此内部证书用于保护消息。 接收队列管理器使用 Active Directory 来搜索并查找匹配的证书，以便对客户端进行身份验证，并检查 SID 是否还与客户端的 SID 匹配。 如果证书（在 `WindowsDomain` 身份验证模式下在内部生成或在 `Certificate` 身份验证模式下在外部生成）附加到消息中，则即使目标队列未标记为要求身份验证，也将执行此身份验证步骤。  
  
> [!NOTE]
> 创建队列时，您可以将队列标记为已进行身份验证的队列，以指示队列要求对向队列发送消息的客户端进行身份验证。 这可以确保队列中不接受未经身份验证的消息。  
  
 附加到消息中的 SID 还用于根据目标队列的 ACL 进行检查，以确保客户端具有向队列发送消息的权限。  
  
#### <a name="certificate-authentication-mode"></a>证书身份验证模式  

 使用证书身份验证模式不需要 Active Directory 集成。 实际上，在某些情况下，例如在工作组模式（没有 Active Directory 集成）中安装 MSMQ 时，或使用 SOAP 可靠传送消息协议 (SRMP) 将消息发送到队列时，只有 <xref:System.ServiceModel.MsmqAuthenticationMode.Certificate> 才起作用。  
  
 使用发送 WCF 消息时 <xref:System.ServiceModel.MsmqAuthenticationMode.Certificate> ，wcf 通道不会将 WINDOWS SID 附加到 MSMQ 消息。 同样，目标队列 ACL 必须允许使用 `Anonymous` 用户权限向队列发送消息。 接收队列管理器检查 MSMQ 消息是否已使用证书进行签名，但不执行任何身份验证。  
  
 带有声明和标识信息的证书 <xref:System.ServiceModel.ServiceSecurityContext> 由 WCF 排队传输通道在中填充。 服务可使用此信息来对发送方执行其自己的身份验证。  
  
### <a name="msmq-protection-level"></a>MSMQ 保护级别  

 保护级别指示如何保护 MSMQ 消息以确保该消息不会被篡改。 保护级别是在 <xref:System.ServiceModel.MsmqTransportSecurity.MsmqProtectionLevel%2A> 属性中指定的。 默认值为 <xref:System.Net.Security.ProtectionLevel.Sign>。  
  
#### <a name="sign-protection-level"></a>“签名”保护级别  

 如果使用 `WindowsDomain` 身份验证模式，则使用内部生成的证书对 MSMQ 消息进行签名；如果使用 `Certificate` 身份验证模式，则使用外部生成的证书对 MSMQ 消息进行签名。  
  
#### <a name="sign-and-encrypt-protection-level"></a>“签名和加密”保护级别  

 如果使用 `WindowsDomain` 身份验证模式，则使用内部生成的证书对 MSMQ 消息进行签名；如果使用 `Certificate` 身份验证模式，则使用外部生成的证书对 MSMQ 消息进行签名。  
  
 除对消息进行签名外，MSMQ 消息将使用从属于承载目标队列的接收队列管理器的 Active Directory 处获取的证书公钥进行加密。 发送队列管理器可确保在传递过程中对 MSMQ 消息进行加密。 接收队列管理器可使用其内部证书的私钥来解密 MSMQ 消息，并将该消息以明文形式存储在队列中（如果该队列已经过身份验证和授权）。  
  
> [!NOTE]
> 若要加密消息，必须具有 Active Directory 访问权限（`UseActiveDirectory` 的 <xref:System.ServiceModel.NetMsmqBinding> 属性必须设置为 `true`），并且该权限可与 <xref:System.ServiceModel.MsmqAuthenticationMode.Certificate> 和 <xref:System.ServiceModel.MsmqAuthenticationMode.WindowsDomain> 一起使用。  
  
#### <a name="none-protection-level"></a>“无”保护级别  

 这表示 <xref:System.ServiceModel.MsmqTransportSecurity.MsmqProtectionLevel%2A> 设置为 <xref:System.Net.Security.ProtectionLevel.None>。 对于任何其他身份验证模式，这都不是一个有效值。  
  
> [!NOTE]
> 如果已对 MSMQ 消息签名，MSMQ 将检查该消息是否是使用独立于队列状态（即是否是经过身份验证的队列）的附加证书（内部或外部）进行签名的。  
  
### <a name="msmq-encryption-algorithm"></a>MSMQ 加密算法  

 加密算法指定用于对网络上的 MSMQ 消息进行加密的算法。 仅当 <xref:System.ServiceModel.MsmqTransportSecurity.MsmqProtectionLevel%2A> 设置为 <xref:System.Net.Security.ProtectionLevel.EncryptAndSign> 时，才使用此属性。  
  
 支持的算法是 `RC4Stream` 和 `AES`，默认算法为 `RC4Stream`。  
  
 仅当发送方安装 MSMQ 4.0 后，您才可以使用 `AES` 算法。 此外，目标队列还必须承载在 MSMQ 4.0 之上。  
  
### <a name="msmq-hash-algorithm"></a>MSMQ 哈希算法  

 哈希算法指定用于创建 MSMQ 消息的数字签名的算法。 接收队列管理器使用此算法对 MSMQ 消息进行身份验证。 只有在 <xref:System.ServiceModel.MsmqTransportSecurity.MsmqProtectionLevel%2A> 设置为 <xref:System.Net.Security.ProtectionLevel.Sign> 或 <xref:System.Net.Security.ProtectionLevel.EncryptAndSign> 时，才使用此属性。  
  
 支持的算法包括 `MD5`、`SHA1`、`SHA256` 和 `SHA512`。 默认值为 `SHA1`。

 由于 MD5/SHA1 出现冲突，Microsoft 建议 SHA256 或更好。
  
## <a name="see-also"></a>另请参阅

- [队列概述](queues-overview.md)
- [安全性概念](security-concepts.md)
- [保护服务和客户端的安全](securing-services-and-clients.md)
