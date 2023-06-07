---
title: <security> 的元素 <ws2007FederationHttpBinding>
ms.date: 03/30/2017
ms.assetid: 826219b4-3a16-45fc-832d-0cd7cbbd3b84
ms.openlocfilehash: 943ccc241aef15b58661699408b085d98cf86c3b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91183697"
---
# <a name="security-element-of-ws2007federationhttpbinding"></a>\<security> 的元素 \<ws2007FederationHttpBinding>

定义元素的安全设置 [\<ws2007FederationHttpBinding>](ws2007federationhttpbinding.md) 。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<ws2007FederationHttpBinding>**](ws2007federationhttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<security>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<ws2007FederationBinding>
  <binding>
    <security mode="None/Message/TransportWithMessageCredential">
      <message negotiateServiceCredential="Boolean"
               algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/  Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
               defaultProtectionLevel="none/sign/EncryptAndSign"
               issuedTokenType="string"
               issuedKeyType="SymmetricKey/PublicKey">
      </message>
    </security>
  </binding>
</ws2007FederationBinding>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`mode`|可选。 指定所应用的安全类型。 默认值是 `Message`。 此属性的类型为 <xref:System.ServiceModel.WSFederationHttpSecurityMode>。|  
  
## <a name="mode-attribute"></a>mode 属性  
  
|值|描述|  
|-----------|-----------------|  
|无|SOAP 消息在传输过程中并不安全。|  
|消息|通过使用 SOAP 消息安全，可以提供完整性、保密性、服务器身份验证和客户端身份验证。 默认情况下，将对正文进行加密和签名。 此服务必须使用证书进行配置。 客户端根据由安全令牌服务颁发给客户端的令牌进行身份验证。|  
|TransportWithMessageCredential|完整性、保密性和服务器身份验证均由 HTTPS 提供。 此服务必须使用证书进行配置。 客户端身份验证采用 SOAP 消息安全方式提供，并根据由安全令牌服务颁发给客户端的令牌进行。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<message>](message-of-ws2007httpbinding.md)|定义消息级安全性设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.FederatedMessageSecurityOverHttpElement>。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<binding>](bindings.md)|定义的所有绑定功能 [\<wsDualHttpBinding>](wsdualhttpbinding.md) 。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.WSFederationHttpSecurity>
- <xref:System.ServiceModel.WSFederationHttpBinding.Security%2A>
- <xref:System.ServiceModel.Configuration.WSFederationHttpBindingElement.Security%2A>
- <xref:System.ServiceModel.Configuration.WSFederationHttpSecurityElement>
- [如何：创建 WSFederationHttpBinding](../../../wcf/feature-details/how-to-create-a-wsfederationhttpbinding.md)
- [保护服务和客户端的安全](../../../wcf/feature-details/securing-services-and-clients.md)
- [选择凭据类型](../../../wcf/feature-details/selecting-a-credential-type.md)
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
