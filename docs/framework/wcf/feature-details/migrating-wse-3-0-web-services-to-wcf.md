---
title: 将 WSE 3.0 Web 服务迁移到 WCF
ms.date: 03/30/2017
ms.assetid: 7bc5fff7-a2b2-4dbc-86cc-ecf73653dcdc
ms.openlocfilehash: 84d227a46c4d17291ccf35a759018ffbe6f48b82
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96248157"
---
# <a name="migrating-wse-30-web-services-to-wcf"></a>将 WSE 3.0 Web 服务迁移到 WCF

将 WSE 3.0 Web 服务迁移到 Windows Communication Foundation (WCF) 的好处包括改进的性能和对其他传输、附加安全方案和 WS * 规范的支持。 从 WSE 3.0 迁移到 WCF 的 Web 服务的性能提高了200%，400从而提高了性能。 有关 WCF 支持的传输的详细信息，请参阅 [选择传输](choosing-a-transport.md)。 有关 WCF 支持的方案的列表，请参阅 [常见安全方案](common-security-scenarios.md)。 有关 WCF 支持的规范的列表，请参阅 [Web 服务协议互操作性指南](web-services-protocols-interoperability-guide.md)。  
  
 以下部分提供有关如何将 WSE 3.0 Web 服务的特定功能迁移到 WCF 的指导。  
  
## <a name="general"></a>常规  

 WSE 3.0 和 WCF 应用程序包括线路级互操作性和一组常用的术语。 WSE 3.0 和 WCF 应用程序是基于二者都支持的 WS-* 规范集的线路级互操作。 开发 WSE 3.0 或 WCF 应用程序时，有一组常用的术语，例如，WSE 和身份验证模式下的 "全包式安全声明" 的名称。  
  
 尽管 WCF 和 ASP.NET 或 WSE 3.0 编程模型之间存在很多相似的方面，但它们是不同的。 有关 WCF 编程模型的详细信息，请参阅 [基本编程生命周期](../basic-programming-lifecycle.md)。  
  
> [!NOTE]
> 若要将 WSE Web 服务迁移到 WCF，请使用 " [)  (" 元数据实用工具 "工具 ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 来生成客户端。 另一方面，该客户端还包含可以用作 WCF 服务起点的接口和类。 所生成的接口将 <xref:System.ServiceModel.OperationContractAttribute> 属性 (attribute) 应用于将 <xref:System.ServiceModel.OperationContractAttribute.ReplyAction%2A> 属性 (property) 设置为 `*` 的协定的成员。 当 WSE 客户端使用此设置调用 Web 服务时，将引发以下异常： **Services3。 ResponseProcessingException： WSE910：处理响应消息期间发生错误，你可以在内部异常中找到错误**。 若要缓解此问题，可以将 <xref:System.ServiceModel.OperationContractAttribute.ReplyAction%2A> 属性 (attribute) 的 <xref:System.ServiceModel.OperationContractAttribute> 属性 (property) 设置为非 `null` 值，例如 `http://Microsoft.WCF.Documentation/ResponseToOCAMethod`。  
  
## <a name="security"></a>安全性  
  
### <a name="wse-30-web-services-that-are-secured-using-a-policy-file"></a>使用策略文件保护其安全的 WSE 3.0 Web 服务  

 WCF 服务可以使用配置文件来保护服务的安全，该机制类似于 WSE 3.0 策略文件。 在 WSE 3.0 中，当使用策略文件来保护 Web 服务的安全时，可以使用关守安全断言或自定义策略断言。 交钥匙安全声明与 WCF 安全绑定元素的身份验证模式密切相关。 不只是 WCF 身份验证模式和 WSE 3.0 全包式安全断言，它们使用相同的凭据类型来保护消息的安全。 例如， `usernameForCertificate` WSE 3.0 中的交钥匙安全声明映射到 `UsernameForCertificate` WCF 中的身份验证模式。 下面的代码示例演示了在 WSE 3.0 中使用交钥匙安全断言的最小策略是如何 `usernameForCertificate` 映射到 `UsernameForCertificate` 自定义绑定中 WCF 的身份验证模式的。  
  
 **WSE 3.0**  
  
```xml  
<policies>  
  <policy name="MyPolicy">  
    <usernameForCertificate messageProtectionOrder="SignBeforeEncrypt"  
                            requireDeriveKeys="true"/>  
  </policy>  
</policies>  
```  
  
 **WCF**  
  
```xml  
<customBinding>  
  <binding name="MyBinding">  
    <security authenticationMode="UserNameForCertificate"
              messageProtectionOrder="SignBeforeEncrypt"  
              requireDerivedKeys="true"/>  
  </binding>  
</customBinding>  
```  
  
 若要将策略文件中指定的 WSE 3.0 Web 服务的安全设置迁移到 WCF，必须在配置文件中创建一个自定义绑定，并且必须将交钥匙安全声明设置为其等效的身份验证模式。 此外，当 WSE 3.0 客户端与服务进行通信时，必须将自定义绑定配置为使用 2004 年 8 月版 WS-Addressing 规范。 如果迁移的 WCF 服务不需要与 WSE 3.0 客户端通信，并且必须仅维护安全奇偶校验，请考虑使用具有适当安全设置的 WCF 系统定义的绑定，而不是创建自定义绑定。  
  
 下表列出了 WSE 3.0 策略文件与 WCF 中等效的自定义绑定之间的映射。  
  
