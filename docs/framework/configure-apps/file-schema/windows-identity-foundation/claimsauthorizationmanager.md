---
title: <claimsAuthorizationManager>
ms.date: 03/30/2017
ms.assetid: 9354eee3-f692-4ad6-8427-3169686b8bcc
author: BrucePerlerMS
ms.openlocfilehash: 0718f789ff4d99fb4e2651a9a704da4248cd5f49
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91158430"
---
# \<claimsAuthorizationManager>

为传入声明注册声明授权管理器。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<claimsAuthorizationManager>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <claimsAuthorizationManager type = xs:string>  
      <optionalConfigurationElements />  
    </claimsAuthorizationManager>  
  </identityConfiguration>  
</system.identityModel>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|说明|  
|---------------|-----------------|  
|type|派生自类的自定义类型 <xref:System.Security.Claims.ClaimsAuthorizationManager> 。 有关如何指定属性的详细信息 `type` ，请参阅 [自定义类型引用](../windows-workflow-foundation/index.md)。|  
  
### <a name="child-elements"></a>子元素  

 如果没有 `type` 属性，或者如果 `type` 特性引用 <xref:System.Security.Claims.ClaimsAuthenticationManager> 类，则 `<claimsAuthorizationManager>` 元素不采用子元素; 但是，从派生的类 <xref:System.Security.Claims.ClaimsAuthorizationManager> 可以定义子配置元素。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<identityConfiguration>](identityconfiguration.md)|指定服务级别标识设置。|  
  
## <a name="remarks"></a>备注  

 通过类提供的默认行为 <xref:System.Security.Claims.ClaimsAuthorizationManager> 始终授权传入声明。 如果未 `type` 指定任何特性或 `type` 特性指定了 <xref:System.Security.Claims.ClaimsAuthorizationManager> 类，则元素不 `<claimsAuthorizationManager>` 采用子元素。 可以指定 `type` 用于注册派生自类的类型的属性， <xref:System.Security.Claims.ClaimsAuthorizationManager> 以实现自定义行为。 派生类可以 `<claimsAuthorizationManager>` 通过重写 <xref:System.Security.Claims.ClaimsAuthorizationManager.LoadCustomConfiguration%2A> 方法来处理这些元素，从而支持通过元素的子元素进行的配置。 为子元素定义的架构位于类的设计器中。  
  
> [!IMPORTANT]
> 当使用 <xref:System.IdentityModel.Services.ClaimsPrincipalPermission> 或 <xref:System.IdentityModel.Services.ClaimsPrincipalPermissionAttribute> 类在代码中提供基于声明的访问控制时，元素引用的标识配置将 `<federationConfiguration>` 配置用于做出授权决定的声明授权管理器和策略。 即使在非被动 Web 应用场景的情况下（例如 Windows Communication Foundation (WCF) 应用程序或不是基于 Web 的应用程序），也是如此。 如果应用程序不是被动 Web 应用程序，则 `<claimsAuthorizationManager>` 元素 (及其子策略元素，如果只应用了引用的标识配置) 。 忽略所有其他设置。 有关详细信息，请参阅 [\<federationConfiguration>](federationconfiguration.md) 元素。  
  
 此元素设置 <xref:System.IdentityModel.Configuration.IdentityConfiguration.ClaimsAuthorizationManager%2A?displayProperty=nameWithType> 属性。  
  
## <a name="example"></a>示例  

 下面的 XML 显示了一个声明授权管理器的配置，该管理器实现由资源操作对组成的策略，其中每个规则都指定请求者为对资源执行操作而必须拥有的声明的布尔组合。 示例中可找到实现声明授权管理器的代码，该代码可使用此策略 `ClaimsBasedAuthorization` 。  
  
```xml  
<system.identityModel>  
    <identityConfiguration>  
      <claimsAuthorizationManager type="ClaimsAuthorizationLibrary.MyClaimsAuthorizationManager, ClaimsAuthorizationLibrary">  
        <policy resource="http://localhost:28491/Developers.aspx" action="GET">  
          <or>  
            <claim claimType="http://schemas.microsoft.com/ws/2008/06/identity/claims/role" claimValue="developer" />  
            <claim claimType="http://schemas.xmlsoap.org/claims/Group" claimValue="Administrator" />  
          </or>  
        </policy>  
        <policy resource="http://localhost:28491/Administrators.aspx" action="GET">  
          <and>  
            <claim claimType="http://schemas.xmlsoap.org/claims/Group" claimValue="Administrator" />  
            <claim claimType="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/country" claimValue="USA" />  
          </and>  
        </policy>  
        <policy resource="http://localhost:28491/Default.aspx" action="GET">  
        </policy>  
        <policy resource="http://localhost:28491/" action="GET">  
        </policy>  
        <policy resource="http://localhost:28491/Claims.aspx" action="GET">  
        </policy>  
      </claimsAuthorizationManager>  
    <identityConfiguration>  
<system.identityModel>  
```
