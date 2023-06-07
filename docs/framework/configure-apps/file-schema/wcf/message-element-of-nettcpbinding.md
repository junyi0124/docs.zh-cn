---
title: <message> 的元素 <netTcpBinding>
ms.date: 03/30/2017
ms.assetid: 1d71edd9-c085-4c2e-b6d3-980c313366f9
ms.openlocfilehash: ab767a5a1179de81bf9a8adc61799ede2d915ac1
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204900"
---
# <a name="message-element-of-nettcpbinding"></a>\<message> 的元素 \<netTcpBinding>

为使用配置的终结点定义消息级安全性要求的类型 [\<netTcpBinding>](nettcpbinding.md) 。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<netTcpBinding>**](nettcpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-nettcpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<message>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<message algorithmSuite="System.Servicemodel.Security.SecurityAlgorithmsuite"
         clientCredentialType="None/Windows/UserName/Certificate/IssuedToken" />
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`algorithmSuite`|设置消息加密和密钥包装算法。 算法和密钥大小由 <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> 类确定。 这些算法与“安全策略语言”(WS-SecurityPolicy) 规范中指定的算法一致。<br /><br /> 下表中显示了可能的值。 默认值是 `Basic256`。<br /><br /> 如果服务绑定指定的 `algorithmSuite` 值不等于默认值，并且你使用 Svcutil.exe 生成配置文件，则不会正确生成该文件，你必须手动编辑此配置文件，将此属性设置为所需的值。|  
|`clientCredentialType`|指定要在使用基于消息的安全性执行客户端身份验证时使用的凭据类型。 下表中显示了可能的值。 默认值是 `UserName`。 此属性的类型为 <xref:System.ServiceModel.MessageCredentialType>。|  
  
## <a name="algorithmsuite-attribute"></a>algorithmSuite 属性  
  
|值|描述|  
|-----------|-----------------|  
|Basic128|使用 Aes128 加密，对消息摘要使用 Sha1，对密钥包装使用 Rsa-oaep-mgf1p。|  
|Basic192|使用 Aes192 加密，对消息摘要使用 Sha1，对密钥包装使用 Rsa-oaep-mgf1p。|  
|Basic256|使用 Aes256 加密，对消息摘要使用 Sha1，对密钥包装使用 Rsa-oaep-mgf1p。|  
|Basic256Rsa15|对消息加密使用 Aes256，对消息摘要使用 Sha1，对密钥包装使用 Rsa15。|  
|Basic192Rsa15|对消息加密使用 Aes192，对消息摘要使用 Sha1，对密钥包装使用 Rsa15。|  
|TripleDes|使用 TripleDes 加密，对消息摘要使用 Sha1，对密钥包装使用 Rsa-oaep-mgf1p。|  
|Basic128Rsa15|对消息加密使用 Aes128，对消息摘要使用 Sha1，对密钥包装使用 Rsa15。|  
|TripleDesRsa15|使用 TripleDes 加密，对消息摘要使用 Sha1，对密钥包装使用 Rsa15。|  
|Basic128Sha256|对消息加密使用 Aes256，对消息摘要使用 Sha256，对密钥包装使用 Rsa-oaep-mgf1p。|  
|Basic192Sha256|对消息加密使用 Aes192，对消息摘要使用 Sha256，对密钥包装使用 Rsa-oaep-mgf1p。|  
|Basic256Sha256|对消息加密使用 Aes256，对消息摘要使用 Sha256，对密钥包装使用 Rsa-oaep-mgf1p。|  
|TripleDesSha256|对消息加密使用 TripleDes，对消息摘要使用 Sha256，对密钥包装使用 Rsa-oaep-mgf1p。|  
|Basic128Sha256Rsa15|对消息加密使用 Aes128，对消息摘要使用 Sha256，对密钥包装使用 Rsa15。|  
|Basic192Sha256Rsa15|对消息加密使用 Aes192，对消息摘要使用 Sha256，对密钥包装使用 Rsa15。|  
|Basic256Sha256Rsa15|对消息加密使用 Aes256，对消息摘要使用 Sha256，对密钥包装使用 Rsa15。|  
|TripleDesSha256Rsa15|对消息加密使用 TripleDes，对消息摘要使用 Sha256，对密钥包装使用 Rsa15。|  
  
## <a name="clientcredentialtype-attribute"></a>clientCredentialType 属性  
  
|值|描述|  
|-----------|-----------------|  
|无|允许服务与匿名客户端交互。 对于服务，这表示服务不需要任何客户端凭据。 对于客户端，这表示客户端不提供任何客户端凭据。|  
|Windows|允许 SOAP 交换在已通过身份验证的 Windows 凭据上下文中执行。|  
|UserName|允许服务要求使用 UserName 凭据对客户端进行身份验证。 WCF 不支持发送密码摘要，也不支持使用密码派生密钥，然后用这些密钥来确保消息的安全性。 因此，在使用用户名凭据时，WCF 会强制保护传输。 这种凭据模式将产生可互操作的交换或不可互操作的协商，具体取决于 `negotiateServiceCredential` 属性。|  
|证书|允许服务要求使用证书对客户端进行身份验证。 如果使用消息安全模式并且将 `negotiateServiceCredential` 属性设置为 `false`，则必须向客户端提供服务证书。|  
|IssuedToken|指定自定义令牌，该令牌通常由安全令牌服务 (STS) 颁发。|  
  
### <a name="child-elements"></a>子元素  

 无  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<security>](security-of-nettcpbinding.md)|定义 <xref:System.ServiceModel.Configuration.NetTcpBindingElement>的安全功能。|  
  
## <a name="remarks"></a>备注  

 消息使用消息级安全性实现 SOAP 消息的完整性和保密性，还用于通信对等方的相互身份验证。 如果在绑定上选择此安全模式，则使用消息安全性绑定元素配置信道堆栈，并且 SOAP 消息可按照 WS-Security* 标准进行保护。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.MessageSecurityOverTcp>
- <xref:System.ServiceModel.Configuration.NetTcpSecurityElement.Message%2A>
- <xref:System.ServiceModel.NetTcpSecurity.Message%2A>
- <xref:System.ServiceModel.Configuration.NetTcpSecurityElement>
- [保护服务和客户端的安全](../../../wcf/feature-details/securing-services-and-clients.md)
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
