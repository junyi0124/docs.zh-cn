---
title: <claimsAuthenticationManager>
ms.date: 03/30/2017
ms.assetid: 6d30a450-6d13-4671-81a8-77e0204500c5
author: BrucePerlerMS
ms.openlocfilehash: 9e099b8de486631702548b8a035a46a7e0f4eb30
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91158469"
---
# \<claimsAuthenticationManager>

为传入声明注册声明身份验证管理器。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<claimsAuthenticationManager>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <claimsAuthenticationManager type=xs:string>  
      <optionalConfigurationElements />  
    </claimsAuthenticationManager>  
  </identityConfiguration>  
</system.identityModel>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|说明|  
|---------------|-----------------|  
|type|指定从类派生的自定义类型 <xref:System.Security.Claims.ClaimsAuthenticationManager> 。 有关如何指定属性的详细信息 `type` ，请参阅 [自定义类型引用]。|  
  
### <a name="child-elements"></a>子元素  

 如果没有 `type` 属性，或者如果 `type` 特性引用 <xref:System.Security.Claims.ClaimsAuthenticationManager> 类，则 `<claimsAuthenticationManager>` 元素不采用子元素; 但是，从派生的类 <xref:System.Security.Claims.ClaimsAuthenticationManager> 可以定义子配置元素。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<identityConfiguration>](identityconfiguration.md)|指定服务级别标识设置。|  
  
## <a name="remarks"></a>备注  

 通过类提供的默认行为会 <xref:System.Security.Claims.ClaimsAuthenticationManager> 回显传入声明。 如果未 `type` 指定任何特性或 `type` 特性指定了 <xref:System.Security.Claims.ClaimsAuthenticationManager> 类，则元素不 `<claimsAuthenticationManager>` 采用子元素。 可以指定 `type` 用于注册派生自类的类型的属性， <xref:System.Security.Claims.ClaimsAuthenticationManager> 以实现自定义行为。 派生类可以 `<claimsAuthenticationManager>` 通过重写 <xref:System.Security.Claims.ClaimsAuthenticationManager.LoadCustomConfiguration%2A> 方法来处理这些元素，从而支持通过元素的子元素进行的配置。 为子元素定义的架构位于类的设计器中。  
  
 `<claimsAuthenticationManager>`元素设置 <xref:System.IdentityModel.Configuration.IdentityConfiguration.ClaimsAuthenticationManager%2A?displayProperty=nameWithType> 属性。  
  
## <a name="example"></a>示例  
  
```xml  
<system.identityModel>  
    <identityConfiguration name="MyIdentity">  
      <claimsAuthenticationManager type="MyNamespace.CustomClaimsAuthenticationManager, MyAssembly"/>
    </identityConfiguration>  
</system.identityModel>  
```
