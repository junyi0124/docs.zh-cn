---
title: <transport> 的 <netTcpBinding>
ms.date: 03/30/2017
ms.assetid: 49462e0a-66e1-463f-b3e1-c83a441673c6
ms.openlocfilehash: 8f752373c51992c51b747f5f4dc4a63910a387c6
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91162187"
---
# <a name="transport-of-nettcpbinding"></a>\<transport> 的 \<netTcpBinding>

为使用配置的终结点定义消息级安全性要求的类型 [\<netTcpBinding>](nettcpbinding.md) 。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<netTcpBinding>**](nettcpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-nettcpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<transport>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<netTcpBinding>
  <binding>
    <security mode="None|Transport|Message|TransportWithMessageCredential">
      <transport clientCredentialType="None|Windows|Certificate"
                 protectionLevel="None|Sign|EncryptAndSign"
                 sslProtocols="Tls|Tls11|Tls12">
        <extendedProtectionPolicy policyEnforcement="Never|WhenSupported|Always"
                                  protectionScenario="TransportSelected|TrustedProxy">
          <customServiceNames>
          </customServiceNames>
        </extendedProtectionPolicy>
      </transport>
    </security>
  </binding>
</netTcpBinding>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 以下几节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|clientCredentialType|可选。 指定要在使用传输安全性执行客户端身份验证时使用的凭据类型。<br /><br /> -默认值为 `Windows` 。<br />-此属性的类型为 <xref:System.ServiceModel.TcpClientCredentialType> 。|  
|protectionLevel|可选。 定义 TCP 传输级别的安全性。 消息签名降低了在消息传输过程中第三方对消息进行篡改的风险。 加密为传输过程提供了数据级保密功能。<br /><br /> 默认值是 `EncryptAndSign`。|  
|sslProtocols|指定支持哪些 SslProtocols 的 SslProtocols 枚举标志值。 默认值为 Tls&#124;Tls11&#124;Tls12。|  
|policyEnforcement|此枚举指定应何时强制实施 <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy>。<br /><br /> 1. 从不–不会强制实施策略 (禁用扩展保护) 。<br />WhenSupported-仅当客户端支持扩展保护时才强制实施策略。<br />3. always –始终强制实施策略。 不支持扩展保护的客户端将无法进行身份验证。|  
  
## <a name="clientcredentialtype-attribute"></a>clientCredentialType 属性  
  
|值|描述|  
|-----------|-----------------|  
|无|客户端为匿名客户端。 这需要服务证书。|  
|Windows|指定使用 SP 协商（Kerberos 协商）进行客户端 Windows 身份验证。|  
|证书|使用证书进行对客户端进行身份验证。 这使用 SSL 协商并需要服务证书。|  
  
## <a name="protectionlevel-attribute"></a>protectionLevel 属性  
  
|值|描述|  
|-----------|-----------------|  
|无|无保护。|  
|签名|对消息进行签名。|  
|EncryptAndSign|-对消息进行加密和签名。|  
  
### <a name="child-elements"></a>子元素  

 无  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<security>](security-of-nettcpbinding.md)|指定的安全功能 [\<netTcpBinding>](nettcpbinding.md) 。|  
  
## <a name="remarks"></a>备注  

 使用传输安全性以获得 SOAP 消息的完整性和保密性以及相互身份验证。 如果在绑定上选择此安全模式，则使用安全传输配置信道栈，并且使用传输安全性（如 Windows (Negotiate) 或 SSLL）保护 SOAP 消息安全通过 TCP 传递。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.TcpTransportSecurity>
- <xref:System.ServiceModel.Configuration.NetTcpSecurityElement.Transport%2A>
- <xref:System.ServiceModel.NetTcpSecurity.Transport%2A>
- <xref:System.ServiceModel.Configuration.NetTcpSecurityElement>
- [保护服务和客户端的安全](../../../wcf/feature-details/securing-services-and-clients.md)
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
