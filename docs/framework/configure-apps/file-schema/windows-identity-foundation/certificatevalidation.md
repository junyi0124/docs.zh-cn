---
title: <certificateValidation>
ms.date: 03/30/2017
ms.assetid: 6c54c704-b55e-4631-88ff-4d4a5621554c
author: BrucePerlerMS
ms.openlocfilehash: 583fef7eb364c39890b3f9304770b383c1ea6d2a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91183502"
---
# \<certificateValidation>

控制标记处理程序用于验证证书的设置。 如果为特定处理程序配置了其自己的验证程序，则会重写这些设置。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<certificateValidation>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <certificateValidation  
      certificateValidationMode="None||ChainTrust||PeerTrust||PeerOrChainTrust||Custom"  
      revocationMode="NoCheck||Offline||Online"  
      trustedStoreLocation="CurrentLocation||LocalMachine" >  
    </certificateValidation>  
  </identityConfiguration>  
</system.identityModel>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|certificateValidationMode|一个 <xref:System.ServiceModel.Security.X509CertificateValidationMode> 值，该值指定要用于 x.509 证书的验证模式。 默认值为 "PeerOrChainTrust"。 若要指定自定义验证程序，请将此特性设置为 "Custom"，并使用元素指定验证程序 [\<certificateValidator>](certificatevalidator.md) 。 可选。|  
|revocationMode|一个 <xref:System.Security.Cryptography.X509Certificates.X509RevocationMode> 值，该值指定要用于 x.509 证书的吊销模式。 默认值为 "Online"。 可选。|  
|trustedStoreLocation|一个 <xref:System.Security.Cryptography.X509Certificates.StoreLocation> 值，该值指定 x.509 证书存储区。 默认值为 "LocalMachine"。 可选。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<certificateValidator>](certificatevalidator.md)|指定证书验证的自定义类型。 仅当 `certificateValidationMode` 元素的属性 [\<certificateValidation>](certificatevalidation.md) 设置为 "Custom" 时才使用此类型。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<identityConfiguration>](identityconfiguration.md)|指定服务级别标识设置。|  
|[\<securityTokenHandlerConfiguration>](securitytokenhandlerconfiguration.md)|为安全标记处理程序的集合提供配置。|  
  
## <a name="remarks"></a>备注  

 可以在元素 `<certificateValidation>` 下的服务级别指定元素， `<identityConfiguration>` 或在元素下的安全令牌处理程序集合级别指定元素 `<securityTokenHandlerConfiguration>` 。 标记处理程序集合上的设置将重写服务上指定的设置。 某些标记处理程序允许您在配置中指定证书验证设置。 各个标记处理程序上的设置将覆盖在服务级别和安全令牌处理程序集合上指定的设置。  
  
## <a name="example"></a>示例  
  
```xml  
<certificateValidation certificateValidationMode="PeerOrChainTrust"  
                       revocationMode="Online"  
                       trustedStoreLocation="LocalMachine" />  
```
