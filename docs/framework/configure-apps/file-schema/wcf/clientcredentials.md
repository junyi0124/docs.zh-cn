---
title: <clientCredentials>
ms.date: 03/30/2017
ms.assetid: 1e6eef0d-a34e-4d74-b0f7-f65d2181858d
ms.openlocfilehash: 6094006df24ee824c419a783ab29d7604757577c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91201455"
---
# \<clientCredentials>

指定用于向服务验证客户端身份的凭据。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpointBehaviors>**](endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<clientCredentials>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<clientCredentials type="String"
                   supportInteractive="Boolean" >
  <clientCertificate>
  </clientCertificate>
  <digest>
  </digest>
  <isuedToken>
  </isuedToken>
  <peer>
  </peer>
  <serviceCertificate>
  </serviceCertificate>
  <windowsAuthentication>
  </windowsAuthentication>
</clientCredentials>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`supportInteractive`|一个布尔值，指定在运行时选择客户端凭据的过程中是否可以涉及交互式用户。 默认值是 `true`。|  
|`type`|一个指定此配置元素的类型的字符串。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<clientCertificate>](clientcertificate-of-clientcredentials-element.md)|指定用于向服务验证客户端身份的证书。 此元素的类型为 <xref:System.ServiceModel.Configuration.X509InitiatorCertificateClientElement>。|  
|[\<httpDigest>](httpdigest-element.md)|指定用于向服务验证客户端身份的摘要。 此元素的类型为 <xref:System.ServiceModel.Configuration.HttpDigestClientElement>。|  
|[\<issuedToken>](issuedtoken.md)|指定用于向安全令牌服务 (STS) 验证客户端身份的自定义令牌类型。 此元素的类型为 <xref:System.ServiceModel.Configuration.IssuedTokenClientElement>。|  
|[\<peer>](peer-of-clientcredentials-element.md)|指定一个当前对等凭据。 此元素的类型为 <xref:System.ServiceModel.Configuration.PeerCredentialElement>。|  
|[\<serviceCertificate>](servicecertificate-of-clientcredentials-element.md)|指定用于向客户端验证服务身份的证书，并提供一个用于设置证书选项的结构。 必须从服务以带外方式向客户端提供此证书。 此元素的类型为 <xref:System.ServiceModel.Configuration.X509RecipientCertificateClientElement>。|  
|[\<windows>](windows-of-clientcredentials-element.md)|指定 Windows 凭据。 默认值是当前线程的凭据。 此元素的类型为 <xref:System.ServiceModel.Configuration.WindowsClientElement>。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<behavior>](behavior-of-endpointbehaviors.md)|指定终结点行为。|  
  
## <a name="remarks"></a>备注  

 在要求相互进行身份验证的情况下，需要使用客户端凭据使客户端通过服务的身份验证。 当客户端必须使用服务的证书来保护发送到服务的消息时，还可以使用该配置节来指定服务证书。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.ClientCredentialsElement>
- <xref:System.ServiceModel.Description.ClientCredentials>
- [安全行为](../../../wcf/feature-details/security-behaviors-in-wcf.md)
- [保证客户端的安全](../../../wcf/securing-clients.md)
