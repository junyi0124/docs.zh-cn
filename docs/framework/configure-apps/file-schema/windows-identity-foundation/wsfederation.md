---
title: <wsFederation>
ms.date: 03/30/2017
ms.assetid: c537f770-68bd-4f82-96ad-6424ad91369f
author: BrucePerlerMS
ms.openlocfilehash: 93661af6c907d8cce1a73536a8ebca7bd53c00d8
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91185504"
---
# \<wsFederation>

为 <xref:System.IdentityModel.Services.WSFederationAuthenticationModule> (WSFAM) 提供配置。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel.services>**](system-identitymodel-services.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<federationConfiguration>**](federationconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<wsFederation>**  
  
## <a name="syntax"></a>语法  
  
```xml
<system.identityModel.services>  
  <federationConfiguration>  
    <wsFederation authenticationType=xs:string (URI)  
                  freshness=xs:decimal  
                  homerealm=xs:string (URI)  
                  issuer=xs:string (URI)  
                  persistentCookiesOnPassiveRedirects=xs:boolean  
                  passiveRedirectEnabled=xs:boolean  
                  policy=xs:string (URI)  
                  realm=xs:string (URI)  
                  reply=xs:string (URI)  
                  request=xs:string (URI)  
                  requestPtr=xs:string (URI)  
                  requireHttps=xs:boolean  
                  resource=xs:string (URI)  
                  signInQueryString=xs:string  
                  signOutQueryString=xs:string  
                  signOutReply=xs:string (URL)  
    </wsFederation>  
  </federationConfiguration>  
</system.identityModel.services>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|authenticationType|一个认证类型的 URI。 设置 WS-FEDERATION 登录请求 wauth 参数。 可选。 默认值为空字符串，指定在请求中不包含 wauth 参数。|  
|时效性|身份验证请求所需的最大生存期（以分钟为单位）。 设置 WS-Federation 登录请求 wfresh 参数。 可选。 默认值为零。 可选。 **警告：**  在 .NET Framework 4.5 的下一版本中，该 `freshness` 属性的类型为 `xs:string` ，其默认值将为 `null` 。|  
|homeRealm|标识提供程序的主领域 (IdP) 用于身份验证。 设置 WS-Federation 登录请求 whr 参数。 可选。 默认值为空字符串，该字符串指定在请求中不包含瓦时参数。|  
|颁发者|预期令牌颁发者的 URI。 设置 WS 联合身份验证登录请求和需要的注销请求的基 URL。|  
|persistentCookiesOnPassiveRedirects|指定是否在身份验证时发出永久性 cookie。 可选。 默认值为 "false"，则不颁发 cookie。|  
|passiveRedirectEnabled|指定是否启用 WSFAM 以将未经授权的请求自动重定向到 STS。 可选。 默认值为 "true"，将自动重定向未经授权的请求。|  
|policy|一个 URL，它指定用于登录请求的相关策略的位置。 默认值为一个空字符串。 设置 WS-Federation 登录请求 wp 参数。 可选。 默认值为空字符串，指定在请求中不包含 wp 参数。|  
|realm|请求领域的 URI。  (一个 URI，用于标识 (RP) 到 security token service (STS) 的信赖方。 ) 设置请求 wtrealm WS 联合身份验证登录请求参数。 必需。|  
|回邮|标识依赖方 (RP) 应用程序的地址的 URL 要接受来自安全标志服务 (STS) 的回复。 设置 WS-FEDERATION 登录请求 wreply 参数。 可选。 默认值为空字符串，指定在请求中不包含 wreply 参数。|  
|request|令牌颁发请求。 设置 WS-Federation 登录请求 wreq 参数。 可选。 默认值为空字符串，指定在请求中不包含 wreq 参数。 如果未在请求中包括 wreq 或 wreqptr 参数，则表示 STS 知道要颁发的令牌类型。|  
|requestPtr|一个指定令牌颁发请求位置的 URI。 设置 request wreqptr 参数。 可选。 默认值为空字符串，指定在请求中不包含 wreqptr 参数。 如果未在请求中包括 wreq 或 wreqptr 参数，则表示 STS 知道要颁发的令牌类型。|  
|requireHttps|指定与 security token service (STS) 的通信是否必须使用 HTTPS 协议。 可选。 默认值为 "true"，则必须使用 HTTPS。|  
|resource|标识访问的资源、依赖方 (RP)和对安全标志服务 (STS) 的 URI。 可选。 设置 WS-FEDERATION 登录请求 wres 参数。 可选。 默认值为空字符串，指定在请求中不包含 wres 参数。 **注意：**  wres 是一个旧参数。 指定 `realm` 要改用 wtrealm 参数的属性。|  
|signInQueryString|提供一个扩展点，用于在 WS 联合身份验证登录请求 URL 中指定应用程序定义的查询参数。 可选。 默认值为空字符串，指定在请求中不应包含其他参数。 使用以下格式将参数指定为查询字符串片段： `"param1=value1&param2=value2&param3=value3"` 等。 **注意：**  在配置文件中，查询字符串中的 "&" 字符必须使用其实体引用来指定 `&` 。|  
|signOutQueryString|提供一个扩展点，用于在 WS 联合身份验证登录请求 URL 中指定应用程序定义的查询参数。 可选。 默认值为空字符串，指定在请求中不应包含其他参数。 使用以下格式将参数指定为查询字符串片段： `"param1=value1&param2=value2&param3=value3"` 等。 **注意：**  在配置文件中，查询字符串中的 "&" 字符必须使用其实体引用来指定 `&` 。|  
|signOutReply|指定在通过 WS 联合身份验证协议进行被动注销过程中，security token service (STS) 的客户端应重定向到的 URL。 设置 WS 联合身份验证注销请求上的 wreply 参数。 可选。 默认值为空字符串，指定在请求中不应包含其他参数。|  
  
### <a name="child-elements"></a>子元素  

 无  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<federationConfiguration>](federationconfiguration.md)|包含配置 <xref:System.IdentityModel.Services.WSFederationAuthenticationModule> (WSFAM) 和 <xref:System.IdentityModel.Services.SessionAuthenticationModule> (SAM) 的设置。|  
  
## <a name="remarks"></a>备注  

 您可以使用 `<wsFederation>` 元素来配置 WSFAM 的默认 WS 联合身份验证参数设置和默认行为。 在元素下定义的 WS 联合身份验证参数设置 `<wsFederation>` 由 <xref:System.IdentityModel.Services.WSFederationAuthenticationModule> 类公开。 这些属性对于 WSFAM 发出的每个请求都是相同的。 通过为 WSFAM 公开的事件添加事件处理程序，可以在请求处理过程中动态更改 WS 联合身份验证参数;例如， <xref:System.IdentityModel.Services.WSFederationAuthenticationModule.RedirectingToIdentityProvider> 事件。 有关详细信息，请参阅类的文档 <xref:System.IdentityModel.Services.WSFederationAuthenticationModule> 。  
  
 `<wsFederation>`元素由 <xref:System.IdentityModel.Services.Configuration.WSFederationElement> 类表示。 配置对象本身由 <xref:System.IdentityModel.Services.Configuration.WsFederationConfiguration> 类表示。 单个 <xref:System.IdentityModel.Services.Configuration.WsFederationConfiguration> 实例是在 <xref:System.IdentityModel.Services.Configuration.FederationConfiguration> 通过属性访问的对象上设置的，为 <xref:System.IdentityModel.Services.FederatedAuthentication.FederationConfiguration%2A?displayProperty=nameWithType> WSFAM 提供配置。  
  
## <a name="example"></a>示例  

 下面的 XML 显示了一个 `<wsFederation>` 元素，该元素指定 WSFAM 的设置。  
  
> [!WARNING]
> 在此示例中，WSFAM 不需要使用 HTTPS。 这是因为 `requireHttps` `<wsFederation>` 已设置了元素的属性 `false` 。 对于大多数生产环境，不建议使用此设置，因为这可能会带来安全风险。  
  
```xml
<wsFederation passiveRedirectEnabled="true"
              issuer="http://localhost:15839/wsFederationSTS/Issue"
              realm="http://localhost:50969/"
              reply="http://localhost:50969/"
              requireHttps="false"
              signOutReply="http://localhost:50969/SignedOutPage.html"
              signOutQueryString="Param1=value2&Param2=value2"
              persistentCookiesOnPassiveRedirects="true" />
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.IdentityModel.Services.WSFederationAuthenticationModule>
- <xref:System.IdentityModel.Services.FederatedAuthentication.FederationConfiguration%2A?displayProperty=nameWithType>
