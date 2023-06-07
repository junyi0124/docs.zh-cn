---
title: <identityConfiguration>
ms.date: 03/30/2017
ms.assetid: 1db76253-07da-447b-9e7a-3705c7228cf4
author: BrucePerlerMS
ms.openlocfilehash: 0fa8c574fd5663606cf081f1000a24884306edfe
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "70251986"
---
# \<identityConfiguration>

指定服务级别标识设置。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<identityConfiguration>**  

## <a name="syntax"></a>语法

```xml
<system.identityModel>
  <identityConfiguration
      name=xs:string
      saveBootstrapContext=xs:boolean>
      maximumClockSkew=TimeSpan >
  </identityConfiguration>
</system.identityModel>
```

## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

|属性|说明|
|---------------|-----------------|
|name|标识配置节的名称。 您可以使用此名称来引用特定的配置节。 如果未 `name` 指定任何属性，则节将定义默认配置。 默认配置始终用于被动联合身份验证方案。 有关详细信息，请参阅 [\<federationConfiguration>](federationconfiguration.md) 元素。|
|saveBootstrapContext|指定是否应在会话令牌中包含启动令牌。 还可以通过在元素上设置属性，在令牌处理程序集合上设置值 `saveBootstrapContext` [\<securityTokenHandlerConfiguration>](securitytokenhandlerconfiguration.md) 。 标记处理程序集合上设置的值将覆盖在服务上设置的值。|
|maximumClockSkew|一个 <xref:System.TimeSpan> ，它指定允许的最大时钟偏差。 控制执行区分时间的操作时允许的最大时钟偏差，如验证登录会话的过期时间。 默认值为5分钟 "00:05:00"。 有关如何指定值的详细信息 <xref:System.TimeSpan> ，请参阅[Timespan 值](../windows-workflow-foundation/index.md)。 通过在元素上设置属性，还可以对令牌处理程序集合设置最大时钟偏差 `maximumClockSkew` [\<securityTokenHandlerConfiguration>](securitytokenhandlerconfiguration.md) 。 标记处理程序集合上设置的值将覆盖在服务上设置的值。|

### <a name="child-elements"></a>子元素

|元素|描述|
|-------------|-----------------|
|[\<caches>](caches.md)|注册用于会话令牌和令牌重播检测的缓存。 可在服务级别或安全标记处理程序集合上指定。 可选。|
|[\<certificateValidation>](certificatevalidation.md)|控制标记处理程序用于验证证书的设置。 可在服务级别或安全标记处理程序集合上指定。 可选。|
|[\<claimsAuthenticationManager>](claimsauthenticationmanager.md)|为传入声明注册声明身份验证管理器。 可选。|
|[\<claimsAuthorizationManager>](claimsauthorizationmanager.md)|为传入声明注册声明授权管理器。 可选。|
|[\<claimTypeRequired>](claimtyperequired.md)|指定传入安全令牌所需的声明集。 可选。|
|[\<securityTokenHandlers>](securitytokenhandlers.md)|指定安全令牌处理程序的集合。 可以指定零个或多个安全标记处理程序集合。 可选。|
|[\<tokenReplayDetection>](tokenreplaydetection.md)|启用令牌重播检测并指定令牌的过期时间。 可在服务级别或安全标记处理程序集合上指定。 可选。|

### <a name="parent-elements"></a>父元素

|元素|描述|
|-------------|-----------------|
|[\<system.identityModel>](system-identitymodel.md)|为在应用程序中启用 Windows Identity Foundation （WIF）选项提供配置。|

## <a name="remarks"></a>注解

可以定义多个标识配置，每个配置都具有唯一的名称。 此行为如下所示：

1. 如果未 `<identityConfiguration>` 指定元素，则为。 默认标识配置是在运行时创建的，并使用默认值进行填充。

2. 如果指定单个 `<identityConfiguration>` 元素，则为。 这是默认的标识配置。 这并不重要。

3. 如果 `<identityConfiguration>` 指定了多个元素。 未命名元素指定默认标识配置。 建议在指定多个 `<identityConfiguration>` 元素时，其中一项应为未命名元素。

> [!WARNING]
> 如果指定多个 `<identityConfiguration>` 元素，则其中一个元素应该是未命名元素。 未命名的元素将是默认的标识配置。

 元素中指定的某些设置 `<identityConfiguration>` 可由安全令牌处理程序集合上的设置重写，或由单个安全令牌处理程序上的设置重写。

> [!IMPORTANT]
> 当使用 <xref:System.IdentityModel.Services.ClaimsPrincipalPermission> 或 <xref:System.IdentityModel.Services.ClaimsPrincipalPermissionAttribute> 类在代码中提供基于声明的访问控制时，元素引用的标识配置将 `<federationConfiguration>` 配置用于做出授权决定的声明授权管理器和策略。 即使在非被动 Web 方案（例如 Windows Communication Foundation （WCF）应用程序或不是基于 Web 的应用程序）情况下，也是如此。 如果应用程序不是被动 Web 应用程序，则 [\<claimsAuthorizationManager>](claimsauthorizationmanager.md) 仅应用所引用标识配置的元素（及其子策略元素，如果存在）。 忽略所有其他设置。 有关详细信息，请参阅 [\<federationConfiguration>](federationconfiguration.md) 元素。

`<identityConfiguration>`元素由 <xref:System.IdentityModel.Configuration.IdentityConfigurationElement> 类表示。 标识配置部分由 <xref:System.IdentityModel.Configuration.IdentityConfiguration> 类表示。

> [!IMPORTANT]
> 将以下元素指定为元素的子元素 `<identityConfiguration>` 已被弃用，但仍支持此行为以实现向后兼容性。 应改为在元素下指定这些元素 [\<securityTokenHandlerConfiguration>](securitytokenhandlerconfiguration.md) 。
>
> - [\<audienceUris>](audienceuris.md)
> - [\<issuerNameRegistry>](issuernameregistry.md)
> - [\<issuerTokenResolver>](issuertokenresolver.md)
> - [\<serviceTokenResolver>](servicetokenresolver.md)

## <a name="example"></a>示例

以下示例创建名为 "alternateConfiguration" 的标识配置。 标识配置指定默认设置。

```xml
<system.identityModel>
    <identityConfiguration name="alternateConfiguration"/>
</system.identityModel>
```

## <a name="see-also"></a>另请参阅

- <xref:System.IdentityModel.Configuration.IdentityConfiguration>
- <xref:System.IdentityModel.Configuration.IdentityConfigurationElement>
