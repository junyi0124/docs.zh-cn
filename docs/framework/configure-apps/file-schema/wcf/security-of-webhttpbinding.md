---
title: <security> 的 <webHttpBinding>
ms.date: 03/30/2017
ms.assetid: 727cf3d2-6f56-48ad-a59f-ba423edb9c83
ms.openlocfilehash: 60b863a0a2a846a60dde2e4b323a305b5096b1cc
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91169890"
---
# <a name="security-of-webhttpbinding"></a>\<security> 的 \<webHttpBinding>

指定用配置的终结点的安全要求 [\<webHttpBinding>](webhttpbinding.md) 。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<webHttpBinding>**](webhttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<security>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.ServiceModel>
  <bindings>
    <webHttpBinding>
      <binding name = "String">
        <security mode="None/Transport/TransportCredentialOnly">
          <transport clientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
                     proxyCredentialType="Basic/Digest/None/Ntlm/Windows"
                     realm="String" />
        </security>
      </binding>
    </webHttpBinding>
  </bindings>
</system.ServiceModel>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|mode|指定终结点是使用传输级安全模式还是不使用安全模式。 默认为 `None`。 此属性的类型为 <xref:System.ServiceModel.WebHttpSecurityMode>。|  
  
## <a name="mode-attribute"></a>Mode 属性  
  
|值|描述|  
|-----------|-----------------|  
|无|禁用安全性。|  
|传输|使用 HTTPS 提供安全性。 此服务需要使用 SSL 证书进行配置。 消息使用 HTTPS 获得全面保护，而且客户端使用服务的 SSL 证书对服务进行身份验证。 客户端身份验证通过 `ClientCredentialType` 的属性进行控制 [\<transport>](transport-of-webhttpbinding.md) 。|  
|TransportCredentialOnly|此模式并不提供消息的完整性和保密性， 而是提供基于 HTTP 的客户端身份验证。 使用此模式时应当小心。 它应该用于通过其他 (方式（如 IPSec) ）提供传输安全的环境中，并且 WCF 基础结构只提供客户端身份验证。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<transport>](transport-of-webhttpbinding.md)|定义传输安全设置。 此元素与 <xref:System.ServiceModel.Configuration.HttpTransportSecurityElement> 类型相对应。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<webHttpBinding>](webhttpbinding.md)|一个绑定元素，用于为响应 HTTP 请求（而不是 SOAP 消息）的 Windows Communication Foundation (WCF) Web 服务配置终结点。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.WebHttpBindingElement>
- <xref:System.ServiceModel.Configuration.WSHttpSecurityElement>
- <xref:System.ServiceModel.WebHttpBinding.Security%2A>
- <xref:System.ServiceModel.Configuration.WebHttpBindingElement.Security%2A>
- <xref:System.ServiceModel.WebHttpSecurity>
- [保护服务和客户端的安全](../../../wcf/feature-details/securing-services-and-clients.md)
- [选择凭据类型](../../../wcf/feature-details/selecting-a-credential-type.md)
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
- [WCF Web HTTP 编程模型](../../../wcf/feature-details/wcf-web-http-programming-model.md)
