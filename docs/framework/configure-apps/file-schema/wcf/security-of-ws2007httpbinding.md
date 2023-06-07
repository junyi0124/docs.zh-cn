---
title: <security> 的 <ws2007HttpBinding>
ms.date: 03/30/2017
ms.assetid: fdda0ff7-b462-4e26-af52-e87ddab71945
ms.openlocfilehash: 48b49bf69f791f90ed5b2eea8e6d412438cd9519
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91169838"
---
# <a name="security-of-ws2007httpbinding"></a>\<security> 的 \<ws2007HttpBinding>

表示与元素一起使用的安全设置 [\<ws2007HttpBinding>](ws2007httpbinding.md) 。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<ws2007HttpBinding>**](ws2007httpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<security>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.serviceModel>
  <bindings>
    <ws2007HttpBinding>
      <binding name = "String">
        <security mode="None/Message/Transport/TransportWithMessageCredential">
          <transport>
          </transport>
          <message>
          </message>
        </security>
      </binding>
    </ws2007HttpBinding>
  </bindings>
</system.ServiceModel>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`mode`|可有可无. 指定所应用的安全类型。 默认为 `Message`。<br /><br /> 此属性的类型为 <xref:System.ServiceModel.SecurityMode>。|  
  
## <a name="mode-attribute"></a>Mode 属性  
  
|值|描述|  
|-----------|-----------------|  
|`None`|禁用安全性。|  
|`Transport`|使用 HTTPS 提供安全性。 此服务必须使用安全套接字层 (SSL) 证书进行配置。 消息使用 HTTPS 获得全面保护，而且客户端使用服务的 SSL 证书对服务进行身份验证。 客户端身份验证通过元素的 `ClientCredentials` 属性进行控制 [\<transport>](transport-of-ws2007httpbinding.md) 。|  
|`Message`|使用 SOAP 消息安全提供安全性。 默认情况下，将对 SOAP 正文进行加密和签名。 此模式提供了各种各样的功能，例如服务凭据在带外客户端是否可用、要使用的算法套件以及通过 <xref:System.ServiceModel.Security.SecurityMessageProperty> 要应用于消息正文的保护级别。 每个会话将执行一次客户端身份验证，身份验证的结果在会话过程中将进行缓存。|  
|`TransportWithMessageCredential`|在此模式下，HTTPS 将提供完整性、保密性和服务器身份验证，SOAP 消息安全将提供客户端身份验证。 默认情况下，每个会话将执行一次客户端身份验证，身份验证的结果在会话过程中将进行缓存。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<transport>](transport-of-ws2007httpbinding.md)|定义传输安全设置。 此元素与 <xref:System.ServiceModel.Configuration.HttpTransportSecurityElement> 类型相对应。 仅在将模式设置为“Transport”时才应用这些设置。|  
|[\<message>](message-of-ws2007httpbinding.md)|定义消息的安全设置。 此元素与 <xref:System.ServiceModel.Configuration.MessageSecurityOverHttpElement> 类型相对应。 将模式设置为“Transport”时，不应用这些设置。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<ws2007HttpBinding>](ws2007httpbinding.md)|HTTP 传输应用程序的安全绑定。|  
  
## <a name="remarks"></a>备注  

 此元素专用于与实现 WS-* 规范的服务进行互操作。 此绑定的传输安全为 HTTP 上的安全套接字层 (SSL)，即 HTTPS。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.WSHttpSecurity>
- <xref:System.ServiceModel.WSHttpBinding.Security%2A>
- <xref:System.ServiceModel.Configuration.WSHttpBindingElement.Security%2A>
- <xref:System.ServiceModel.Configuration.WSHttpSecurityElement>
- <xref:System.ServiceModel.BasicHttpSecurity>
- [保护服务和客户端的安全](../../../wcf/feature-details/securing-services-and-clients.md)
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
