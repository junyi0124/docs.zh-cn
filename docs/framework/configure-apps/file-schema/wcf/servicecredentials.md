---
title: <serviceCredentials>
ms.date: 03/30/2017
ms.assetid: 96db336c-4f7a-4193-81a5-910b8ffd804f
ms.openlocfilehash: 63368355027856118546bc70183b41864eddb0e6
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91172874"
---
# \<serviceCredentials>

指定要用于对服务进行身份验证的凭据以及与客户端凭据验证相关的设置。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<serviceCredentials>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<serviceCredentials type="String">
  <clientCertificate>
  </clientCertificate>
  <issuedTokenAuthentication>
  </issuedTokenAuthentication>
  <peer>
  </peer>
  <secureConversationAuthentication>
  </secureConversationAuthentication>
  <serviceCertificate>
  </serviceCertificate>
  <userNameAuthentication>
  </userNameAuthentication>
  <windowsAuthentication>
  </windowsAuthentication>
</serviceCredentials>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`type`|一个指定此配置元素的类型的字符串。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<clientCertificate>](clientcertificate-of-servicecredentials.md)|指定证书，当可在带外使用客户端证书时，将使用该证书。 此元素还指定客户端证书验证设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.X509InitiatorCertificateServiceElement>。|  
|[\<issuedTokenAuthentication>](issuedtokenauthentication-of-servicecredentials.md)|指定此服务的当前颁发令牌。 此元素的类型为 <xref:System.ServiceModel.Configuration.IssuedTokenServiceElement>。|  
|[\<peer>](peer-of-servicecredentials.md)|指定对等节点的当前凭据。 此元素的类型为 <xref:System.ServiceModel.Configuration.PeerCredentialElement>。|  
|[\<secureConversationAuthentication>](secureconversationauthentication-of-servicecredential.md)|指定安全对话的当前凭据。 此元素的类型为 <xref:System.ServiceModel.Configuration.SecureConversationServiceElement>。|  
|[\<serviceCertificate>](servicecertificate-of-servicecredentials.md)|指定服务用于标识自身的证书。 此元素的类型为 <xref:System.ServiceModel.Configuration.X509RecipientCertificateServiceElement>。|  
|[\<userNameAuthentication>](usernameauthentication.md)|指定用户名密码验证的设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.UserNameServiceElement>。|  
|[\<windowsAuthentication>](windowsauthentication-of-servicecredentials.md)|指定 Windows 凭据验证的设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.WindowsServiceElement>。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<behavior>](behavior-of-endpointbehaviors.md)|指定行为元素。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.ServiceCredentialsElement>
- <xref:System.ServiceModel.Description.ServiceCredentials>
- [安全行为](../../../wcf/feature-details/security-behaviors-in-wcf.md)
