---
title: 系统提供的互操作性绑定支持的 Web 服务协议
ms.date: 03/30/2017
helpviewer_keywords:
- WS-protocols
- Web services protocols
- Windows Communication Foundation, Web service protocols
ms.assetid: 1f7fc4ff-30fe-4e46-adda-91caad3b06c6
ms.openlocfilehash: 0dbfca5d56fe588864a371f4a81bbefbdfeb284c
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96238972"
---
# <a name="web-services-protocols-supported-by-system-provided-interoperability-bindings"></a>系统提供的互操作性绑定支持的 Web 服务协议

Windows Communication Foundation 生成 (WCF) 与支持一组称为 Web 服务规范的规范的 Web 服务进行互操作。 为了简化互操作性最佳做法的服务配置，WCF 引入了三个可互操作的系统提供绑定： <xref:System.ServiceModel.BasicHttpBinding?displayProperty=nameWithType> 、 <xref:System.ServiceModel.WSHttpBinding?displayProperty=nameWithType> 和 <xref:System.ServiceModel.WSDualHttpBinding?displayProperty=nameWithType> 。 为了实现与组织的互操作性， (OASIS) 标准，WCF 包括一个可互操作的系统提供的绑定： <xref:System.ServiceModel.WS2007HttpBinding?displayProperty=nameWithType> 。 对于元数据发布，WCF 包括两个可互操作的系统提供的绑定： [\<mexHttpBinding>](../../configure-apps/file-schema/wcf/mexhttpbinding.md) 和 [\<mexHttpsBinding>](../../configure-apps/file-schema/wcf/mexhttpsbinding.md) 。 本主题列出系统提供的可互操作绑定支持的规范。  
  
## <a name="web-services-protocols-supported-by-basichttpbinding-wshttpbinding-ws2007httpbinding-and-wsdualhttpbinding-bindings"></a>basicHttpBinding、wsHttpBinding、ws2007HttpBinding 和 wsDualHttpBinding 绑定支持的 Web 服务协议  
  
### <a name="all-bindings"></a>所有绑定  

 [\<basicHttpBinding>](../../configure-apps/file-schema/wcf/basichttpbinding.md)、 [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) 和 [\<ws2007HttpBinding>](../../configure-apps/file-schema/wcf/ws2007httpbinding.md) 绑定支持以下协议。  
  
> [!NOTE]
> 有关用于发布元数据的绑定的信息，请参见本主题后面的“系统提供的元数据绑定”一节。  
  
