---
title: <userNameAuthentication>
ms.date: 03/30/2017
ms.assetid: 24d8b398-770f-418f-ba23-c4325419cfa6
ms.openlocfilehash: 30fd78d6c56e8b22e0e744a38f18ac076dc70162
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91178029"
---
# \<userNameAuthentication>

指定基于用户名和密码的服务凭据。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceCredentials>**](servicecredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<userNameAuthentication>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<userNameAuthentication cacheLogonTokenLifetime="TimeSpan"
                        cacheLogonTokens="Boolean"
                        customUserNamePasswordValidatorType="String"
                        includeWindowsGroups="Boolean"
                        maxCacheLogonTokens="Integer"
                        membershipProviderName="String"
                        userNamePasswordValidationMode="Windows/MembershipProvider/Custom" />
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`cacheLogonTokenLifetime`|一个 <xref:System.TimeSpan>，指定缓存令牌的最大时间长度。 默认值为 00:15:00。|  
|`cacheLogonTokens`|一个布尔值，指定是否缓存登录令牌。 默认为 `false`。|  
|`customUserNamePasswordValidatorType`|一个字符串，指定要使用的自定义用户名密码验证程序的类型。 默认值为空字符串。|  
|`includeWindowsGroups`|一个布尔值，指定 Windows 组是否包含在安全上下文中。 默认为 `true`。<br /><br /> 将此属性设置为 `true` 会影响性能，因为这会导致完全组扩展。 如果不需要建立用户所属组的列表，请将此属性设置为 `false`。|  
|`maxCacheLogonTokens`|一个整数，指定要缓存的最大登录令牌数。 此值应大于零。 默认值为 128。|  
|`membershipProviderName`|如果将绑定的 `clientCredentialType` 属性设置为 `username`，则用户名将映射到 Windows 帐户。 可以使用此属性重写此行为，此属性是一个包含 <xref:System.Web.Security.MembershipProvider> 值的名称的字符串，该值提供相关的密码验证机制。|  
|`userNamePasswordValidationMode`|指定对用户名密码进行验证的方式。 有效值是：<br /><br /> -Windows<br />-MembershipProvider<br />-Custom<br /><br /> 默认值为 Windows。 此属性的类型为 <xref:System.ServiceModel.Security.UserNamePasswordValidationMode>。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<serviceCredentials>](servicecredentials.md)|指定要用于对服务进行身份验证的凭据以及与客户端凭据验证相关的设置。|  
  
## <a name="remarks"></a>备注  

 如果没有为基于用户名/密码的身份验证配置服务所用的任何绑定，则忽略此元素的一些属性， 其中包括 `customUserNamePasswordValidatorType`、`includeWindowsGroups`、`membershipProviderName` 和 `userNamePasswordValidationMode`。  
  
 如果没有配置服务所用的绑定，以使用 Windows 用户名/密码身份验证，则忽略与登录令牌的缓存相关的设置。 这些设置包括 `cacheLogonTokenLifetime`、`cacheLogonTokens` 和 `maxCacheLogonTokens`。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.UserNameServiceElement>
- <xref:System.ServiceModel.Description.ServiceCredentials.UserNameAuthentication%2A>
- <xref:System.ServiceModel.Security.UserNamePasswordServiceCredential>
- <xref:System.ServiceModel.Configuration.ServiceCredentialsElement.UserNameAuthentication%2A>
