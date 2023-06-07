---
title: <certificate> 元素
ms.date: 03/30/2017
ms.assetid: 9b3d9233-ef35-477a-bf5d-efd1e80a52f4
ms.openlocfilehash: 8cc0404a5896dd23cffce6f1f77b91a2f01f23d2
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91151085"
---
# <a name="certificate-element"></a>\<certificate> 元素

指定要用于为对等客户端的消息进行签名和加密的 X.509 证书。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpointBehaviors>**](endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<clientCredentials>**](clientcredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<peer>**](peer-of-clientcredentials-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<certificate>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<certificate findValue="String"
             storeLocation="LocalMachine/CurrentUser"
             storeName="AddressBook/AuthRoot/CertificateAuthority/Disallowed/My/Root/TrustedPeople/TrustedPublisher"
             X509FindType="FindByThumbPrint/FindBySubjectName/FindBySubjectDistinguishedName/FindByIssuerName/FindByIssuerDistinguishedName/FindBySerialNumber/FindByTimeValid/FindByTimeNotYetValid/FindByTemplateName/FindByApplicationPolicy/FindByCertificatePolicy/FindByExtension/FindByKeyUsage/FindBySubjectKeyIdentifier" />
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`findValue`|一个字符串，包含要在 X.509 证书存储中搜索的值。 此属性中包含的类型必须满足指定 `x509FindType` 的要求。 默认值为空字符串。|  
|`storeLocation`|指定客户端可用于验证对等方的证书的 X.509 证书存储的位置。 有效值包括以下值：<br /><br /> -LocalMachine：分配给本地计算机的证书存储区。<br />-CurrentUser：分配给当前用户的证书存储区。<br /><br /> 默认值为 LocalMachine。|  
|`storeName`|指定要打开的 X.509 证书存储区的名称。 有效值包括以下值：<br /><br /> -通讯簿：其他用户的证书存储区。<br />-AuthRoot：第三方证书颁发机构的证书存储 (Ca) 。<br />-CertificateAuthority：用于中间证书颁发机构的证书存储 (Ca) 。<br />-不允许：吊销的证书的证书存储区。<br />-My：个人证书的证书存储区。<br />-Root：受信任的根证书颁发机构的证书存储 (Ca) 。<br />-TrustedPeople：直接受信任的人和资源的证书存储区。<br />-TrustedPublisher：直接受信任的发布者的证书存储区。<br /><br /> 默认值为 My。|  
|`X509FindType`|定义要执行的 X.509 搜索的类型。 有效值包括以下值：<br /><br /> -FindByThumbPrint<br />-Findbysubjectname) <br />-FindBySubjectDistinguishedName<br />- FindByIssuerName<br />- FindByIssuerDistinguishedName<br />-FindBySerialNumber<br />- FindByTimeValid<br />- FindByTimeNotYetValid<br />- FindByTemplateName<br />- FindByApplicationPolicy<br />- FindByCertificatePolicy<br />- FindByExtension<br />- FindByKeyUsage<br />- FindBySubjectKeyIdentifier<br /><br /> `findValue` 属性中包含的类型必须满足指定 `X509FindType` 的要求。<br /><br /> 默认值为 FindBySubjectDistinguishedName。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<peer>](peer-of-clientcredentials-element.md)|指定在向对等客户端进行身份验证时使用的凭据。|  
  
## <a name="remarks"></a>备注  

 此配置元素包含对对等网格中的邻居进行身份验证时使用的 X509Certificate2 实例。  
  
 有关对等编程的详细信息，请参阅对等 [网络](../../../wcf/feature-details/peer-to-peer-networking.md)。  
  
## <a name="example"></a>示例  

 下面的代码指定如何查找在对等方案中使用的证书。  
  
```xml  
<behaviors>
  <endpointBehaviors>
    <behavior name="MyEndpointBehavior">
      <clientCredentials>
        <peer>
          <certificate findValue="www.contoso.com"
                       storeLocation="LocalMachine"
                       x509FindType="FindByIssuerName" />
        </peer>
      </clientCredentials>
    </behavior>
  </endpointBehaviors>
</behaviors>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.PeerCredentialElement>
- <xref:System.ServiceModel.Configuration.PeerCredentialElement.Certificate%2A>
- <xref:System.ServiceModel.Configuration.X509PeerCertificateElement>
- <xref:System.ServiceModel.Security.PeerCredential.Certificate%2A>
- [使用证书](../../../wcf/feature-details/working-with-certificates.md)
- [对等网络](../../../wcf/feature-details/peer-to-peer-networking.md)
- [对等通道消息身份验证](/previous-versions/dotnet/netframework-3.5/aa967730(v=vs.90))
- [对等通道自定义身份验证](/previous-versions/dotnet/netframework-3.5/ms751447(v=vs.90))
- [保护对等通道应用程序](../../../wcf/feature-details/securing-peer-channel-applications.md)
