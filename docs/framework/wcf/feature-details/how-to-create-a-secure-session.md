---
title: 如何：创建安全会话
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- security [WCF], creating a session
ms.assetid: b6f42b5a-bbf7-45cf-b917-7ec9fa7ae110
ms.openlocfilehash: f6fb73653add7362e8c8452e75be802395ffc3cd
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96286521"
---
# <a name="how-to-create-a-secure-session"></a>如何：创建安全会话

除 [\<basicHttpBinding>](../../configure-apps/file-schema/wcf/basichttpbinding.md) 绑定外，系统提供的绑定 Windows Communication Foundation (WCF) 当启用消息安全时，将自动使用安全会话。  
  
 默认情况下，安全会话不会在已回收的 Web 服务器中存在。 建立安全会话时，客户端和服务将缓存与安全会话关联的密钥。 交换消息时，只交换已缓存密钥的标识符。 如果回收了 Web 服务器，则也会回收缓存，因此 Web 服务器将无法检索该标识符的已缓存密钥。 如果发生这种情况，将会引发异常并返回至客户端。 使用有状态安全上下文令牌 (SCT) 的安全会话可以在回收 Web 服务器后存在。 有关在安全会话中使用有状态 SCT 的详细信息，请参阅 [如何：为安全会话创建安全上下文令牌](how-to-create-a-security-context-token-for-a-secure-session.md)。  
  
### <a name="to-specify-that-a-service-uses-secure-sessions-by-using-one-of-the-system-provided-bindings"></a>通过使用系统提供的一个绑定指定服务使用安全会话  
  
- 配置服务以使用支持消息安全的系统提供的绑定。  
  
     除 [\<basicHttpBinding>](../../configure-apps/file-schema/wcf/basichttpbinding.md) 绑定外，当系统提供的绑定配置为使用消息安全性时，WCF 将自动使用安全会话。 下表列出了支持消息安全的系统提供的绑定以及消息安全是否是默认的安全机制。  
  
    |系统提供的绑定|配置元素|默认情况下是否启用消息安全|  
    |------------------------------|---------------------------|------------------------------------|  
    |<xref:System.ServiceModel.BasicHttpBinding>|[\<basicHttpBinding>](../../configure-apps/file-schema/wcf/basichttpbinding.md)|否|  
    |<xref:System.ServiceModel.WSHttpBinding>|[\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md)|是|  
    |<xref:System.ServiceModel.WSDualHttpBinding>|[\<wsDualHttpBinding>](../../configure-apps/file-schema/wcf/wsdualhttpbinding.md)|是|  
    |<xref:System.ServiceModel.WSFederationHttpBinding>|[\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md)|是|  
    |<xref:System.ServiceModel.NetTcpBinding>|[\<netTcpBinding>](../../configure-apps/file-schema/wcf/nettcpbinding.md)|否|  
    |<xref:System.ServiceModel.NetMsmqBinding>|[\<netMsmqBinding>](../../configure-apps/file-schema/wcf/netmsmqbinding.md)|否|  
  
     下面的代码示例使用配置来指定一个名为 `wsHttpBinding_Calculator` 的绑定，该绑定使用 [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) 、消息安全和安全会话。  
  
    ```xml  
    <bindings>  
      <WSHttpBinding>  
       <binding name = "wsHttpBinding_Calculator">  
         <security mode="Message">  
           <message clientCredentialType="Windows"/>  
         </security>  
        </binding>  
      </WSHttpBinding>  
    </bindings>  
    ```  
  
     下面的代码示例指定 [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) 使用、消息安全和安全会话来保护 `secureCalculator` 服务。  
  
     [!code-csharp[c_CreateSecureSession#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_createsecuresession/cs/secureservice.cs#1)]
     [!code-vb[c_CreateSecureSession#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_createsecuresession/vb/secureservice.vb#1)]  
  
    > [!NOTE]
    > 可以 [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) 通过将属性设置为来关闭安全会话 `establishSecurityContext` `false` 。 对于其他系统提供的绑定，只能通过创建自定义绑定来关闭安全会话。  
  
### <a name="to-specify-that-a-service-uses-secure-sessions-by-using-a-custom-binding"></a>通过使用自定义绑定来指定服务使用安全会话  
  
- 创建一个自定义绑定，该绑定指定由安全会话保护 SOAP 消息。  
  
     有关创建自定义绑定的详细信息，请参阅 [如何：自定义 System-Provided 绑定](../extending/how-to-customize-a-system-provided-binding.md)。  
  
     下面的代码示例使用配置来指定使用安全会话的消息的自定义绑定。  
  
    ```xml  
    <bindings>  
      <!-- configure a custom binding -->  
      <customBinding>  
        <binding name="customBinding_Calculator">  
          <security authenticationMode="SecureConversation" />  
          <secureConversationBootstrap authenticationMode="SspiNegotiated" />  
          <textMessageEncoding messageVersion="Soap12WSAddressing10" writeEncoding="utf-8"/>  
          <httpTransport/>  
        </binding>  
      </customBinding>  
    </bindings>  
    ```  
  
     下面的代码示例创建一个自定义绑定，该绑定使用 <xref:System.ServiceModel.Configuration.AuthenticationMode.MutualCertificate> 身份验证模式启动安全会话。  
  
     [!code-csharp[c_CreateSecureSession#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_createsecuresession/cs/secureservice.cs#2)]
     [!code-vb[c_CreateSecureSession#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_createsecuresession/vb/secureservice.vb#2)]  
  
## <a name="see-also"></a>另请参阅

- [WCF 绑定概述](../bindings-overview.md)
