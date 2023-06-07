---
title: <trustedIssuers>
ms.date: 03/30/2017
ms.assetid: d818c917-07b4-40db-9801-8676561859fd
author: BrucePerlerMS
ms.openlocfilehash: 08cddd19f40f039f86e100cc7ee6a78633502eb2
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91185556"
---
# \<trustedIssuers>

配置基于配置的颁发者名称注册表 () 所使用的受信任颁发者证书的列表 <xref:System.IdentityModel.Tokens.ConfigurationBasedIssuerNameRegistry> 。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.identityModel>**](system-identitymodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<identityConfiguration>**](identityconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<securityTokenHandlers>**](securitytokenhandlers.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<securityTokenHandlerConfiguration>**](securitytokenhandlerconfiguration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<issuerNameRegistry>**](issuernameregistry.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<trustedIssuers>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<system.identityModel>  
  <identityConfiguration>  
    <securityTokenHandlers>  
      <securityTokenHandlerConfiguration>  
        <issuerNameRegistry>  
          <trustedIssuers>  
            <add thumbprint=xs:string name=xs:string>  
            <clear>  
            <remove thumbprint=xs:string>  
          </trustedIssuers>  
        </issuerNameRegistry>  
      </securityTokenHandlerConfiguration>  
    </securityTokenHandlers>  
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
|`<add thumbprint=xs:string name=xs:string>`|将证书添加到受信任的颁发者的集合中。 证书是用属性指定的 `thumbprint` 。 此属性是必需的，并且应该包含证书指纹的 ASN. 1 编码形式。 `name`属性是可选的，可用于指定证书的友好名称。|  
|`<clear>`|从受信任的颁发者集合中清除所有证书。|  
|`<remove thumbprint=xs:string>`|从受信任的颁发者集合中删除证书。 证书是用属性指定的 `thumbprint` 。 该属性是必选项。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<issuerNameRegistry>](issuernameregistry.md)|配置颁发者名称注册表。 **重要提示：** `type` 元素的属性 `<issuerNameRegistry>` 必须引用类，才能使 <xref:System.IdentityModel.Tokens.ConfigurationBasedIssuerNameRegistry> `<trustedIssuers>` 元素有效。|  
  
## <a name="remarks"></a>备注  

 Windows Identity Foundation (WIF) 提供类的单一实现，该类是类的一种实现 <xref:System.IdentityModel.Tokens.IssuerNameRegistry> <xref:System.IdentityModel.Tokens.ConfigurationBasedIssuerNameRegistry> 。 配置颁发者名称注册表将维护从配置加载的受信任颁发者的列表。 此列表将每个颁发者名称与 x.509 证书关联，该证书是验证颁发者生成的令牌签名所需的证书。 受信任颁发者证书的列表是在元素下指定的 `<trustedIssuers>` 。 列表中的每个元素都将助记键颁发者名称与 x.509 证书关联，该证书是验证该颁发者生成的令牌签名所需的。 受信任的证书是使用 "证书指纹" 的 "ASN. 1" 编码形式指定的，并使用元素添加集合 `<add>` 。 您可以使用和元素从列表中清除或删除颁发者 (证书 `<clear>`) `<remove>` 。  
  
 `type`元素的属性 `<issuerNameRegistry>` 必须引用类，才能使 <xref:System.IdentityModel.Tokens.ConfigurationBasedIssuerNameRegistry> `<trustedIssuers>` 元素有效。  
  
## <a name="example"></a>示例  

 下面的 XML 演示如何指定基于配置的颁发者名称注册表。  
  
```xml  
<issuerNameRegistry type="System.IdentityModel.Tokens.ConfigurationBasedIssuerNameRegistry, System.IdentityModel, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089">  
  <trustedIssuers>  
    <add thumbprint="9B74CB2F32 … B1DC01EF40D0" name="LocalSTS" />  
  </trustedIssuers>  
</issuerNameRegistry>  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.IdentityModel.Tokens.ConfigurationBasedIssuerNameRegistry>
- <xref:System.IdentityModel.Tokens.IssuerNameRegistry>
