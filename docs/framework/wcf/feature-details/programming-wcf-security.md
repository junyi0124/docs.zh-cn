---
title: WCF 安全编程
description: 了解如何创建安全的 WCF 应用程序，包括身份验证、机密性和完整性。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- message security [WCF], programming overview
ms.assetid: 739ec222-4eda-4cc9-a470-67e64a7a3f10
ms.openlocfilehash: 4ffbf6a05abd3ed9ebcea4b2e85f0dc305a4f2db
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96244764"
---
# <a name="programming-wcf-security"></a>WCF 安全编程

本主题介绍用于创建安全 Windows Communication Foundation (WCF) 应用程序的基本编程任务。 本主题仅介绍身份验证、机密性和完整性，共同称为 *传输安全性*。 本主题不涵盖 (访问资源或服务) 的授权;有关授权的信息，请参阅 [授权](authorization-in-wcf.md)。  
  
> [!NOTE]
> 有关安全概念的重要介绍（特别是在 WCF 方面），请参阅 MSDN 上的模式和实践教程集 [，以了解 Web 服务增强 (WSE) 3.0 的方案、模式和实现指南](/previous-versions/msp-n-p/ff648183(v=pandp.10))。  
  
 WCF 安全编程基于三个步骤设置：安全模式、客户端凭据类型和凭据值。 可以通过代码或配置执行这些步骤。  
  
## <a name="setting-the-security-mode"></a>设置安全模式  

 下面说明了在 WCF 中用安全模式编程的一般步骤：  
  
