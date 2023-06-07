---
title: <cookieHandler>
ms.date: 03/30/2017
ms.assetid: bfdc127f-8d94-4566-8bef-f583c6ae7398
author: BrucePerlerMS
ms.openlocfilehash: 5f5b432830a61adab324b2b6cd2ebe6eeccca7f0
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91189833"
---
# \<cookieHandler>

配置 <xref:System.IdentityModel.Services.CookieHandler> <xref:System.IdentityModel.Services.SessionAuthenticationModule> (SAM) 用于读取和写入 cookie 的。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel.services>**](system-identitymodel-services.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<federationConfiguration>**](federationconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<cookieHandler>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.identityModel.services>  
  <federationConfiguration>  
    <cookieHandler name=xs:string  
        path=Path  
        mode="Chunked||Custom||Default"  
        persistentSessionLifetime=xs:string  
        hideFromScript=xs:boolean  
        requireSSL=xs:boolean  
        domain=xs:string  
      <chunkedCookieHandler size=xs:int />  
      <customCookieHandler type="MyNamespace.MyCustomCookieHandler, MyAssembly" />  
    </cookieHandler>  
  </federationConfiguration>  
</system.identityModel.services>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|说明|  
|---------------|-----------------|  
|name|指定写入的任何 cookie 的基名称。 默认值为 "FedAuth"。|  
|path|指定写入的任何 cookie 的路径值。 默认值为 "HttpRuntime. AppDomainAppVirtualPath"。|  
|mode|<xref:System.IdentityModel.Services.CookieHandlerMode>值之一，它指定 SAM 使用的 cookie 处理程序的类型。 可以使用以下值：<br /><br /> -"Default"-与 "分块" 相同。<br />-"分块"-使用类的实例 <xref:System.IdentityModel.Services.ChunkedCookieHandler> 。 此 cookie 处理程序可确保各个 cookie 不会超过设置的最大大小。 它通过将一个逻辑 cookie "分块" 为多个网络中的多个 cookie 来实现这一点。<br />-"Custom"-使用派生自的自定义类的实例 <xref:System.IdentityModel.Services.CookieHandler> 。 派生类由 `<customCookieHandler>` 子元素引用。<br /><br /> 默认值为 "Default"。|  
|persistentSessionLifetime|指定持久会话的生存期。 如果为零，将始终使用瞬变会话。 默认值为 "0:0:0"，该值指定暂时性会话。 最大值为 "365:0:0"，该值指定的会话为365天。 应根据以下限制指定值： `<xs:pattern value="([0-9.]+:){0,1}([0-9]+:){0,1}[0-9.]+" />` ，其中最左端的值指定天，中间值 (如果存在) 指定小时数，最右侧的值 (如果存在) 指定分钟。|  
|requireSsl|指定是否为写入的任何 cookie 发出 "Secure" 标志。 如果设置了此值，则只能通过 HTTPS 使用登录会话 cookie。 默认值为“true”。|  
|hideFromScript|控制是否为写入的任何 cookie 发出 "HttpOnly" 标志。 某些 web 浏览器通过保持客户端脚本访问 cookie 值来服从此标志。 默认值为“true”。|  
|域|写入的任何 cookie 的域值。 默认值为 ""。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<chunkedCookieHandler>](chunkedcookiehandler.md)|配置 <xref:System.IdentityModel.Services.ChunkedCookieHandler> 。 仅当 `mode` 元素的属性 `<cookieHandler>` 为 "默认值" 或 "分块" 时，此元素才能存在。|  
|[\<customCookieHandler>](customcookiehandler.md)|设置自定义 cookie 处理程序类型。 如果 `mode` 元素的属性 `<cookieHandler>` 为 "Custom"，则此元素必须存在。 此属性的任何其他值不能出现此情况 `mode` 。 自定义类型必须派生自 <xref:System.IdentityModel.Services.CookieHandler> 类。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<federationConfiguration>](federationconfiguration.md)|包含配置 <xref:System.IdentityModel.Services.WSFederationAuthenticationModule> (WSFAM) 和 <xref:System.IdentityModel.Services.SessionAuthenticationModule> (SAM) 的设置。|  
  
## <a name="remarks"></a>备注  

 <xref:System.IdentityModel.Services.CookieHandler>负责在 HTTP 协议级别读取和写入原始 cookie。 可以配置 <xref:System.IdentityModel.Services.ChunkedCookieHandler> 从类派生的或自定义 cookie 处理程序 <xref:System.IdentityModel.Services.CookieHandler> 。  
  
 若要配置分块 cookie 处理程序，请将 mode 属性设置为 "分块" 或 "Default"。 默认块区大小为2000个字节，但您可以选择通过包括子元素来指定不同的区块大小 `<chunkedCookieHandler>` 。  
  
 若要配置自定义 cookie 处理程序，请将 mode 属性设置为 "Custom"。 还必须指定 `<customCookieHandler>` 引用自定义处理程序的类型的子元素。  
  
 `<cookieHandler>`元素由 <xref:System.IdentityModel.Services.CookieHandlerElement> 类表示。 在配置中指定的 cookie 处理程序可通过 <xref:System.IdentityModel.Services.Configuration.FederationConfiguration.CookieHandler%2A> <xref:System.IdentityModel.Services.Configuration.FederationConfiguration> 属性上设置的对象的属性获得 <xref:System.IdentityModel.Services.FederatedAuthentication.FederationConfiguration%2A?displayProperty=nameWithType> 。  
  
## <a name="example"></a>示例  

 下面的 XML 显示了一个 `<cookieHandler>` 元素。 在此示例中，由于 `mode` 未指定属性，SAM 将使用默认的 cookie 处理程序。 这是类的实例 <xref:System.IdentityModel.Services.ChunkedCookieHandler> 。 由于 `<chunkedCookieHandler>` 未指定子元素，将使用默认的块区大小。 不需要 HTTPS，因为 `requireSsl` 已设置属性 `false` 。  
  
> [!WARNING]
> 在此示例中，无需 HTTPS 即可写入会话 cookie。 这是因为 `requireSsl` 元素上的特性 `<cookieHandler>` 设置为 `false` 。 对于大多数生产环境，不建议使用此设置，因为这可能会带来安全风险。  
  
```xml  
<cookieHandler requireSsl="false" />  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.IdentityModel.Services.CookieHandler>
- <xref:System.IdentityModel.Services.ChunkedCookieHandler>
- <xref:System.IdentityModel.Services.SessionAuthenticationModule>
