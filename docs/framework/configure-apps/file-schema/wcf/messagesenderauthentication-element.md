---
title: <messageSenderAuthentication> 元素
ms.date: 03/30/2017
ms.assetid: 8d979dfc-a6f9-42ec-96d5-7fbc13a48118
ms.openlocfilehash: e7e636571c0dbb1845438c22f7e7509dfc7987f9
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204783"
---
# <a name="messagesenderauthentication-element"></a>\<messageSenderAuthentication> 元素

指定用于对等消息发送方的身份验证选项。  
  
 有关对等编程的详细信息，请参阅对等 [网络](../../../wcf/feature-details/peer-to-peer-networking.md)。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpointBehaviors>**](endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<clientCredentials>**](clientcredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<peer>**](peer-of-clientcredentials-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<messageSenderAuthentication>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<messageSenderAuthentication customCertificateValidatorType= "namespace.typeName, [,AssemblyName] [,Version=version number] [,Culture=culture] [,PublicKeyToken=token]"
                             certificateValidationMode = "ChainTrust/None/PeerTrust/PeerOrChainTrust/Custom"
                             revocationMode="NoCheck/Online/Offline"
                             trustedStoreLocation="CurrentUser/LocalMachine" />
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 以下几节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`customCertificateValidatorType`|一个用于验证自定义类型的类型和程序集。 当 `certificateValidationMode` 设置为 `Custom` 时，必须设置此属性。|  
|`certificateValidationMode`|指定用来验证凭据的三种模式之一。 如果设置为 `Custom`，则还必须提供 `customCertificateValidator`。|  
|`revocationMode`|用于检查吊销证书列表 (CRL) 的一种模式。|  
|`trustedStoreLocation`|两个系统存储位置之一：`LocalMachine` 或 `CurrentUser`。 在向客户端协商服务证书时使用此值。 针对指定存储位置中的 " **受信任人** " 存储执行验证。|  
  
## <a name="customcertificatevalidatortype-attribute"></a>customCertificateValidatorType 属性  
  
|值|说明|  
|-----------|-----------------|  
|字符串|可选。 指定类型名称和程序集以及用于查找类型的其他数据。 至少需要命名空间和类型名称。 可选信息包括：程序集名称、版本号、区域性和公钥标记。|  
  
## <a name="certificatevalidationmode-attribute"></a>certificateValidationMode 属性  
  
|值|描述|  
|-----------|-----------------|  
|枚举|可选。 下列值之一：`None`、`PeerTrust`、`ChainTrust`、`PeerOrChainTrust` 和 `Custom`。 默认为 `ChainTrust`。 默认为 `ChainTrust`。<br /><br /> 有关详细信息，请参阅使用 [证书](../../../wcf/feature-details/working-with-certificates.md)。|  
  
## <a name="revocationmode-attribute"></a>revocationMode 属性  
  
|值|描述|  
|-----------|-----------------|  
|枚举|下列值之一：`NoCheck`、`Online` 和 `Offline`。 默认为 `Online`。<br /><br /> 有关详细信息，请参阅使用 [证书](../../../wcf/feature-details/working-with-certificates.md)。|  
  
## <a name="trustedstorelocation-attribute"></a>trustedStoreLocation 属性  
  
|值|描述|  
|-----------|-----------------|  
|枚举|以下值之一：`LocalMachine` 或 `CurrentUser`。 默认为 `CurrentUser`。 如果客户端应用程序在系统帐户下运行，则证书通常位于 `LocalMachine`。 如果客户端应用程序在用户帐户下运行，则证书通常位于 `CurrentUser`。 默认为 `CurrentUser`。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<peer>](peer-of-clientcredentials-element.md)|指定一个用于向对等服务证明客户端身份的凭据。|  
  
## <a name="remarks"></a>备注  

 如果选择了消息身份验证，则必须配置此元素。 对于输出通道，每条消息都使用提供的证书进行签名 [\<certificate>](certificate-element.md) 。 在将任一消息传递到应用程序之前，都会使用由此元素的 `customCertificateValidatorType` 属性指定的验证程序对照消息凭据对其进行检查。 验证程序可以接受或拒绝凭据。  
  
## <a name="example"></a>示例  

 下面的代码将消息发送方验证模式设置为 `PeerOrChainTrust`。  
  
```xml  
<behaviors>
  <endpointBehaviors>
    <behavior name="MyEndpointBehavior">
      <clientCredentials>
        <peer>
          <certificate findValue="www.contoso.com"
                       storeLocation="LocalMachine"
                       x509FindType="FindByIssuerName" />
          <messageSenderAuthentication certificateValidationMode="PeerOrChainTrust" />
          <messageSenderAuthentication certificateValidationMode="None" />
        </peer>
      </clientCredentials>
    </behavior>
  </endpointBehaviors>
</behaviors>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Security.X509PeerCertificateAuthentication>
- <xref:System.ServiceModel.Security.PeerCredential.MessageSenderAuthentication%2A>
- <xref:System.ServiceModel.Configuration.PeerCredentialElement.MessageSenderAuthentication%2A>
- <xref:System.ServiceModel.Configuration.X509PeerCertificateAuthenticationElement>
- [使用证书](../../../wcf/feature-details/working-with-certificates.md)
- [对等网络](../../../wcf/feature-details/peer-to-peer-networking.md)
- [对等通道消息身份验证](/previous-versions/dotnet/netframework-3.5/aa967730(v=vs.90))
- [对等通道自定义身份验证](/previous-versions/dotnet/netframework-3.5/ms751447(v=vs.90))
- [保护对等通道应用程序](../../../wcf/feature-details/securing-peer-channel-applications.md)
