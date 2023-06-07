---
title: 若 <identity>，表示集 <certificateReference>
ms.date: 03/30/2017
ms.assetid: ac359c65-c22d-42d2-97de-db53b77cebdb
ms.openlocfilehash: f3daa2dcdf9b464b51cfb9c883cbb828bccb42df
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91148992"
---
# <a name="certificatereference-for-identity"></a>若 \<identity>，表示集 \<certificateReference>

指定 X.509 证书验证的设置。 使用此标识连接到终结点的安全 Windows Communication Foundation (WCF) 客户端将验证服务器提供的声明是否包含用于构造此标识的标识声明。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<client>**](client.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpoint>**](endpoint-of-client.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<identity>**](identity.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<certificateReference>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<certificateReference findValue="String"
                      isChainIncluded="Boolean"
                      storeName="AddressBook/AuthRoot/CertificateAuthority/Disallowed/My/Root/TrustedPeople/TrustedPublisher"
                      storeLocation="LocalMachine/CurrentUser"
                      X509FindType="FindByThumbPrint/FindBySubjectName/FindBySubjectDistinguishedName/FindByIssuerName/FindByIssuerDistinguishedName/FindBySerialNumber/FindByTimeValid/FindByTimeNotYetValid/FindByTemplateName/FindByApplicationPolicy/FindByCertificatePolicy/FindByExtension/FindByKeyUsage/FindBySubjectKeyIdentifier">
</certificateReference>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|findValue|指定要在 X.509 证书存储中搜索的值。 此属性中包含的类型必须满足指定的 `X509FindType` 值的要求。 默认值为空字符串。|  
|isChainIncluded|一个布尔值，指定是否使用证书链来执行验证。|  
|storeLocation|指定客户端可用于验证服务器证书的证书存储的位置。<br /><br /> 有效值包括以下值：<br /><br /> -LocalMachine：分配给本地计算机的证书存储区。<br />-CurrentUser：分配给当前用户的证书存储区。<br /><br /> 默认值为 LocalMachine。<br /><br /> 此属性的类型为 <xref:System.Security.Cryptography.X509Certificates.StoreLocation>。|  
|storeName|指定要打开的 X.509 证书存储区的名称。<br /><br /> 有效值包括以下值：<br /><br /> -通讯簿：其他用户的证书存储区。<br />-AuthRoot：第三方证书颁发机构的证书存储 (Ca) 。<br />-CertificateAuthority：中间 Ca 的证书存储区。<br />-不允许：吊销的证书的证书存储区。<br />-My：个人证书的证书存储区。<br />-Root：受信任的根 Ca 的证书存储区。<br />-TrustedPeople：直接受信任的人和资源的证书存储区。<br />-TrustedPublisher：直接受信任的发布者的证书存储区。<br /><br /> 默认值为 My。<br /><br /> 此属性的类型为 <xref:System.Security.Cryptography.X509Certificates.StoreName>。|  
|X509FindType|指定要执行的 X.509 搜索的类型。 `findValue` 属性中包含的类型必须满足指定 X509FindType 的要求。<br /><br /> 有效值包括以下值：<br /><br /> -FindByThumbPrint<br />-Findbysubjectname) <br />-FindBySubjectDistinguishedName<br />- FindByIssuerName<br />- FindByIssuerDistinguishedName<br />-FindBySerialNumber<br />- FindByTimeValid<br />- FindByTimeNotYetValid<br />- FindByTemplateName<br />- FindByApplicationPolicy<br />- FindByCertificatePolicy<br />- FindByExtension<br />- FindByKeyUsage<br />- FindBySubjectKeyIdentifier<br /><br /> 默认值为 FindBySubjectDistinguishedName。<br /><br /> 此属性的类型为 <xref:System.Security.Cryptography.X509Certificates.X509FindType>。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<identity>](identity.md)|指定一些设置，与某个终结点交换消息的其他终结点可以使用这些设置对该终结点进行身份验证。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.CertificateReferenceElement>
- <xref:System.ServiceModel.Configuration.IdentityElement>
- <xref:System.ServiceModel.EndpointAddress>
- <xref:System.ServiceModel.EndpointAddress.Identity%2A>
