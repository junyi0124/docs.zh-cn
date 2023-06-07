---
title: <clientCertificate> 的 <serviceCredentials>
ms.date: 03/30/2017
ms.assetid: 90ad03aa-2317-43dd-8a72-6d24cdcad15c
ms.openlocfilehash: 9df49777efc80f425cad3b353f95db523a027214
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91148912"
---
# <a name="clientcertificate-of-servicecredentials"></a>\<clientCertificate> 的 \<serviceCredentials>

定义一个用于在双工通信模式中对从服务发送到客户端的消息进行签名和加密的 X.509 证书。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceCredentials>**](servicecredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<clientCertificate>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<clientCertificate>
  <certificate />
  <authentication />
</clientCertificate>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 以下几节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  

 无。  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<authentication>](authentication-of-clientcertificate-element.md)|指定客户端证书的身份验证选项。|  
|[\<certificate>](certificate-of-clientcertificate-element.md)|指定要使用的证书。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<serviceCredentials>](servicecredentials.md)|指定要用于对服务进行身份验证的凭据以及与客户端凭据验证相关的设置。|  
  
## <a name="remarks"></a>备注  

 如果服务必须事先拥有客户端的证书才能与该客户端进行安全通信，则需要使用此元素。 使用双工通信模式时，会出现这种情况。 在更为典型的请求/响应模式中，客户端会将其证书包含在请求中，服务将使用该证书对发送回客户端的响应进行加密和签名。 但是，在双工通信模式中，服务没有来自客户端的请求，因此服务需要事先具有客户端的证书以确保发送到客户端的消息的安全。 因此，您必须通过带外协商来获取客户端的证书，并使用此元素指定该证书。 有关双工服务的详细信息，请参阅 [如何：创建双工协定](../../../wcf/feature-details/how-to-create-a-duplex-contract.md)。  
  
 在此元素中设置的证书用于仅针对配置有 `MutualCertificateDuplex` 消息安全身份验证模式的绑定加密发送到客户端的消息。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.X509InitiatorCertificateServiceElement>
- <xref:System.ServiceModel.Configuration.ServiceCredentialsElement.ClientCertificate%2A>
- <xref:System.ServiceModel.Configuration.X509InitiatorCertificateServiceElement>
- <xref:System.ServiceModel.Description.ServiceCredentials.ClientCertificate%2A>
- <xref:System.ServiceModel.Security.X509CertificateInitiatorServiceCredential>
- [如何：创建双工协定](../../../wcf/feature-details/how-to-create-a-duplex-contract.md)
- [安全行为](../../../wcf/feature-details/security-behaviors-in-wcf.md)
- [使用证书](../../../wcf/feature-details/working-with-certificates.md)
