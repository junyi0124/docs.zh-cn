---
title: <transport> 的 <netMsmqBinding>
ms.date: 03/30/2017
ms.assetid: 72e1b338-39f0-4af1-a5d9-7a2fb79f6a0b
ms.openlocfilehash: 84a5437de851ecdb96d0463ec574186ba5f91d9e
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203873"
---
# <a name="transport-of-netmsmqbinding"></a>\<transport> 的 \<netMsmqBinding>

定义传输安全设置。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<netMsmqBinding>**](netmsmqbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-netmsmqbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<transport>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<netMsmqBinding>
  <binding>
    <security>
      <transport msmqAuthenticationMode="None/WindowsDomain/Certificate"
                 msmqEncryptionAlgorithm="RC4Stream/AES"
                 msmqProtectionLevel="None/Sign/EncryptAndSign"
                 msmqSecureHashAlgorithm="MD5/SHA1/SHA256/SHA512" />
    </security>
  </binding>
</netMsmqBinding>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|msmqAuthenticationMode|指定 MSMQ 传输必须采用什么方式对消息进行身份验证。 有效值包括以下值：<br /><br /> -None：无身份验证。<br />-WindowsDomain：身份验证机制使用 Active Directory 检索与消息关联的安全标识符的 x.509 证书。 然后使用它来检查队列的 ACL 以确保用户具有队列写权限。<br />-Certificate：通道从证书存储中检索证书。<br /><br /> 默认为 `WindowsDomain`。<br /><br /> 如果将此属性设置为 `None`，则 `msmqProtectionLevel` 属性也必须设置为 `None`。 此特性的类型为 <xref:System.ServiceModel.MsmqAuthenticationMode>|  
|msmqEncryptionAlgorithm|指定在消息队列管理器之间传输消息时用于在网络上对消息进行加密的算法。 有效值包括以下值：<br /><br /> -RC4Stream<br />-AES<br />-默认值为 `RC4Stream` 。 此属性的类型为 <xref:System.ServiceModel.MsmqEncryptionAlgorithm>。|  
|msmqProtectionLevel|指定在 MSMQ 传输级别采用什么方式来保护消息。 加密可确保消息的完整性，而签名和加密不仅可以确保消息的完整性，还可以确保消息的不可否认性。 也就是说，消息确实来自发件人，发件人是谁说。 有效值包括以下值：<br /><br /> -None：无保护。<br />-Sign：对消息进行签名。<br />-EncryptAndSign：对消息进行加密和签名。<br />-默认值为 `Sign` 。|  
|msmqSecureHashAlgorithm|指定用于计算消息摘要的哈希算法。 有效值包括以下值：<br /><br /> -MD5<br />-SHA1<br />-SHA256<br />-SHA512<br /><br /> 默认为 `SHA1`。 此属性的类型为 <xref:System.ServiceModel.MsmqSecureHashAlgorithm>。<br>由于 MD5 和 SHA1 出现冲突，Microsoft 建议 SHA256 或更好。|  
  
### <a name="child-elements"></a>子元素  

 无  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<security>](security-of-netmsmqbinding.md)|定义排队传输的传输安全设置。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.MsmqTransportSecurityElement>
- <xref:System.ServiceModel.Configuration.NetMsmqSecurityElement.Transport%2A>
- <xref:System.ServiceModel.NetMsmqSecurity.Transport%2A>
- <xref:System.ServiceModel.MsmqTransportSecurity>
- [WCF 中的队列](../../../wcf/feature-details/queues-in-wcf.md)
- [保护服务和客户端的安全](../../../wcf/feature-details/securing-services-and-clients.md)
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
