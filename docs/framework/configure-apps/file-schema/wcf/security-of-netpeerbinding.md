---
title: <security> 的 <netPeerBinding>
ms.date: 03/30/2017
ms.assetid: 1ef40d8c-f903-4426-9b08-da81462766d8
ms.openlocfilehash: 543c57d6b2dba1ff5934b49e0e219cf2e5cad153
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91170020"
---
# <a name="security-of-netpeerbinding"></a>\<security> 的 \<netPeerBinding>

定义的安全设置 [\<netPeerTcpBinding>](netpeertcpbinding.md) ，包括使用的身份验证类型和用于消息传输的安全性。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<netPeerTcpBinding>**](netpeertcpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<security>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<netPeerBinding>
  <binding>
    <security mode="Message/None/Transport//TransportWithMessageCredential">
      <transport credentialType="Certificate/Password" />
    </security>
  </binding>
</netPeerBinding>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 以下几节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|mode|可选。 指定采用此绑定配置的对等端所使用的安全类型。 默认值是 `Message`。 此属性的类型为 <xref:System.ServiceModel.SecurityMode>。|  
  
## <a name="mode-attribute"></a>mode 属性  
  
|值|描述|  
|-----------|-----------------|  
|消息|SOAP 安全提供身份验证、完整性和保密性。|  
|无|禁用安全性。|  
|传输|使用 HTTPS 提供安全性。|  
|TransportWithMessageCredential|HTTPS 提供身份验证和保密性。 SOAP 消息提供丰富的凭据类型。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<transport>](transport-of-netpeertcpbinding.md)|定义采用此绑定配置的对等端所发送的安全消息的传输类型。 此元素的类型为 <xref:System.ServiceModel.Configuration.PeerTransportSecurityElement>。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<binding>](bindings.md)|定义的所有绑定功能 [\<netPeerTcpBinding>](netpeertcpbinding.md) 。|  
  
## <a name="remarks"></a>备注  

 安全性可以是特定于消息的，也可以是特定于传输的。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.PeerSecurityElement>
- <xref:System.ServiceModel.NetPeerTcpBinding.Security%2A>
- <xref:System.ServiceModel.Configuration.NetPeerTcpBindingElement.Security%2A>
- <xref:System.ServiceModel.PeerSecuritySettings>
- [保护服务和客户端的安全](../../../wcf/feature-details/securing-services-and-clients.md)
- [选择凭据类型](../../../wcf/feature-details/selecting-a-credential-type.md)
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
