---
title: <caches>
ms.date: 03/30/2017
ms.assetid: 4651091b-3a20-40d8-b293-4408c0710143
author: BrucePerlerMS
ms.openlocfilehash: 791c5be8aa48db2b17a42a216ad2bf5e7b5a4bc1
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91189872"
---
# \<caches>

注册用于会话令牌和令牌重播检测的缓存。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<caches>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <caches>  
    </caches>  
  </identityConfiguration>  
</system.identityModel>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  

 无  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<sessionSecurityTokenCache>](sessionsecuritytokencache.md)|使用服务或安全标记处理程序集合为会话令牌注册缓存。|  
|[\<tokenReplayCache>](tokenreplaycache.md)|向服务或安全标记处理程序集合注册令牌重播缓存。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<identityConfiguration>](identityconfiguration.md)|指定服务级别标识设置。|  
|[\<securityTokenHandlerConfiguration>](securitytokenhandlerconfiguration.md)|为安全标记处理程序的集合提供配置。|  
  
## <a name="remarks"></a>备注  

 可以在元素 `<caches>` 下的服务级别指定元素， `<identityConfiguration>` 或在元素下的安全令牌处理程序集合级别指定元素 `<securityTokenHandlerConfiguration>` 。 标记处理程序集合上的设置将重写服务上指定的设置。  
  
 `<caches>`元素由 <xref:System.IdentityModel.Configuration.IdentityModelCachesElement> 类表示。 配置的缓存由 <xref:System.IdentityModel.Configuration.IdentityModelCaches> 类表示。  
  
## <a name="example"></a>示例  

 下面的 XML 演示了如何配置自定义缓存，用于保存)  (的会话安全令牌 <xref:System.IdentityModel.Tokens.SessionSecurityToken> 。 此配置取自 `ClaimsAwareWebFarm` 示例。  
  
```xml  
<caches>  
  <sessionSecurityTokenCache type="CacheLibrary.SharedSessionSecurityTokenCache, CacheLibrary">  
    <!--cacheServiceAddress points to the centralized session security token cache service running in the web farm.-->  
    <cacheServiceAddress url="http://localhost:4161/SessionSecurityTokenCacheService.svc" />  
  </sessionSecurityTokenCache>  
</caches>  
```
