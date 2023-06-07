---
title: <securityTokenHandlerConfiguration>
ms.date: 03/30/2017
ms.assetid: 28724cc6-020c-4a06-9a1f-d7594f315019
author: BrucePerlerMS
ms.openlocfilehash: 4c6affbc24a58424158e466fb732e9a3b3d6f1ed
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91157013"
---
# \<securityTokenHandlerConfiguration>

为标记处理程序的集合提供配置。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<securityTokenHandlers>**](securitytokenhandlers.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<securityTokenHandlerConfiguration>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <securityTokenHandlers>  
      <securityTokenHandlerConfiguration saveBootstrapContext=xs:boolean  
          maximumClockSkew=TimeSpan>  
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
|saveBootstrapContext|指定是否应在会话令牌中包含启动令牌。 还可以通过在元素上设置属性，在令牌处理程序集合上设置值 `saveBootstrapContext` [\<identityConfiguration>](identityconfiguration.md) 。 标记处理程序集合上设置的值将覆盖在服务上设置的值。|  
|maximumClockSkew|一个 <xref:System.TimeSpan> ，它指定允许的最大时钟偏差。 控制执行区分时间的操作时允许的最大时钟偏差，如验证登录会话的过期时间。 默认值为5分钟 "00:05:00"。 有关如何指定值的详细信息 <xref:System.TimeSpan> ，请参阅 [Timespan 值](../windows-workflow-foundation/index.md)。 通过在元素上设置属性，还可以在服务级别设置最大时钟偏差 `maximumClockSkew` [\<identityConfiguration>](identityconfiguration.md) 。 标记处理程序集合上设置的值将覆盖在服务上设置的值。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<audienceUris>](audienceuris.md)|指定作为此信赖方的可接受标识符的 Uri 集。 可选。|  
|[\<caches>](caches.md)|注册用于会话令牌和令牌重播检测的缓存。 可在服务级别或安全标记处理程序集合上指定。 可选。|  
|[\<certificateValidation>](certificatevalidation.md)|控制标记处理程序用于验证证书的设置。 可在服务级别或安全标记处理程序集合上指定。 如果为特定处理程序配置了其自己的验证程序，则会重写这些设置。 可选。|  
|[\<issuerNameRegistry>](issuernameregistry.md)|配置标记处理程序集合中的处理程序使用的颁发者名称注册表。 可选。|  
|[\<issuerTokenResolver>](issuertokenresolver.md)|注册由标记处理程序集合中的处理程序使用的颁发者令牌解析程序。 颁发者令牌解析器用于解析传入令牌和消息的签名令牌。 可选。|  
|[\<serviceTokenResolver>](servicetokenresolver.md)|注册令牌处理程序集合中的处理程序使用的服务令牌解析程序。 服务令牌解析器用于解析传入令牌和消息的加密令牌。 可选。|  
|[\<tokenReplayDetection>](tokenreplaydetection.md)|启用令牌重播检测并指定令牌的过期时间。 可在服务级别或安全标记处理程序集合上指定。 可选。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<securityTokenHandlers>](securitytokenhandlers.md)|指定注册到终结点的安全令牌处理程序的集合。|  
  
## <a name="remarks"></a>备注  

 本部分提供对象的属性值 <xref:System.IdentityModel.Tokens.SecurityTokenHandlerConfiguration> 。 此部分中配置的设置将覆盖在服务上配置的设置。 其中的某些设置可以通过在将处理程序添加到安全标记处理程序集合时指定的设置重写。  
  
## <a name="example"></a>示例  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <securityTokenHandlers>
      <securityTokenHandlerConfiguration>  
  
        <audienceUris>  
          <clear/>  
          <add value="http://www.example.com/myapp/" />  
        </audienceUris>  
  
        <issuerNameRegistry type="System.IdentityModel.Tokens.ConfigurationBasedIssuerNameRegistry, System.IdentityModel">  
          <trustedIssuers>  
            <add thumbprint="97249e1a … 4c9158de" name="contoso.com" />  
          </trustedIssuers>  
        </issuerNameRegistry>  
  
        <issuerTokenResolver type="MyNamespace.CustomTokenResolver, MyAssembly" />  
  
        <serviceTokenResolver type="MyNamespace.CustomTokenResolver, MyAssembly" />  
  
      </securityTokenHandlerConfiguration>  
    </securityTokenHandlers>  
  </identityConfiguration>  
</system.identityModel>  
```
