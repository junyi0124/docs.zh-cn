---
title: <security> 的 <wsHttpBinding>
ms.date: 03/30/2017
ms.assetid: 8658b162-2ddf-4162-a869-aa517a42288a
ms.openlocfilehash: 9f984759fb52242bf8030a101b567c14627dd314
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91158690"
---
# <a name="security-of-wshttpbinding"></a>\<security> 的 \<wsHttpBinding>

表示的安全功能 [\<wsHttpBinding>](wshttpbinding.md) 。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<wsHttpBinding>**](wshttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<security>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<security mode="Message/None/Transport/TransportWithMessageCredential">
  <transport clientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
             proxyCredentialType="Basic/Digest/None/Ntlm/Windows"
             realm="String"
             defaultClientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
             defaultProxyCredentialType="Basic/Digest/None/Ntlm/Windows"
             defaultRealm="String" />
  <message clientCredentialType="Certificate/IssuedToken/None/UserName/Windows"
           algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
           establishSecurityContext="Boolean"
           negotiateServiceCredential="Boolean" />
</security>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 以下几节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|mode|可有可无. 指定所应用的安全类型。 默认为 `Message`。<br />-此属性的类型为 <xref:System.ServiceModel.SecurityMode> 。|  
  
## <a name="mode-attribute"></a>Mode 属性  
  
|值|描述|  
|-----------|-----------------|  
|无|禁用安全性。|  
|传输|使用 HTTPS 提供安全性。 此服务需要使用 SSL 证书进行配置。 消息使用 HTTPS 得到了全面保护，客户端使用服务的 SSL 证书对消息进行身份验证。 客户端身份验证通过 的 [\<transport>](transport-of-wshttpbinding.md) 。|  
|消息|使用 SOAP 消息安全提供安全性。 默认情况下，将对 SOAP 正文进行加密和签名。 此模式提供了各种各样的功能，例如服务凭据在带外客户端是否可用、要使用的算法套件以及通过 Security.Message 属性要应用于消息正文的保护级别。 每个会话将执行一次客户端身份验证，身份验证的结果在会话过程中将被缓存。|  
|TransportWithMessageCredential|在此模式下，HTTPS 将提供完整性、保密性和服务器身份验证，SOAP 消息安全将提供客户端身份验证。 默认情况下，每个会话将执行一次客户端身份验证，身份验证的结果在会话过程中将被缓存。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<transport>](transport-of-wshttpbinding.md)|定义传输安全设置。 此元素与 <xref:System.ServiceModel.Configuration.HttpTransportSecurityElement> 类型相对应。|  
|[\<message>](message-of-wshttpbinding.md)|定义消息的安全设置。 此元素与 <xref:System.ServiceModel.Configuration.MessageSecurityOverHttpElement> 类型相对应。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<wsHttpBinding>](wshttpbinding.md)|HTTP 传输应用程序的安全绑定。|  
  
## <a name="remarks"></a>备注  

 WSHttpBinding 类专用于与实现 WS-* 规范的服务进行互操作。 此绑定的传输安全为 HTTP 上的安全套接字层 (SSL)，即 HTTPS。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.WSHttpSecurity>
- <xref:System.ServiceModel.WSHttpBinding.Security%2A>
- <xref:System.ServiceModel.Configuration.WSHttpBindingElement.Security%2A>
- <xref:System.ServiceModel.Configuration.WSHttpSecurityElement>
- [保护服务和客户端的安全](../../../wcf/feature-details/securing-services-and-clients.md)
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
