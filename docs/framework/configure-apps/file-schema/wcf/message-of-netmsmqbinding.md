---
title: <message> 的 <netMsmqBinding>
ms.date: 03/30/2017
ms.assetid: 6ebf0240-d7be-4493-b0fe-f00fd5989d77
ms.openlocfilehash: 5a4a4e8b645ee2c607988ac3031af537c93ca8c0
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/06/2020
ms.locfileid: "73736756"
---
# <a name="message-of-netmsmqbinding"></a>\<message> 的 \<netMsmqBinding>

在此 `netMsmqBinding` 绑定上定义 SOAP 消息安全设置。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<netMsmqBinding>**](netmsmqbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-netmsmqbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<message>**  

## <a name="syntax"></a>语法

```xml
<netMsmqBinding>
  <binding>
    <security>
      <message algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
               clientCredentialType="None/Windows/UserName/Certificate/CardSpace" />
    </security>
  </binding>
</netMsmqBinding>
```

## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。

### <a name="attributes"></a>特性

|属性|说明|
|---------------|-----------------|
|algorithmSuite|设置消息加密和密钥包装算法，这些算法用于针对通过 MSMQ 传输发送的消息实现基于消息的安全性。<br /><br /> 默认值为 `Aes256`。 此属性的类型为 <xref:System.ServiceModel.Security.SecurityAlgorithmSuite>。|
|clientCredentialType|指定针对通过 MSMQ 传输发送的消息执行客户端身份验证时要使用的凭据类型。 有效值包括以下值：<br /><br /> -None：这允许服务与匿名客户端交互。 服务和客户端都不需要凭据。<br />-Windows：这使得 SOAP 交换在 Windows 凭据的已验证上下文下。 此设置总是执行基于 Kerberos 的身份验证。<br />-UserName：这使服务可以要求使用用户名凭据对客户端进行身份验证。 在这种情况下，需要使用以下行为指定 `clientCredentials` 凭据 **：** Windows Communication Foundation （WCF）不支持发送密码摘要，也不支持使用密码派生密钥并使用此类密钥来实现消息安全性。 因此，在使用用户名凭据时，WCF 会强制使用交换。 此模式要求使用 `clientCredential` 行为和 `serviceCertificate` 在客户端指定服务证书。 <br /><br /> -Certificate：使服务可以要求使用证书对客户端进行身份验证。 在此情况下，需要使用 `clientCredentials` 行为指定客户端凭据。 在此情况下，需要使用 `clientCredentials` 行为，通过指定 `serviceCertificate` 来指定服务凭据。<br />-CardSpace：这允许服务要求使用 CardSpace 对客户端进行身份验证。 必须在 `serviceCertificate` 行为中设置 `clientCredential`。<br /><br /> 默认值为 `Windows`。 此属性的类型为 <xref:System.ServiceModel.MessageCredentialType>。|

### <a name="child-elements"></a>子元素

无

### <a name="parent-elements"></a>父元素

|元素|描述|
|-------------|-----------------|
|[\<security>](security-of-netmsmqbinding.md)|定义绑定的安全设置。|

## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Configuration.MessageSecurityOverMsmqElement>
- <xref:System.ServiceModel.Configuration.NetMsmqSecurityElement.Message%2A>
- <xref:System.ServiceModel.NetMsmqSecurity.Message%2A>
- <xref:System.ServiceModel.MessageSecurityOverMsmq>
- [WCF 中的队列](../../../wcf/feature-details/queues-in-wcf.md)
- [保护服务和客户端的安全](../../../wcf/feature-details/securing-services-and-clients.md)
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
