---
title: <federationConfiguration>
ms.date: 03/30/2017
ms.assetid: 8b14054c-6d07-46ab-ab58-03f14beac0f2
author: BrucePerlerMS
ms.openlocfilehash: 39e96a161a2e75d5f00b73f6b08b1e4a0c109aee
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91201351"
---
# \<federationConfiguration>

<xref:System.IdentityModel.Services.WSFederationAuthenticationModule> <xref:System.IdentityModel.Services.SessionAuthenticationModule> 通过 WS 联合身份验证协议使用联合身份验证时，配置 (WSFAM) 和 (SAM) 。 <xref:System.Security.Claims.ClaimsAuthorizationManager>当使用 <xref:System.IdentityModel.Services.ClaimsPrincipalPermission> 或 <xref:System.IdentityModel.Services.ClaimsPrincipalPermissionAttribute> 类提供基于声明的访问控制时，配置。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel.services>**](system-identitymodel-services.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<federationConfiguration>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.identityModel.services>  
  <federationConfiguration name=xs:string identityConfigurationName=xs:string>  
  </federationConfiguration>  
</system.identityModel.services>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|说明|  
|---------------|-----------------|  
|name|此联合配置元素的名称。 此属性主要为未来的协议提供扩展点。 可选。|  
|identityConfigurationName|要使用的元素中指定的标识配置节的名称 [\<identityConfiguration>](identityconfiguration.md) 。 如果未指定此属性，则使用 "默认标识配置" 部分。 可选。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<cookieHandler>](cookiehandler.md)|配置 SAM 使用的 cookie 处理程序。 可选。|  
|[\<serviceCertificate>](servicecertificate.md)|配置用于加密和解密令牌的证书。 可选。|  
|[\<wsFederation>](wsfederation.md)|配置 WS 联合身份验证模块 (WSFAM) 。 可选。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<system.identityModel.services>](system-identitymodel-services.md)|使用 WS 联合身份验证协议进行身份验证的配置节。|  
  
## <a name="remarks"></a>备注  

 \<federationConfiguration>元素在两个不同的方案中提供设置：  
  
- 在被动 Web 应用程序中使用 WS 联合身份验证时，元素包含配置 <xref:System.IdentityModel.Services.WSFederationAuthenticationModule> (WSFAM) 和 <xref:System.IdentityModel.Services.SessionAuthenticationModule> (SAM) 的设置。 它还引用了用于配置安全令牌处理程序和证书的标识配置，以及声明授权管理器和声明身份验证管理器等组件。  
  
- 当使用 <xref:System.IdentityModel.Services.ClaimsPrincipalPermission> 或 <xref:System.IdentityModel.Services.ClaimsPrincipalPermissionAttribute> 类在代码中提供基于声明的访问控制时，元素会引用标识配置，该配置将用于做出授权决定的声明授权管理器和策略。 即使在非被动 Web 应用场景的情况下，也是如此。例如，Windows Communication Foundation (WCF) 应用程序或不是基于 Web 的应用程序。 如果应用程序不是被动 Web 应用程序，则 [\<claimsAuthorizationManager>](claimsauthorizationmanager.md) 元素 (及其子策略元素，如果仅应用元素所引用的标识配置) ，则为 `<federationConfiguration>` 。 将忽略所有其他成员。  
  
 无论方案如何，运行时都将加载默认的联合身份验证配置。 此行为定义如下：  
  
1. 如果没有任何 `<federationConfiguration>` 元素，则运行时将创建联合配置并使用默认值填充该配置。 此默认的联合身份验证配置将引用默认的标识配置。  
  
2. 如果存在单个 `<federationConfiguration>` 元素，则它是默认的联合配置，而不考虑它是命名还是未命名。 如果 `identityConfiguration` 指定其属性，则引用命名标识配置; 否则，将引用默认标识配置。  
  
3. 如果存在未命名的 `<federationConfiguration>` 元素，则它是默认的联合身份验证配置。 如果 `identityConfiguration` 指定其属性，则引用命名标识配置; 否则，将引用默认标识配置。  
  
4. 如果存在多个命名 `<federationConfiguration>` 元素，且未命名 `<federationConfiguration>` 元素不存在，则会引发异常。  
  
 通常，只定义了一个 `<federationConfiguration>` 部分。 此部分是默认的联合身份验证配置。 您可以指定多个唯一命名的 `<federationConfiguration>` 元素; 但是，在这种情况下，如果要加载非命名的联合配置，则必须为提供处理程序。 <xref:System.IdentityModel.Services.FederatedAuthentication.FederationConfigurationCreated> 事件，并将 <xref:System.IdentityModel.Services.Configuration.FederationConfigurationCreatedEventArgs.FederationConfiguration%2A?displayProperty=nameWithType> 处理程序中的属性设置为 <xref:System.IdentityModel.Services.Configuration.FederationConfiguration> 使用 `<federationConfiguration>` 配置文件中相应元素的值进行初始化的对象。  
  
 `<federationConfiguration>`元素由 <xref:System.IdentityModel.Services.Configuration.FederationConfigurationElement> 类表示。 配置对象本身由 <xref:System.IdentityModel.Services.Configuration.FederationConfiguration> 类表示。 单个 <xref:System.IdentityModel.Services.Configuration.FederationConfiguration> 实例在属性上设置 <xref:System.IdentityModel.Services.FederatedAuthentication.FederationConfiguration%2A?displayProperty=nameWithType> ，并为应用程序提供联合配置。  
  
## <a name="example"></a>示例  

 下面的 XML 演示一个 `<federationConfiguration>` 元素，它指定 WSFAM 的设置，并指定默认 cookie 处理程序 (类的实例 <xref:System.IdentityModel.Services.ChunkedCookieHandler>) 由 SAM 使用。  
  
> [!WARNING]
> 在此示例中，cookie 处理程序和 WSFAM 都不需要使用 HTTPS。 这是因为 `requireHttps` 元素上的特性 `<wsFederation>` 和上的 `requireSsl` 特性 `<cookieHandlerElement>` 是 `false` 。 对于大多数生产环境，不建议使用这些设置，因为它们可能会带来安全风险。  
  
```xml  
<system.identityModel.services>  
  <federationConfiguration>  
    <wsFederation passiveRedirectEnabled="true"
      issuer="http://localhost:15839/wsFederationSTS/Issue"
      realm="http://localhost:50969/" reply="http://localhost:50969/"
      requireHttps="false"
      signOutReply="http://localhost:50969/SignedOutPage.html"
      signOutQueryString="Param1=value2&Param2=value2"
      persistentCookiesOnPassiveRedirects="true" />  
    <cookieHandler requireSsl="false" />  
  </federationConfiguration>  
</system.identityModel.services>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.IdentityModel.Services.WSFederationAuthenticationModule>
- <xref:System.IdentityModel.Services.SessionAuthenticationModule>
- <xref:System.IdentityModel.Services.FederatedAuthentication.FederationConfiguration%2A?displayProperty=nameWithType>
- [\<identityConfiguration>](identityconfiguration.md)
