---
title: <add> 的 <knownCertificates>
ms.date: 03/30/2017
ms.assetid: 128aaabe-3f1a-4c3b-b59f-898d0f02910f
ms.openlocfilehash: 453593918de15613edb801cca8a16c9dbf71aa90
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91176079"
---
# <a name="add-of-knowncertificates"></a>\<add> 的 \<knownCertificates>

向已知证书集合添加 X.509 证书。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceCredentials>**](servicecredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<issuedTokenAuthentication>**](issuedtokenauthentication-of-servicecredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<knownCertificates>**](knowncertificates.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<knownCertificates>
   <add findValue="String"
      storeLocation="CurrentUser/LocalMachine"
      storeName="AddressBook/AuthRoot/CertificateAuthority/Disallowed/My/Root/TrustedPeople/TrustedPublisher"
      x509FindType="FindByThumbprint/FindBySubjectName/FindBySubjectDistinguishedName/FindByIssuerName/FindByIssuerDistinguishedName/FindBySerialNumber/FindByTimeValid/FindByTimeNotYetValid/FindBySerialNumber/FindByTimeExpired/FindByTemplateName/FindByApplicationPolicy/FindByCertificatePolicy/FindByExtension/FindByKeyUsage/FindBySubjectKeyIdentifier"/>
</knownCertificates>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|findValue|字符串。 要搜索的值。|  
|storeLocation|枚举。 要搜索的两个存储位置之一。|  
|storeName|枚举。 要搜索的系统存储之一。|  
|x509FindType|枚举。 要搜索的证书字段之一。|  
  
## <a name="findvalue-attribute"></a>findValue 属性  
  
|值|说明|  
|-----------|-----------------|  
|字符串|值取决于搜索的字段（由 X509FindType 属性指定）。 例如，如果搜索指纹，则此值必须为十六进制数字字符串。|  
  
## <a name="x509findtype-attribute"></a>x509FindType 属性  
  
|值|描述|  
|-----------|-----------------|  
|枚举|值包括：FindByThumbprint、FindBySubjectName、FindBySubjectDistinguishedName、FindByIssuerName、FindByIssuerDistinguishedName、FindBySerialNumber、FindByTimeValid、FindByTimeNotYetValid、FindBySerialNumber、FindByTimeExpired、FindByTemplateName、FindByApplicationPolicy、FindByCertificatePolicy、FindByExtension、FindByKeyUsage 和 FindBySubjectKeyIdentifier。|  
  
## <a name="storelocation-attribute"></a>storeLocation 属性  
  
|值|描述|  
|-----------|-----------------|  
|枚举|CurrentUser 或 LocalMachine。|  
  
## <a name="storename-attribute"></a>storeName 属性  
  
|值|描述|  
|-----------|-----------------|  
|枚举|值包括：AddressBook、AuthRoot、CertificateAuthority、Disallowed、My、Root、TrustedPeople 和 TrustedPublisher。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<knownCertificates>](knowncertificates.md)|表示一个 X.509 证书集合，这些证书由安全令牌服务 (STS) 提供以用于验证安全令牌。|  
  
## <a name="remarks"></a>备注  

 颁发的令牌方案包含三个阶段。 在第一阶段中，尝试访问服务的客户端被称为 " *安全令牌服务*"。 然后，安全令牌服务对该客户端进行身份验证，随后向该客户端颁发一个令牌（通常是一个安全断言标记语言 (SAML) 令牌）。 接下来，客户端携带此令牌返回服务。 服务检查该令牌，以获取使其可以对该令牌并进而对该客户端进行身份验证的数据。 若要对令牌进行身份验证，安全令牌服务所使用的证书必须为该服务所知。  
  
 [\<issuedTokenAuthentication>](issuedtokenauthentication-of-servicecredentials.md)元素是任何此类安全令牌服务证书的储存库。 若要添加证书，请使用 [\<knownCertificates>](knowncertificates.md) 。 为每个证书插入一个[ \<add> 元素 \<knownCertificates> 元素](add-of-knowncertificates.md)，如下面的示例中所示。  
  
```xml  
<issuedTokenAuthentication>
  <knownCertificates>
    <add findValue="www.contoso.com"
         storeLocation="LocalMachine"
         storeName="My"
         X509FindType="FindBySubjectName" />
  </knownCertificates>
</issuedTokenAuthentication>
```  
  
 默认情况下，必须从安全令牌服务那里获取证书。 这些“已知的”证书可以确保只有合法的客户端才能访问服务。  
  
 若要查看客户端通过联合服务进行身份验证所需的条件，以及有关使用此配置元素的详细信息，请参阅 [如何：在联合身份验证服务上配置凭据](../../../wcf/feature-details/how-to-configure-credentials-on-a-federation-service.md)。 有关联合方案的详细信息，请参阅 [联合和颁发的令牌](../../../wcf/feature-details/federation-and-issued-tokens.md)。  
  
## <a name="example"></a>示例  

 下面的示例将证书添加到任意 STS 证书的存储库。  
  
```xml  
<serviceBehaviors>
  <behavior name="myServiceBehavior">
    <serviceCredentials>
      <issuedTokenAuthentication>
        <knownCertificates>
          <add findValue="www.contoso.com"
               storeLocation="LocalMachine"
               storeName="CertificateAuthority"
               x509FindType="FindByIssuerName" />
        </knownCertificates>
      </issuedTokenAuthentication>
    </serviceCredentials>
  </behavior>
</serviceBehaviors>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator>
- <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator.AllowedAudienceUris%2A>
- <xref:System.IdentityModel.Selectors.SamlSecurityTokenAuthenticator.AudienceUriMode%2A>
- <xref:System.ServiceModel.Configuration.IssuedTokenServiceElement.KnownCertificates%2A>
- <xref:System.ServiceModel.Configuration.X509CertificateTrustedIssuerElementCollection>
- <xref:System.ServiceModel.Configuration.X509CertificateTrustedIssuerElement>
- <xref:System.ServiceModel.Security.IssuedTokenServiceCredential.KnownCertificates%2A>
- [\<knownCertificates>](knowncertificates.md)
- [使用证书](../../../wcf/feature-details/working-with-certificates.md)
- [联合令牌与颁发的令牌](../../../wcf/feature-details/federation-and-issued-tokens.md)
- [如何：在联合身份验证服务上配置凭据](../../../wcf/feature-details/how-to-configure-credentials-on-a-federation-service.md)
- [保护服务和客户端的安全](../../../wcf/feature-details/securing-services-and-clients.md)
