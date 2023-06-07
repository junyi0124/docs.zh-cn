---
title: <userPrincipalName>
ms.date: 03/30/2017
ms.assetid: 68032f69-149e-4613-bae4-18314d4fd294
ms.openlocfilehash: 353d3e2d88e3515e54deadab85c37ce3be26ef29
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203860"
---
# \<userPrincipalName>

指定要由客户端进行身份验证的服务的用户主体名称 (UPN)。  
  
有关设置 UPN 的详细信息，请参阅 [服务标识和身份验证](../../../wcf/feature-details/service-identity-and-authentication.md)。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<client>**](client.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpoint>**](endpoint-of-client.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<identity>**](identity.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<userPrincipalName>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<userPrincipalName value="String" />
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 以下几节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|说明|  
|---------------|-----------------|  
|值|一个用户帐户名（有时称为“用户登录名”）和一个域名（标识用户帐户所在的域）。 这是登录到 Windows 域的标准用法。 格式为： someone@example.com () 电子邮件地址。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<identity>](identity.md)|指定要由客户端进行身份验证的服务的标识。|  
  
## <a name="remarks"></a>备注  

 使用此标识连接到终结点的安全 Windows Communication Foundation (WCF) 客户端使用 UPN 在对终结点进行 SSPI 身份验证时使用 UPN。  
  
## <a name="example"></a>示例  

 下面的配置代码指定要由客户端进行身份验证的服务的 UPN。  
  
```xml  
<identity>
  <userPrincipalName value="someone@cohowinery.com" />
</identity>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.IdentityElement>
- <xref:System.ServiceModel.EndpointAddress>
- <xref:System.ServiceModel.EndpointAddress.Identity%2A>
- <xref:System.ServiceModel.UpnEndpointIdentity>
- [服务标识和身份验证](../../../wcf/feature-details/service-identity-and-authentication.md)
- [\<identity>](identity.md)
