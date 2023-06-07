---
title: <audienceUris>
ms.date: 03/30/2017
ms.assetid: 7a3d8515-d756-4afe-a22d-07cbe2217ee3
author: BrucePerlerMS
ms.openlocfilehash: c9787d8e0d8d66494bbf2dbd0e24ff39178a4cde
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91189898"
---
# \<audienceUris>

指定一组 Uri，这些 Uri 是信赖方 (RP) 的可接受标识符。 除非已针对允许的某个受众 URI 确定了令牌的范围，否则不会接受令牌。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<securityTokenHandlers>**](securitytokenhandlers.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<securityTokenHandlerConfiguration>**](securitytokenhandlerconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<audienceUris>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <securityTokenHandlers>  
      <securityTokenHandlerConfiguration>  
        <audienceUris mode=xs:string>  
          <add value=xs:string />  
          <clear />  
          <remove value=xs:string />  
        </audienceUris>  
      </securityTokenHandlerConfiguration>  
    </securityTokenHandlers>  
  </identityConfiguration>  
</system.identityModel>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|mode|一个 <xref:System.IdentityModel.Selectors.AudienceUriMode> 值，该值指定是否应将受众限制应用于传入令牌。 可能的值为 "始终"、"从不" 和 "BearerKeyOnly"。 默认值为 "Always"。 可选。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|`<add value=xs:string>`|将属性指定的 URI 添加 `value` 到 audienceUris 集合。 需要 `value` 属性。 URI 区分大小写。|  
|`<clear>`|清除 audienceUris 集合。 所有标识符都将从集合中删除。|  
|`<remove value=xs:string>`|`value`从 audienceUris 集合中移除特性指定的 URI。 需要 `value` 属性。 URI 区分大小写。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<securityTokenHandlerConfiguration>](securitytokenhandlerconfiguration.md)|为安全标记处理程序的集合提供配置。|  
  
## <a name="remarks"></a>备注  

 默认情况下，集合为空;使用 `<add>` 、 `<clear>` 和 `<remove>` 元素可修改集合。 <xref:System.IdentityModel.Tokens.SamlSecurityTokenHandler> 和 <xref:System.IdentityModel.Tokens.Saml2SecurityTokenHandler> 对象使用受众 uri 集合中的值来配置对象中允许的受众 uri 限制 <xref:System.IdentityModel.Tokens.SamlSecurityTokenRequirement> 。  
  
 `<audienceUris>`元素由 <xref:System.IdentityModel.Configuration.AudienceUriElementCollection> 类表示。 添加到集合中的单个 URI 由 <xref:System.IdentityModel.Configuration.AudienceUriElement> 类表示。  
  
> [!NOTE]
> 使用 `<audienceUris>` 元素作为元素的子元素 [\<identityConfiguration>](identityconfiguration.md) 已被弃用，但仍支持向后兼容。 元素上的设置将 `<securityTokenHandlerConfiguration>` 替代元素上的设置 `<identityConfiguration>` 。  
  
## <a name="example"></a>示例  

 下面的 XML 演示如何配置应用程序的可接受受众 Uri。 此示例配置单个 URI。 将接受范围为此 URI 的标记，所有其他标记将被拒绝。  
  
```xml  
<audienceUris>  
  <add value="http://localhost:19851/"/>  
</audienceUris>  
```