|类别|协议|规范和用法|  
|--------------|--------------|-----------------------------|  
|Transport|HTTP 1.1|[HTTP 1.1](https://www.ietf.org/rfc/rfc2616.txt)<br /><br /> `BasicHttpBinding`、`WSHttpBinding` 和 `WS2007HttpBinding` 使用 HTTP 和 HTTPS 传输。|  
|Messaging|MTOM|[MTOM](https://www.w3.org/TR/soap12-mtom/)<br /><br /> `basicHttpBinding`、`wsHttpBinding` 和 `ws2007HttpBinding` 支持消息传输优化机制 (MTOM)。 默认情况下不使用。 若要使用 MTOM，请将 `messageEncoding` 属性设置为 `"Mtom"`。<br /><br /> 例如：<br /><br /> `<wsHttpBinding> <binding messageEncoding="Mtom"/> </wsHttpBinding>`|  
|元数据|WSDL 1.1|[WSDL 1.1](https://www.w3.org/TR/wsdl/)<br /><br /> WCF 使用 Web 服务描述语言 (WSDL) 来描述服务。|  
|元数据|WS-Policy|[WS-Policy](https://www.w3.org/Submission/WS-Policy/)<br /><br /> WCF 将 WS-Policy 规范与域特定断言结合使用，以描述服务要求和功能。|  
|元数据|WS-Policy 1.5|[WS-Policy 1.5](https://www.w3.org/TR/2007/CR-ws-policy-20070605/)<br /><br /> WCF 将 WS-Policy 规范与域特定断言结合使用，以描述服务要求和功能。|  
|元数据|WS-PolicyAttachment|[WS-PolicyAttachment](http://specs.xmlsoap.org/ws/2004/09/policy/ws-policyattachment.pdf)<br /><br /> WCF 实现 WS-PolicyAttachment 在 Web 服务描述语言 (WSDL) 的不同范围内附加策略表达式。|  
|元数据|WS-MetadataExchange|[WS-MetadataExchange](http://specs.xmlsoap.org/ws/2004/09/mex/WS-MetadataExchange.pdf)<br /><br /> WCF 实现 WS-MetadataExchange 来检索 XML 架构、WSDL 和 WS 策略。|  
  
### <a name="basichttpbinding"></a>basicHttpBinding  
  
|类别|协议|规范和用法|  
|--------------|--------------|-----------------------------|  
|Messaging|SOAP 1.1|[SOAP 1.1](https://www.w3.org/TR/2000/NOTE-SOAP-20000508/)<br /><br /> `basicHttpBinding` 元素根据基本配置文件 1.1 实现 SOAP 1.1 消息协议。|  
|安全性|WSS SOAP Message Security 1.0（WSS SOAP 消息安全 1.0）|[WSS SOAP Message Security 1.0（WSS SOAP 消息安全 1.0）](http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-soap-message-security-1.0.pdf)<br /><br /> `basicHttpBinding` 元素根据基本安全配置文件为用户名/密码和基于 X.509 的安全实现 Web 服务安全 (WSS) SOAP 消息安全 1.0 规范。<br /><br /> `<basicHttpBinding> <binding name="Binding1"> <security mode="TransportWithMessageCredential &#124;                     "Message" .../> </binding> </basicHttpBinding>`|  
|安全性|WSS SOAP 消息安全用户名令牌配置文件 1.0|[WSS SOAP 消息安全用户名令牌配置文件 1.0](http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-username-token-profile-1.0.pdf)<br /><br /> `<basicHttpBinding> <binding name="Binding1"> <security mode="TransportWithMessageCredential"> <transport clientCredentialType="Basic"/> </security> </basicHttpBinding>`|  
|安全性|WSS SOAP Message Security x.509 证书令牌配置文件1。0|[WSS SOAP Message Security x.509 证书令牌配置文件1。0](http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-x509-token-profile-1.0.pdf)<br /><br /> `<basicHttpBinding>   <security mode="Message"> <message clientCredentialType="Certificate"/> </security> </basicHttpBinding>`|  
  
### <a name="wshttpbinding-ws2007httpbinding-and-wsdualhttpbinding"></a>wsHttpBinding、ws2007HttpBinding 和 wsDualHttpBinding  
  
|类别|协议|规范和用法|  
|--------------|--------------|-----------------------------|  
|Messaging|SOAP 1.2|[入门](https://www.w3.org/TR/soap12-part0/)<br /><br /> [Messaging framework（消息传送框架）](https://www.w3.org/TR/2007/REC-soap12-part1-20070427/)<br /><br /> [附属（包括 HTTP 绑定）](https://www.w3.org/TR/soap12-part2/)|  
|Messaging|WS-Addressing 2005/08|[Web 服务寻址 1.0 – 核心（可能为英文网页）](https://www.w3.org/TR/ws-addr-core/)<br /><br /> [Web Services Addressing 1.0 - SOAP](https://www.w3.org/TR/ws-addr-soap/)<br /><br /> `wsHttpBinding`、`ws2007HttpBinding` 和 `wsDualHttpBinding` 实现万维网联合会 (W3C) WS-Addressing 建议以启用异步消息传送、消息关联和非特定传输寻址机制。<br /><br /> WCF 不支持对 WS-Addressing 标头进行加密，尽管 WS-＊ 规范允许这样做。|  
|Messaging|WS-Addressing 1.0 - 元数据|[Ws-addressing 1.0 元数据](https://www.w3.org/2007/05/addressing/metadata/) 通过设置 ServiceMetadata 行为中的策略版本来启用对此协议的支持-将 policyversion 设置为 1.2 (默认的) ，wsdl 说明符合 WS-Addressing wsdl，policyversion 设置为1.5，wsdl 说明符合 ws-addressing metadata。<br /><br /> WCF 不支持对 WS-Addressing 标头进行加密，尽管 WS-＊ 规范允许这样做。|  
|安全性|WSS SOAP Message Security 1.0（WSS SOAP 消息安全 1.0）|[WSS SOAP Message Security 1.0（WSS SOAP 消息安全 1.0）](http://docs.oasis-open.org/wss/2004/01/oasis-200401-wss-soap-message-security-1.0.pdf)<br /><br /> 当 `securityMode` 属性设置为“wsSecurityOverHttp”（默认值）并使用 `wsSecurity` 子元素配置了参数时使用。<br /><br /> `<wsHttpBinding>   <binding name="myBinding">      <security mode="Message" .../>   </binding> </wsHttpBinding>`|  
|安全性|WSS SOAP Message Security UsernameToken Profile 1。1|[WSS SOAP 消息安全用户名令牌配置文件 1.0](https://www.oasis-open.org/committees/download.php/16782/wss-v1.1-spec-os-UsernameTokenProfile.pdf)<br /><br /> 当 `wsSecurity` 元素的 `authenticationMode` 属性设置为“Username”时使用。<br /><br /> `<wsHttpBinding>   <binding name="MyBinding">     <security mode="Message>       <message           clientCredentialType="UserName        negotiateServiceCredential="false"        establishSecurityContext="false"/>     </security> </binding> </wsHttpBinding>`|  
|安全性|WSS SOAP Message Security X.509 Certificate Token Profile 1.1（WSS SOAP 消息安全 X.509 证书令牌配置文件 1.1）|[WSS SOAP Message Security X.509 Certificate Token Profile 1.1（WSS SOAP 消息安全 X.509 证书令牌配置文件 1.1）](https://www.oasis-open.org/committees/download.php/16785/wss-v1.1-spec-os-x509TokenProfile.pdf)<br /><br /> 当 `wsSecurity` 元素的 `authenticationMode` 属性设置为“Username”、“Certificate”或“None”时用于消息保护。 另外，当 `wsSecurity` 元素的 `authenticationMode` 属性设置为“Certificate”时用于客户端身份验证。<br /><br /> `<wsHttpBinding>   <binding name="MyBinding">     <security mode="Message>       <message           clientCredentialType="Certificate"        negotiateServiceCredential="false"        establishSecurityContext="false"/>     </security>   </binding> </wsHttpBinding>`|  
|安全性|WSS SOAP 消息安全 Kerberos 令牌配置文件 1.1|[WSS SOAP 消息安全 Kerberos 令牌配置文件1。1](https://www.oasis-open.org/committees/download.php/16788/wss-v1.1-spec-os-KerberosTokenProfile.pdf)<br /><br /> 当 `wsSecurity` 元素的 `authenticationMode` 属性设置为“Windows”时用于身份验证和消息保护。<br /><br /> `<wsHttpBinding>   <binding name="MyBinding">     <security mode="Message>       <message           clientCredentialType="Windows"        negotiateServiceCredential="false"        establishSecurityContext="false"/>     </security>   </binding> </wsHttpBinding>`|  
|安全性|WS-SecureConversation|[WS-SecureConversation](http://specs.xmlsoap.org/ws/2005/02/sc/ws-secureconversation.pdf)<br /><br /> 当 `security/@mode` 属性设置为“Message”且 `message/@establishSecurityContext` 属性设置为“true”（默认值）时用于提供安全会话。|  
|安全性|WS-Trust|[WS-TRUST](http://specs.xmlsoap.org/ws/2005/02/trust/ws-trust.pdf)<br /><br /> 由 WS-SecureConversation 使用（参见上面）。|  
|可靠消息传递|WS-ReliableMessaging|[WS-ReliableMessaging](http://specs.xmlsoap.org/ws/2005/02/rm/ws-reliablemessaging.pdf)<br /><br /> 当绑定配置为使用 `reliableSession` 时使用。<br /><br /> `<wsHttpBinding>  <binding name="myBinding">    <reliableSession/>   </binding> </wsHttpBinding>`|  
|事务|WS-AtomicTransaction|[WS-AtomicTransaction](http://specs.xmlsoap.org/ws/2004/10/wsat/wsat.pdf)<br /><br /> 用于事务管理器之间的通信。 WCF 客户端和服务始终使用本地事务管理器。|  
|事务|WS-Coordination|[WS-Coordination](/previous-versions/ms951231(v=msdn.10))<br /><br /> 当 `flowTransactions` 属性设置为“Allowed”或“Required”时用于对事务上下文进行流处理。<br /><br /> `<wsHttpBinding>   <binding transactionFlow="true"/> </wsHttpBinding>`|  
  
## <a name="wsfederationhttpbinding-and-ws2007federationhttpbinding"></a>wsFederationHttpBinding 和 ws2007FederationHttpBinding  

 [\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md)引入和 [\<ws2007FederationHttpBinding>](../../configure-apps/file-schema/wcf/ws2007federationhttpbinding.md) 元素是为了为联合方案提供支持，其中第三方颁发了用于对客户端进行身份验证的令牌。 除了 `wsHttpBinding` 使用的协议以外，`wsFederationHttpBinding` 还使用：  
  
- 用于令牌颁布的 `WS-Trust`。  
  
- 用于已颁发令牌最常见格式的 WSS 安全断言标记语言 (SAML) 令牌配置文件 1.0 和 1.1。  
  
 例如：  
  
```xml  
<wsFederationHttpBinding>  
  <binding name="myBinding">  
     <security mode="Message">  
       <message issuedKeyType="Symmetric"
                issuedTokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1">  
         <issuerMetadata address =
         'http://localhost/FederationSample/HomeRealmSTS/STS.svc/mex'/>  
       </message>  
     </security>  
  </binding>  
</wsFederationHttpBinding>  
```  
  
 有关详细信息，请参阅 [联合](federation.md) 。  
  
## <a name="system-provided-metadata-bindings"></a>系统提供的元数据绑定  

 下表说明系统提供的可互操作元数据绑定（由 <xref:System.ServiceModel.Description.MetadataExchangeBindings?displayProperty=nameWithType> 类公开）支持的协议。  
  
### <a name="mexhttpbinding"></a>mexHttpBinding  

 [\<mexHttpBinding>](../../configure-apps/file-schema/wcf/mexhttpbinding.md)绑定支持以下协议。 有关使用此绑定的详细信息，请参阅 [发布元数据](publishing-metadata.md)。  
  
|类别|协议|规范和用法|  
|--------------|--------------|-----------------------------|  
|Transport|HTTP 1.1|[HTTP 1.1](https://www.ietf.org/rfc/rfc2616.txt)|  
|Messaging|SOAP 1.2|[入门](https://www.w3.org/TR/soap12-part0/)<br /><br /> [Messaging framework（消息传送框架）](https://www.w3.org/TR/2007/REC-soap12-part1-20070427/)<br /><br /> [附属（包括 HTTP 绑定）](https://www.w3.org/TR/soap12-part2/)|  
|Messaging|WS-Addressing 2005/08|[Web 服务寻址 1.0 – 核心（可能为英文网页）](https://www.w3.org/TR/ws-addr-core/)<br /><br /> [Web Services Addressing 1.0 - SOAP](https://www.w3.org/TR/ws-addr-soap/)|  
|元数据|WS-MetadataExchange|[WS-MetadataExchange](http://specs.xmlsoap.org/ws/2004/09/mex/WS-MetadataExchange.pdf)<br /><br /> WCF 实现 WS-MetadataExchange 来检索 XML 架构、WSDL 和 WS 策略。|  
  
### <a name="mexhttpsbinding"></a>mexHttpsBinding  

 [\<mexHttpsBinding>](../../configure-apps/file-schema/wcf/mexhttpsbinding.md) 支持以下协议。 有关使用此绑定的详细信息，请参阅 [发布元数据](publishing-metadata.md)。  
  
|类别|协议|规范和用法|  
|--------------|--------------|-----------------------------|  
|Transport|HTTP 1.1|[HTTP 1.1](https://www.ietf.org/rfc/rfc2616.txt)<br /><br /> 启用传输安全。|  
|Messaging|SOAP 1.2|[入门](https://www.w3.org/TR/soap12-part0/)<br /><br /> [Messaging framework（消息传送框架）](https://www.w3.org/TR/2007/REC-soap12-part1-20070427/)<br /><br /> [附属（包括 HTTP 绑定）](https://www.w3.org/TR/soap12-part2/)|  
|Messaging|WS-Addressing 2005/08|[Web 服务寻址 1.0 – 核心（可能为英文网页）](https://www.w3.org/TR/ws-addr-core/)<br /><br /> [Web Services Addressing 1.0 - SOAP](https://www.w3.org/TR/ws-addr-soap/)|  
|元数据|WS-MetadataExchange|[WS-MetadataExchange](http://specs.xmlsoap.org/ws/2004/09/mex/WS-MetadataExchange.pdf)<br /><br /> WCF 实现 WS-MetadataExchange 来检索 XML 架构、WSDL 和 WS 策略。|  
  
## <a name="see-also"></a>另请参阅

- [系统提供的绑定](../system-provided-bindings.md)
- [\<basicHttpBinding>](../../configure-apps/file-schema/wcf/basichttpbinding.md)
- [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md)
- [\<wsDualHttpBinding>](../../configure-apps/file-schema/wcf/wsdualhttpbinding.md)
- [\<mexHttpsBinding>](../../configure-apps/file-schema/wcf/mexhttpsbinding.md)
- [\<mexHttpBinding>](../../configure-apps/file-schema/wcf/mexhttpbinding.md)
