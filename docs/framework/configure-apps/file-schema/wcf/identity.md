---
title: <identity>
ms.date: 03/30/2017
ms.assetid: c1d2ae56-e231-4a07-9c3f-9f13381dc0d8
ms.openlocfilehash: bb9468b6005361a2a480f7c0ebfb2cbb9e9199c2
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91157377"
---
# \<identity>

标识元素允许客户端开发人员在设计时指定服务的期望标识。 在客户端与服务之间的握手过程中，Windows Communication Foundation (WCF) 基础结构将确保预期服务的标识与此元素的值相匹配，因而可以进行身份验证。 有关详细信息，请参阅 [服务标识和身份验证](../../../wcf/feature-details/service-identity-and-authentication.md)。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<client>**](client.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpoint>**](endpoint-of-client.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<identity>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<identity>
  <certificate encodedValue="String" />
  <certificateReference findValue="String"
                        isChainIncluded="Boolean"
                        storeName="AddressBook/AuthRoot/CertificateAuthority/Disallowed/My/Root/TrustedPeople/TrustedPublisher"
                        storeLocation="LocalMachine/CurrentUser"
                        X509FindType="Enumeration" />
  <dns value="String" />
  <rsa value="String" />
  <servicePrincipalName value="String" />
  <userPrincipalName value="String" />
</identity>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  

 无。  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|证书 (certificate)|指定 X.509 证书的设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.CertificateElement>。 它包含一个 `encodedValue` 属性，该属性是一个字符串，用于指定此证书编码的值。|  
|certificateReference|指定 X.509 证书验证的设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.CertificateReferenceElement>。|  
|dns|指定用于对服务进行身份验证的 X.509 证书的 DNS。 此元素包含一个字符串属性 `value`，并包含实际的标识。|  
|rsa|指定用于向客户端验证服务身份的 X.509 证书的 RSA 字段的值。 此元素包含一个字符串属性 `value`，并包含实际的标识。|  
|servicePrincipalName|指定服务器主体名称 (SPN) 标识，它是客户端用来唯一标识一个服务实例的主体名称。 此元素包含一个 `value` 属性，该属性是一个字符串，其中包含实际的主体名称。 此元素的类型为 <xref:System.ServiceModel.Configuration.ServicePrincipalNameElement>。|  
|userPrincipalName|指定用户主体名称 (UPN) 标识，它是网络上的用户登录名类型。 用户主体名称包含 Active Directory 中使用的用户对象名称，后跟符号 (\@) 然后通常是域名系统父域。 例如，Fabrikam.com 域树中的 Jeff 可能具有用户主体名称 [jeff@fabrikam.com](mailto:jeffsmith@fabrikam.com) 。  此元素包含一个 `value` 属性，该属性是一个字符串，其中包含实际的主体名称。 此元素的类型为 <xref:System.ServiceModel.Configuration.UserPrincipalNameElement>。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<custom>](custom.md)|指定 netPeerTcpBinding 的自定义对等解析程序。|  
|[\<endpoint>](endpoint-element.md)|配置服务终结点。|  
|[\<client> 的 \<endpoint>](endpoint-of-client.md)|配置通道终结点。|  
|[\<issuer>](issuer.md)|指定联合服务的安全令牌服务 (STS)。|  
|[\<issuerMetadata>](issuermetadata.md)|指定联合服务的安全令牌服务 (STS) 的元数据终结点。|  
|[\<issuedTokenParameters>](issuedtokenparameters.md)|定义自定义绑定中的已颁发令牌的参数。|  
|[\<localIssuer>](localissuer.md)|指定本地安全令牌服务 (STS)。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.IdentityElement>
- <xref:System.ServiceModel.EndpointAddress>
- <xref:System.ServiceModel.EndpointAddress.Identity%2A>
- [服务标识和身份验证](../../../wcf/feature-details/service-identity-and-authentication.md)
- [终结点：地址、绑定和协定](../../../wcf/feature-details/endpoints-addresses-bindings-and-contracts.md)
