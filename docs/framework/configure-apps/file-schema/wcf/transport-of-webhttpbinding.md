---
title: <transport> 的 <webHttpBinding>
ms.date: 03/30/2017
ms.assetid: f150fb19-7de1-44af-81f4-86cad881cd05
ms.openlocfilehash: b9efc732832a8862373b14f657796a59fb52c1a1
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91162109"
---
# <a name="transport-of-webhttpbinding"></a>\<transport> 的 \<webHttpBinding>

定义配置为接收 HTTP 请求的服务终结点的传输级安全设置。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<webHttpBinding>**](webhttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-webhttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<transport>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<webHttpBinding>
  <binding>
    <security mode="None|Transport|Message|TransportWithMessageCredential|TransportCredentialOnly">
      <transport clientCredentialType="None|Basic|Digest|Ntlm|Windows"
                 proxyCredentialType="None|Basic|Digest|Ntlm|Windows"
                 realm="string">
        <extendedProtectionPolicy policyEnforcement="Never|WhenSupported|Always"
                                  protectionScenario="TransportSelected|TrustedProxy">
          <customServiceNames>
          </customServiceNames>
        </extendedProtectionPolicy>
      </transport>
    </security>
  </binding>
</webHttpBinding>
```  
  
## <a name="type"></a>类型  

 <xref:System.ServiceModel.HttpTransportSecurity>  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`clientCredentialType`|指定用于向服务证明客户端身份的凭据。 此属性的类型为 <xref:System.ServiceModel.HttpClientCredentialType>。|  
|`proxyCredentialType`|指定用于向域代理证明客户端身份的凭据。 此属性的类型为 <xref:System.ServiceModel.HttpProxyCredentialType>。|  
|`realm`|一个字符串，指定摘要式或基本身份验证的身份验证领域。 默认值为一个空字符串。<br /><br /> 身份验证领域至少指定执行身份验证的主机的名称。 它还可以指定具有访问权限的用户的集合。 用户可以查询身份验证领域，以确定多个可能的用户名和密码中哪一个可以使用。|  
|`policyEnforcement`|此枚举指定应何时强制实施 <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy>。<br /><br /> 1. 从不–不会强制实施策略 (禁用扩展保护) 。<br />WhenSupported-仅当客户端支持扩展保护时才强制实施策略。<br />3. always –始终强制实施策略。 不支持扩展保护的客户端将无法进行身份验证。|  
  
## <a name="clientcredentialtype-attribute"></a>clientCredentialType 属性  
  
|值|描述|  
|-----------|-----------------|  
|`None`|禁用安全性。|  
|`Basic`|使用基本身份验证。|  
|`Certificate`|使用 X.509 证书对客户端进行身份验证。|  
|`Digest`|使用摘要式身份验证。|  
|`Ntlm`|对 Windows 域使用 NTLM 身份验证作为回退。|  
|`Windows`|使用集成 Windows 身份验证。|  
  
## <a name="proxycredentialtype-attribute"></a>proxyCredentialType 属性  
  
|值|描述|  
|-----------|-----------------|  
|`None`|禁用安全性。|  
|`Basic`|使用基本身份验证。|  
|`Digest`|使用摘要式身份验证。|  
|`Ntlm`|对 Windows 域使用 NTLM 作为回退。|  
|`Windows`|使用集成 Windows 身份验证。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<security>](security-of-webhttpbinding.md)|表示元素的安全功能 [\<wsHttpBinding>](wshttpbinding.md) 。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.HttpTransportSecurity>
- <xref:System.ServiceModel.Configuration.WebHttpSecurityElement.Transport%2A>
- <xref:System.ServiceModel.WebHttpSecurity.Transport%2A>
- <xref:System.ServiceModel.Configuration.HttpTransportSecurityElement>
- [保护服务和客户端的安全](../../../wcf/feature-details/securing-services-and-clients.md)
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
- [WCF Web HTTP 编程模型](../../../wcf/feature-details/wcf-web-http-programming-model.md)