|WSE 3.0 关守安全断言|WCF 自定义绑定配置|  
|----------------------------------------|--------------------------------------|  
|\<usernameOverTransportSecurity />|`<customBinding>   <binding name="MyBinding">     <security authenticationMode="UserNameOverTransport" />     <textMessageEncoding messageVersion="Soap12WSAddressingAugust2004" />   </binding> </customBinding>`|  
|\<mutualCertificate10Security />|`<customBinding>   <binding name="MyBinding">     <security messageSecurityVersion="WSSecurity10WSTrustFebruary2005WSSecureConversationFebruary2005WSSecurityPolicy11BasicSecurityProfile10" authenticationMode="MutualCertificate" />     <textMessageEncoding messageVersion="Soap12WSAddressingAugust2004" />   </binding> </customBinding>`|  
|\<usernameForCertificateSecurity />|`<customBinding>   <binding name="MyBinding">     <security authenticationMode="UsernameForCertificate"/>     <textMessageEncoding messageVersion="Soap12WSAddressingAugust2004" />   </binding> </customBinding>`|  
|\<anonymousForCertificateSecurity />|`<customBinding>   <binding name="MyBinding">     <security authenticationMode="AnonymousForCertificate"/>     <textMessageEncoding messageVersion="Soap12WSAddressingAugust2004" />   </binding> </customBinding>`|  
|\<kerberosSecurity />|`<customBinding>   <binding name="MyBinding">     <security authenticationMode="Kerberos"/>     <textMessageEncoding messageVersion="Soap12WSAddressingAugust2004" />   </binding> </customBinding>`|  
|\<mutualCertificate11Security />|`<customBinding>   <binding name="MyBinding">     <security authenticationMode="MutualCertificate"/>     <textMessageEncoding messageVersion="Soap12WSAddressingAugust2004" />   </binding> </customBinding>`|  
  
 有关在 WCF 中创建自定义绑定的详细信息，请参阅 [自定义绑定](../extending/custom-bindings.md)。  
  
### <a name="wse-30-web-services-that-are-secured-using-application-code"></a>使用应用程序代码保护其安全的 WSE 3.0 Web 服务  

 无论是使用 WSE 3.0 还是使用 WCF，都可以在应用程序代码而不是配置中指定安全要求。 在 WSE 3.0 中，可以通过以下方法完成此工作：创建一个派生自 `Policy` 类的类，然后通过调用 `Add` 方法添加相应的要求。 有关在代码中指定安全要求的详细信息，请参阅 [如何：在不使用策略文件的情况下保护 Web 服务](/previous-versions/dotnet/netframework-2.0/aa528763(v=msdn.10))。 在 WCF 中，若要在代码中指定安全要求，请创建类的一个实例， <xref:System.ServiceModel.Channels.BindingElementCollection> 然后将的一个实例添加 <xref:System.ServiceModel.Channels.SecurityBindingElement> 到中 <xref:System.ServiceModel.Channels.BindingElementCollection> 。 使用 <xref:System.ServiceModel.Channels.SecurityBindingElement> 类的静态身份验证模式帮助器方法来设置安全断言需求。 有关使用 WCF 在代码中指定安全要求的详细信息，请参阅 [如何：使用 SecurityBindingElement 创建自定义绑定](how-to-create-a-custom-binding-using-the-securitybindingelement.md) 和 [如何：为指定的身份验证模式创建 SecurityBindingElement](how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md)。  
  
### <a name="wse-30-custom-policy-assertion"></a>WSE 3.0 自定义策略断言  

 在 WSE 3.0 中，有两种类型的自定义策略断言：保护 SOAP 消息安全的策略断言和不保护 SOAP 消息安全的策略断言。 保护 SOAP 消息安全的策略断言派生自 WSE 3.0 `SecurityPolicyAssertion` 类，WCF 中的概念等效项为 <xref:System.ServiceModel.Channels.SecurityBindingElement> 类。  
  
 需要注意的一点是，WSE 3.0 交钥匙安全声明是 WCF 身份验证模式的子集。 如果在 WSE 3.0 中创建了自定义策略断言，则可能会有等效的 WCF 身份验证模式。 例如，WSE 3.0 没有提供与 `UsernameOverTransport` 关守安全断言等效的 CertificateOverTransport 安全断言，但使用 X.509 证书进行客户端身份验证。 如果已为此方案定义了自己的自定义策略断言，则 WCF 会使迁移非常简单。 WCF 为此方案定义了身份验证模式，因此您可以利用静态身份验证模式帮助器方法来配置 WCF <xref:System.ServiceModel.Channels.SecurityBindingElement> 。  
  
 如果没有与用于保护 SOAP 消息的自定义策略断言等效的 WCF 身份验证模式，则从或 WCF 类中派生一个类， <xref:System.ServiceModel.Channels.TransportSecurityBindingElement> <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> 并指定等效的绑定元素。 有关更多详细信息，请参阅 [如何：使用 SecurityBindingElement 创建自定义绑定](how-to-create-a-custom-binding-using-the-securitybindingelement.md)。  
  
 若要转换不确保 SOAP 消息安全的自定义策略断言，请参阅 [筛选](filtering.md) 和 [自定义消息侦听器](../samples/custom-message-interceptor.md)示例。  
  
