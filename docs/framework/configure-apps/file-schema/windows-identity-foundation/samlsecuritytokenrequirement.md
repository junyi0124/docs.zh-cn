---
title: <samlSecurityTokenRequirement>
ms.date: 03/30/2017
ms.assetid: 09202d12-88d3-49cc-953b-703bcc1690eb
author: BrucePerlerMS
ms.openlocfilehash: f93ec0007b537e306a570b166eaa4cd2fe7f81e2
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91157026"
---
# \<samlSecurityTokenRequirement>

为 <xref:System.IdentityModel.Tokens.SamlSecurityTokenHandler> 类、 <xref:System.IdentityModel.Tokens.Saml2SecurityTokenHandler> 类或其中任何一个类的派生类提供配置。 由类表示 <xref:System.IdentityModel.Tokens.SamlSecurityTokenRequirement> 。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<securityTokenHandlers>**](securitytokenhandlers.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<add>**](add.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<samlSecurityTokenRequirement>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <securityTokenHandlers>  
      <add>  
        <samlSecurityTokenRequirement
            issuerCertificateValidationMode="None||ChainTrust||PeerTrust||PeerOrChainTrust||Custom"  
            issuerCertificateRevocationMode="NoCheck||Offline||Online"  
            issuerCertificateTrustedStoreLocation="CurrentLocation||LocalMachine"  
            issuerCertificateValidator="Namespace.Class Assembly"  
            mapToWindows=xs:boolean  
          <nameClaimType value=xs:string />  
          <roleClaimType value=xs:string />  
        </samlSecurityTokenRequirement>  
      </add>  
    </securityTokenHandlers>  
  </identityConfiguration>  
</system.identityModel>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|mapToWindows|指定令牌处理程序是否应使用传入 UPN 声明将验证令牌映射到 Windows 帐户。 默认值为“false”。|  
|issuerCertificateRevocationMode|一个 <xref:System.Security.Cryptography.X509Certificates.X509RevocationMode> 值，该值指定要用于 x.509 证书的吊销模式。 默认值为 "Online"。|  
|issuerCertificateValidationMode|一个 <xref:System.ServiceModel.Security.X509CertificateValidationMode> 值，该值指定要用于 x.509 证书的验证模式。 默认值为 "PeerOrChainTrust"。|  
|issuerCertificateTrustedStoreLocation|一个 <xref:System.Security.Cryptography.X509Certificates.StoreLocation> 值，该值指定 x.509 证书存储区。 默认值为 "LocalMachine"。|  
|issuerCertificateValidator|派生自的自定义类型 <xref:System.IdentityModel.Selectors.X509CertificateValidator> 。 如果该 `issuerCertificateValidationMode` 属性为 "Custom"，则此类型的实例将用于颁发者证书验证。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<nameClaimType>](nameclaimtype.md)|设置指定属性的声明类型 <xref:System.Security.Principal.IIdentity.Name%2A> 。|  
|[\<roleClaimType>](roleclaimtype.md)|指定声明类型，该声明类型定义 <xref:System.Security.Claims.ClaimsIdentity> 由标记处理程序的方法返回的对象集合中的角色类型声明 <xref:System.IdentityModel.Tokens.SecurityTokenHandler.ValidateToken%2A> 。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<add>](add.md)|将指定的安全令牌处理程序添加到令牌处理程序集合。|  
  
## <a name="remarks"></a>备注  

 `<samlSecurityTokenRequirement>`元素由 <xref:System.IdentityModel.Tokens.SamlSecurityTokenRequirement> 对象模型中的类表示，用于 `SamlSecurityTokenRequirement` 在或上配置属性 <xref:System.IdentityModel.Tokens.SamlSecurityTokenHandler> <xref:System.IdentityModel.Tokens.Saml2SecurityTokenHandler> 。  
  
## <a name="example"></a>示例  
  
```xml  
<add type="System.IdentityModel.Tokens.SamlSecurityTokenHandler, System.IdentityModel">  
    <samlSecurityTokenRequirement issuerCertificateValidationMode="PeerOrChainTrust"  
                                  issuerCertificateRevocationMode="Online"  
                                  issuerCertificateTrustedStoreLocation="LocalMachine"  
                                  mapToWindows="false">  
  
        <nameClaimType value="http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name" />  
        <roleClaimType value="schemas.microsoft.com/ws/2006/04/identity/claims/role" />  
    </samlSecurityTokenRequirement>  
</add>  
```
