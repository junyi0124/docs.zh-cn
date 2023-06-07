---
title: <authentication> of <clientCertificate> 元素
ms.date: 03/30/2017
ms.assetid: 4a55eea2-1826-4026-b911-b7cc9e9c8bfe
ms.openlocfilehash: 13296dbc2b3bc8836770197a1549586c841b4635
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91201598"
---
# <a name="authentication-of-clientcertificate-element"></a>\<authentication> of \<clientCertificate> 元素

指定服务所使用的客户端证书的身份验证行为。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceCredentials>**](servicecredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<clientCertificate>**](clientcertificate-of-servicecredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<authentication>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<authentication customCertificateValidatorType="namespace.typeName, [,AssemblyName] [,Version=version number] [,Culture=culture] [,PublicKeyToken=token]"
                certificateValidationMode="ChainTrust/None/PeerTrust/PeerOrChainTrust/Custom"
                includeWindowsGroups="Boolean"
                mapClientCertificateToWindowsAccount="Boolean"
                revocationMode="NoCheck/Online/Offline"
                trustedStoreLocation="CurrentUser/LocalMachine" />
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 以下几节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|customCertificateValidatorType|可选的字符串。 一个用于验证自定义类型的类型和程序集。 当 `certificateValidationMode` 设置为 `Custom` 时，必须设置此属性。|  
|certificateValidationMode|可选的枚举。 指定用来验证凭据的其中一种模式。 此特性的类型为 <xref:System.ServiceModel.Security.X509CertificateValidationMode>。 如果设置为 <xref:System.ServiceModel.Security.X509CertificateValidationMode.Custom?displayProperty=nameWithType>，则还必须提供 `customCertificateValidator`。 默认值为 <xref:System.ServiceModel.Security.X509CertificateValidationMode.ChainTrust?displayProperty=nameWithType>。|  
|includeWindowsGroups|可选的布尔值。 指定 Windows 组是否包含在安全上下文中。 将此属性设置为 `true` 会影响性能，因为这会导致完全组扩展。 如果不需要建立用户所属组的列表，请将此属性设置为 `false`。|  
|mapClientCertificateToWindowsAccount|布尔值。 指定是否可以使用证书将客户端映射到 Windows 标识。 为此，必须启用 Active Directory。|  
|revocationMode|可选的枚举。 用于检查吊销证书列表 (RCL) 的一种模式。 默认为 `Online`。 使用 HTTP 传输安全性时，将忽略此值。|  
|trustedStoreLocation|可选的枚举。 两个系统存储位置之一：`LocalMachine` 或 `CurrentUser`。 在向客户端协商服务证书时使用此值。 针对指定存储位置中的 " **受信任人** " 存储执行验证。 默认为 `CurrentUser`。|  
  
## <a name="customcertificatevalidatortype-attribute"></a>customCertificateValidatorType 属性  
  
|值|说明|  
|-----------|-----------------|  
|字符串|指定类型名称和程序集以及用于查找类型的其他数据。|  
  
## <a name="certificatevalidationmode-attribute"></a>certificateValidationMode 属性  
  
|值|描述|  
|-----------|-----------------|  
|枚举|下列值之一：None、PeerTrust、ChainTrust、PeerOrChainTrust 和 Custom。<br /><br /> 有关详细信息，请参阅使用 [证书](../../../wcf/feature-details/working-with-certificates.md)。|  
  
## <a name="revocationmode-attribute"></a>revocationMode 属性  
  
|值|描述|  
|-----------|-----------------|  
|枚举|下列值之一：NoCheck、Online 和 Offline。 有关详细信息，请参阅使用 [证书](../../../wcf/feature-details/working-with-certificates.md)。|  
  
## <a name="trustedstorelocation-attribute"></a>trustedStoreLocation 属性  
  
|值|描述|  
|-----------|-----------------|  
|枚举|以下值之一：`LocalMachine` 或 `CurrentUser`。 默认为 `CurrentUser`。 如果客户端应用程序在系统帐户下运行，则证书通常位于 `LocalMachine`。 如果客户端应用程序在用户帐户下运行，则证书通常位于 `CurrentUser`。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<clientCertificate>](clientcertificate-of-servicecredentials.md)|定义用于针对服务进行客户端身份验证的 X.509 证书。|  
  
## <a name="remarks"></a>备注  

 `<authentication>` 元素与 <xref:System.ServiceModel.Security.X509ClientCertificateAuthentication> 类相对应。 利用它您可以自定义对客户端进行身份验证的方式。 可以将 `certificateValidationMode` 属性设置为 `None`、`ChainTrust`、`PeerOrChainTrust`、`PeerTrust` 或 `Custom`。 默认情况下，该级别设置为 `ChainTrust` ，它指定每个证书都必须位于证书层次结构中，以在链顶部以 *根证书颁发机构* 结束。 这是最安全的模式。 您还可以将此值设置为 `PeerOrChainTrust`，该值指定受信任的链中的证书以及自行颁发的证书（对等信任）都被接受。 因为不需要从受信任的证书颁发机构那里购买自行颁发的证书，所以可以在开发和调试客户端和服务时使用此值。 在部署客户端时，请改用 `ChainTrust` 值。  
  
 还可以将该值设置为 `Custom`。 当该值设置为 `Custom` 值时，您还必须将 `customCertificateValidatorType` 属性设置为用于验证证书的程序集和类型。 若要创建您自己的自定义验证程序，必须从 <xref:System.IdentityModel.Selectors.X509CertificateValidator> 抽象类进行继承。 有关详细信息，请参阅 [如何：创建使用自定义证书验证程序的服务](../../../wcf/extending/how-to-create-a-service-that-employs-a-custom-certificate-validator.md)。  
  
## <a name="example"></a>示例  

 下面的代码指定 `<authentication>` 元素中的 X.509 证书和自定义验证类型。  
  
```xml  
<serviceBehaviors>
  <behavior name="myServiceBehavior">
    <clientCertificate>
      <certificate findValue="www.cohowinery.com"
                   storeLocation="CurrentUser"
                   storeName="TrustedPeople"
                   x509FindType="FindByIssuerName" />
      <authentication customCertificateValidatorType="MyTypes.Coho"
                      certificateValidationMode="Custom"
                      revocationMode="Offline"
                      includeWindowsGroups="false"
                      mapClientCertificateToWindowsAccount="true" />
    </clientCertificate>
  </behavior>
</serviceBehaviors>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Security.X509ClientCertificateAuthentication>
- <xref:System.ServiceModel.Security.X509CertificateValidationMode>
- <xref:System.ServiceModel.Security.X509CertificateInitiatorServiceCredential.Authentication%2A>
- <xref:System.ServiceModel.Configuration.X509InitiatorCertificateServiceElement.Authentication%2A>
- <xref:System.ServiceModel.Configuration.X509ClientCertificateAuthenticationElement>
- [安全行为](../../../wcf/feature-details/security-behaviors-in-wcf.md)
- [如何：创建使用自定义证书验证程序的服务](../../../wcf/extending/how-to-create-a-service-that-employs-a-custom-certificate-validator.md)
- [使用证书](../../../wcf/feature-details/working-with-certificates.md)