1. 选择一个适合于应用程序要求的预定义绑定。 有关绑定选项的列表，请参阅 [系统提供的绑定](../system-provided-bindings.md)。 默认情况下，几乎每个绑定都启用了安全。 一种例外情况是 <xref:System.ServiceModel.BasicHttpBinding> 使用配置) 的类 ([\<basicHttpBinding>](../../configure-apps/file-schema/wcf/basichttpbinding.md) 。  
  
     所选的绑定确定了传输协议。 例如，<xref:System.ServiceModel.WSHttpBinding> 使用 HTTP 传输协议；而 <xref:System.ServiceModel.NetTcpBinding> 使用 TCP 传输协议。  
  
2. 为绑定选择一个安全模式。 请注意，所选的绑定确定了可以进行的模式选择。 例如，<xref:System.ServiceModel.WSDualHttpBinding> 不允许启用传输安全（它不是选项）。 同样，<xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> 和 <xref:System.ServiceModel.NetNamedPipeBinding> 都不允许启用消息安全。  
  
     您有三个选择：  
  
    1. `Transport`  
  
         传输安全取决于所选绑定使用的机制。 例如，如果要使用 `WSHttpBinding`，则安全机制是安全套接字层 (SSL)（它也是 HTTPS 协议的机制）。 一般说来，传输安全的主要优点是它提供了较高的吞吐量，而无论您使用哪种传输协议。 但是，它确实具有两个限制：第一个限制是传输机制指示了用于对用户进行身份验证的凭据类型。 只有当服务需要与其他要求不同类型凭据的服务交互操作时，这才是一个缺点。 第二个限制是，因为安全不是在消息级应用的，所以安全是逐个跃点实现的，而不是以端对端方式实现的。 只有当客户端和服务之间的消息路径包含中介时，后一个限制才会成为问题。 有关使用哪种传输的详细信息，请参阅 [选择传输](choosing-a-transport.md)。 有关使用传输安全的详细信息，请参阅 [传输安全概述](transport-security-overview.md)。  
  
    2. `Message`  
  
         消息安全意味着每个消息都包含必要的标头和数据，以保证消息的安全。 因为标头的组成千变万化，所以可以包含任意数量的凭据。 如果您要与其他要求传输机制无法提供的特定凭据类型的服务交互操作，或者如果必须将消息用于多个服务（其中每个服务都要求不同的凭据类型），那么这会是一个有用的办法。  
  
         有关详细信息，请参阅 [消息安全](message-security-in-wcf.md)。  
  
    3. `TransportWithMessageCredential`  
  
         此选择使用传输层来保证消息传输的安全，同时每个消息都包含其他服务需要的丰富凭据。 这便将传输安全的性能优点与消息安全的丰富凭据优点结合起来。 使用下列绑定可实现这一点：<xref:System.ServiceModel.BasicHttpBinding>、<xref:System.ServiceModel.WSFederationHttpBinding>、<xref:System.ServiceModel.NetPeerTcpBinding> 和 <xref:System.ServiceModel.WSHttpBinding>。  
  
3. 如果决定对 HTTP 使用传输安全（即 HTTPS），还必须用 SSL 证书配置主机并且在端口上启用 SSL。 有关详细信息，请参阅 [HTTP 传输安全](http-transport-security.md)。  
  
4. 如果您要使用 <xref:System.ServiceModel.WSHttpBinding> 并且不需要建立安全会话，请将 <xref:System.ServiceModel.NonDualMessageSecurityOverHttp.EstablishSecurityContext%2A> 属性设置为 `false`。  
  
     当客户端和服务使用对称密钥创建通道时（客户端和服务器在整个对话过程中都使用相同的密钥，直到对话结束），将发生安全会话。  
  
## <a name="setting-the-client-credential-type"></a>设置客户端凭据类型  

 选择适当的客户端凭据类型。 有关详细信息，请参阅 [选择凭据类型](selecting-a-credential-type.md)。 下列客户端凭据类型可用：  
  
- `Windows`  
  
- `Certificate`  
  
- `Digest`  
  
- `Basic`  
  
- `UserName`  
  
- `NTLM`  
  
- `IssuedToken`  
  
 根据您设置模式的方式的不同，必须设置凭据类型。 例如，如果您已经选择了 `wsHttpBinding`，并且将模式设置为“Message”，则还可以将 Message 元素的 `clientCredentialType` 属性设置为下列值之一：`None`、`Windows`、`UserName`、`Certificate` 和 `IssuedToken`，如下面的配置示例所示。  
  
```xml  
<system.serviceModel>  
<bindings>  
  <wsHttpBinding>  
    <binding name="myBinding">  
      <security mode="Message"/>  
      <message clientCredentialType="Windows"/>  
    </binding>
  </wsHttpBinding>
</bindings>  
</system.serviceModel>  
```  
  
 或者在代码中：  
  
 [!code-csharp[c_WsHttpService#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wshttpservice/cs/source.cs#1)]
 [!code-vb[c_WsHttpService#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wshttpservice/vb/source.vb#1)]  
  
## <a name="setting-service-credential-values"></a>设置服务凭据值  

 一旦选择了客户端凭据类型，就必须设置可供服务和客户端使用的实际凭据。 在服务上，使用 <xref:System.ServiceModel.Description.ServiceCredentials> 类设置凭据，并且由 <xref:System.ServiceModel.ServiceHostBase.Credentials%2A> 类的 <xref:System.ServiceModel.ServiceHostBase> 属性返回。 所使用的绑定暗示了服务凭据类型、选择的安全模式和客户端凭据类型。 下面的代码为服务凭据设置了证书。  
  
 [!code-csharp[c_tcpService#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_tcpservice/cs/source.cs#3)]
 [!code-vb[c_tcpService#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_tcpservice/vb/source.vb#3)]  
  
## <a name="setting-client-credential-values"></a>设置客户端凭据值  

 在客户端上，使用 <xref:System.ServiceModel.Description.ClientCredentials> 类设置客户端凭据值，并且通过 <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> 类的 <xref:System.ServiceModel.ClientBase%601> 属性返回该值。 下面的代码在使用 TCP 协议的客户端上将证书设置为凭据。  
  
 [!code-csharp[c_TcpClient#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_tcpclient/cs/source.cs#1)]
 [!code-vb[c_TcpClient#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_tcpclient/vb/source.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [基本 WCF 编程](../basic-wcf-programming.md)
- [常用安全方案](common-security-scenarios.md)
