---
title: <security> 的 <basicHttpBinding>
ms.date: 03/30/2017
ms.assetid: 6432708d-5465-4bd9-bfc2-466742db99cb
ms.openlocfilehash: 6144e5448526d7f2a7c89693f70f71a7f26c4a22
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91183658"
---
# <a name="security-of-basichttpbinding"></a>\<security> 的 \<basicHttpBinding>

定义的安全功能 [\<basicHttpBinding>](basichttpbinding.md) 。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<basicHttpBinding>**](basichttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<security>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<security mode="Message/None/Transport/TransportWithCredential">
  <transport clientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
             proxyCredentialType="Basic/Digest/None/Ntlm/Windows"
             realm="string" />
  <message algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
           clientCredentialType="Certificate/IssuedToken/None/UserName/Windows" />
</security>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 以下几节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|mode|可选。 指定所使用的安全类型。 默认为 `None`。 此属性的类型为 <xref:System.ServiceModel.BasicHttpSecurityMode>。|  
  
## <a name="mode-attribute"></a>mode 属性  
  
|值|描述|  
|-----------|-----------------|  
|无|-传输过程中消息不受保护。|  
|传输|使用 HTTPS 传输提供安全性。 使用 HTTPS 对 SOAP 消息进行保护。 使用服务的 X.509 证书向客户端对服务进行身份验证。 使用所提供的 ClientCredentialType 对客户端进行身份验证。 请参阅 [\<transport>](transport-of-basichttpbinding.md) 。|  
|消息|使用 SOAP 消息安全提供安全性。 默认情况下，将对正文进行加密和签名。 对于此绑定，系统要求向带外客户端提供服务器证书。 此绑定仅有的有效 `ClientCredentialType` 为 `Certificate`。|  
|TransportWithMessageCredential|完整性、保密性和服务器身份验证由传输安全来提供。 客户端身份验证采用 SOAP 消息安全方式提供。 如果要使用用户名/密码对用户进行身份验证，并且存在用于保护消息传输的现有 HTTP 部署，则适用此模式。|  
|TransportCredentialOnly|此模式并不提供消息的完整性和保密性， 而是提供基于 http 的客户端身份验证。 使用此模式时应当小心。 它应该用于通过其他 (方式（如 IPSec) ）提供传输安全的环境中，并且 WCF 基础结构只提供客户端身份验证。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<transport>](transport-of-basichttpbinding.md)|定义基本 HTTP 服务的传输安全设置。 此元素与 <xref:System.ServiceModel.HttpTransportSecurity> 相对应。|  
|[\<message>](message-of-basichttpbinding.md)|定义基本 HTTP 服务的消息安全设置。 此元素与 <xref:System.ServiceModel.BasicHttpMessageSecurity> 相对应。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|binding|的绑定元素 [\<basicHttpBinding>](basichttpbinding.md) 。|  
  
## <a name="remarks"></a>备注  

 默认情况下，不会对 SOAP 消息进行保护，也不会对客户端进行身份验证。 使用此元素，您可以配置 `basicHttpBinding` 元素的其他安全设置。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.BasicHttpBinding.Security%2A>
- <xref:System.ServiceModel.Configuration.BasicHttpBindingElement.Security%2A>
- <xref:System.ServiceModel.Configuration.BasicHttpSecurityElement>
- <xref:System.ServiceModel.BasicHttpSecurity>
- [保护服务和客户端的安全](../../../wcf/feature-details/securing-services-and-clients.md)
- [选择凭据类型](../../../wcf/feature-details/selecting-a-credential-type.md)
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