### <a name="wse-30-custom-security-token"></a>WSE 3.0 自定义安全令牌  

 用于创建自定义令牌的 WCF 编程模型与 WSE 3.0 不同。 有关在 WSE 中创建自定义令牌的详细信息，请参阅 [创建自定义安全令牌](/previous-versions/dotnet/netframework-2.0/aa529304(v=msdn.10))。 有关在 WCF 中创建自定义令牌的详细信息，请参阅 [如何：创建自定义令牌](../extending/how-to-create-a-custom-token.md)。  
  
### <a name="wse-30-custom-token-manager"></a>WSE 3.0 自定义令牌管理器  

 在 WCF 中创建自定义令牌管理器的编程模型与 WSE 3.0 不同。 有关如何创建自定义令牌管理器和自定义安全令牌所需的其他组件的详细信息，请参阅 [如何：创建自定义令牌](../extending/how-to-create-a-custom-token.md)。  
  
> [!NOTE]
> 如果已创建自定义 `UsernameToken` 安全令牌管理器，WCF 提供的机制比创建自定义安全令牌管理器更容易指定身份验证逻辑。 有关更多详细信息，请参阅 [如何：使用自定义用户名和密码验证](how-to-use-a-custom-user-name-and-password-validator.md)程序。  
  
### <a name="wse-30-web-services-that-use-mtom-encoded-soap-messages"></a>使用 MTOM 编码 SOAP 消息的 WSE 3.0 Web 服务  

 与 WSE 3 应用程序一样，WCF 应用程序可以在配置中指定 MTOM 消息编码。 若要迁移此设置，请将添加 [\<mtomMessageEncoding>](../../configure-apps/file-schema/wcf/mtommessageencoding.md) 到服务的绑定中。 下面的代码示例演示如何在 WSE 3.0 中为等效于 WCF 的服务指定 MTOM 编码。  
  
 **WSE 3.0**  
  
```xml  
<messaging>  
    <mtom clientMode="On"/>  
</messaging>  
```  
  
 **WCF**  
  
```xml  
<customBinding>  
  <binding name="MyBinding">  
    <mtomMessageEncoding/>  
  </binding>  
</customBinding>  
```  
  
## <a name="messaging"></a>Messaging  
  
### <a name="wse-30-applications-that-use-the-wse-messaging-api"></a>使用 WSE 消息 API 的 WSE 3.0 应用程序  

 当使用 WSE 消息 API 来获取对在客户端和服务之间交换的 XML 的直接访问时，可以将应用程序转换为使用“Plain Old XML”(POX)。 有关 POX 的更多详细信息，请参阅 [与 POX 应用程序的互操作性](interoperability-with-pox-applications.md)。 有关 WSE 消息传递 API 的更多详细信息，请参阅 [使用 Wse 消息传递 Api 发送和接收 SOAP 消息](/previous-versions/dotnet/netframework-2.0/aa529293(v=msdn.10))。  
  
## <a name="transports"></a>传输  
  
### <a name="tcp"></a>TCP  

 默认情况下，使用 TCP 传输发送 SOAP 消息的 WSE 3.0 客户端和 Web 服务不能与 WCF 客户端和 Web 服务进行互操作。 这一不兼容性是由于 TCP 协议中使用的组帧存在差异以及性能方面的原因。 不过，WCF 示例详细说明了如何实现与 WSE 3.0 互操作的自定义 TCP 会话。 有关此示例的详细信息，请参阅 [传输： WSE 3.0 TCP 互操作性](../samples/transport-wse-3-0-tcp-interoperability.md)。  
  
 若要指定 WCF 应用程序使用 TCP 传输，请使用 [\<netTcpBinding>](../../configure-apps/file-schema/wcf/nettcpbinding.md) 。  
  
### <a name="custom-transport"></a>自定义传输协议  

 WCF 中的 WSE 3.0 自定义传输的等效项是通道扩展。 有关创建通道扩展的详细信息，请参阅 [扩展通道层](../extending/extending-the-channel-layer.md)。  
  
## <a name="see-also"></a>另请参阅

- [基本编程生命周期](../basic-programming-lifecycle.md)
- [自定义绑定](../extending/custom-bindings.md)
- [如何：使用 SecurityBindingElement 创建自定义绑定](how-to-create-a-custom-binding-using-the-securitybindingelement.md)
- [如何：为指定的身份验证模式创建 SecurityBindingElement](how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md)
