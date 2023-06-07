---
title: <windowsAuthentication> 的 <serviceCredentials>
ms.date: 03/30/2017
ms.assetid: e0709473-0997-4de3-8f49-783527309a48
ms.openlocfilehash: bda375959b535ce5f2996d594f719893164b0bd4
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91194994"
---
# <a name="windowsauthentication-of-servicecredentials"></a>\<windowsAuthentication> 的 \<serviceCredentials>

指定 Windows 服务凭据的设置。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceCredentials>**](servicecredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<windowsAuthentication>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<windowsAuthentication allowAnonymousLogons="Boolean"
                       includeWindowsGroups="Boolean" />
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`includeWindowsGroups`|一个可选的布尔值属性，指定系统是否将 Windows 组包含在安全上下文中。 默认为 `true`。<br /><br /> 将此属性设置为 `true` 会影响性能，因为这会导致完全组扩展。 如果不需要建立用户所属组的列表，请将此属性设置为 `false`。|  
|`allowAnonymousLogons`|一个可选的布尔值属性，指定是否允许匿名的未经过身份验证的调用方。 默认为 `false`。<br /><br /> 如果将绑定的 `clientCredentialType` 属性设置为 `Windows`，则系统不允许匿名的调用方。 这意味着，只有经过身份验证的域或工作组调用方才可以访问系统。 可以使用此属性重写此行为。<br /><br /> 使用此设置应特别小心。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<serviceCredentials>](servicecredentials.md)|指定要用于对服务进行身份验证的凭据以及与客户端凭据验证相关的设置。|  
  
## <a name="remarks"></a>备注  

 通过设置 `allowAnonymousLogons` 属性，使用此元素可指定是否允许匿名 Windows 用户访问。 此外，通过设置 `includeWindowsGroups` 属性还可以指定是否在 AuthorizationContext 中包含用户所属的组信息。 如果将它设置为 `true`（默认设置），服务就可以确定客户端所属的 Windows 组。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.WindowsServiceElement>
- <xref:System.ServiceModel.Configuration.ServiceCredentialsElement.WindowsAuthentication%2A>
- <xref:System.ServiceModel.Description.ServiceCredentials.WindowsAuthentication%2A>
- <xref:System.ServiceModel.Security.WindowsServiceCredential>
