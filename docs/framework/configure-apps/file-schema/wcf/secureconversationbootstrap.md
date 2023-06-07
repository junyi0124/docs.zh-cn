---
title: <secureConversationBootstrap>
ms.date: 03/30/2017
ms.assetid: 66b46f95-fa2d-4b5b-b6ce-0572ab0cdd50
ms.openlocfilehash: 02072c55e299d9e3a5d53b61c891a9ee9837ada0
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91183710"
---
# \<secureConversationBootstrap>

指定用于启动安全对话服务的默认值。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<secureConversationBootstrap>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<secureConversationBootstrap allowSerializedSigningTokenOnReply="Boolean"
                             authenticationMode="AuthenticationMode"
                             defaultAlgorithmSuite="SecurityAlgorithmSuite"
                             includeTimestamp="Boolean"
                             requireDerivedKeys="Boolean"
                             keyEntropyMode="ClientEntropy/ServerEntropy/CombinedEntropy"
                             messageProtectionOrder="SignBeforeEncrypt/SignBeforeEncryptAndEncryptSignature/EncryptBeforeSign"
                             messageSecurityVersion="WSSecurityJan2004/WSSecurityXXX2005"
                             requireDerivedKeys="Boolean"
                             requireSecurityContextCancellation="Boolean"
                             requireSignatureConfirmation="Boolean"
                             securityHeaderLayout="Strict/Lax/LaxTimestampFirst/LaxTimestampLast"
                             includeTimestamp="Boolean" />
```  
  
## <a name="type"></a>类型  

 `Type`  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`allowSerializedSigningTokenOnReply`|可选。 一个布尔值，指定是否可以在答复时使用序列化令牌。 默认值是 `false`。 使用双向绑定时，默认设置为 `true`，将忽略进行的任何设置。|  
|`authenticationMode`|指定在发起方和响应方之间使用的 SOAP 身份验证模式。<br /><br /> 默认值为 sspiNegotiated。<br /><br /> 此属性的类型为 <xref:System.ServiceModel.Configuration.AuthenticationMode>。|  
|`defaultAlgorithmSuite`|安全算法组定义了各种算法，如规范化、摘要式、密钥包装、签名、加密和密钥派生算法。 每个安全算法套件都定义了这些不同参数的值。 基于消息的安全性是使用这些算法实现的。<br /><br /> 此属性与选取不同于默认算法的算法集的其他平台一起使用。 在对此设置进行修改时，应该注意相关算法的优缺点。 此属性的类型为 <xref:System.ServiceModel.Security.SecurityAlgorithmSuite>。 默认为 `Basic256`。|  
|`includeTimestamp`|一个布尔值，指定是否每个消息都包含时间戳。 默认为 `true`。|  
|`keyEntropyMode`|指定用于保护消息的密钥的计算方法。 密钥只能基于客户端密钥材料、服务密钥材料或两者的组合。 有效值是：<br /><br /> -ClientEntropy：会话密钥基于客户端提供的密钥材料。<br />-ServerEntropy：会话密钥基于服务提供的密钥材料。<br />-CombinedEntropy：会话密钥基于客户端和服务提供的密钥材料。<br /><br /> 默认值为 CombinedEntropy。<br /><br /> 此属性的类型为 <xref:System.ServiceModel.Security.SecurityKeyEntropyMode>。|  
|`messageProtectionOrder`|设置对消息应用消息级安全算法的顺序。 有效值包括以下值：<br /><br /> -SignBeforeEncrypt：先签名，然后加密。<br />-SignBeforeEncryptAndEncryptSignature：签名、加密和加密签名。<br />-EncryptBeforeSign：先加密，然后签名。<br /><br /> 相互证书与 WS-Security 1.1 一起使用时，默认值为 SignBeforeEncryptAndEncryptSignature。  使用 WS-Security 1.0 时，默认值为 SignBeforeEncrypt。<br /><br /> 此属性的类型为 <xref:System.ServiceModel.Security.MessageProtectionOrder>。|  
|`messageSecurityVersion`|设置所使用的 WS-Security 的版本。 有效值包括以下值：<br /><br /> - WSSecurityJan2004<br />-WSSecurityXXX2005<br /><br /> 默认值为 WSSecurityXXX2005。 此属性的类型为 <xref:System.ServiceModel.MessageSecurityVersion>。|  
|`requireDerivedKeys`|一个布尔值，指定是否可以从原始校验密钥中派生密钥。 默认为 `true`。|  
|`requireSecurityContextCancellation`|一个布尔值，指定当不再需要安全上下文时是否应将其取消和终止。 默认为 `true`。|  
|`requireSignatureConfirmation`|一个布尔值，指定是否启用 WS-Security 签名确认。 当设置为 `true` 时，消息签名由响应方进行确认。 默认为 `false`。<br /><br /> 签名确认用于确认服务正在完全知晓请求的情况下做出响应。|  
|`securityHeaderLayout`|指定安全头中元素的排序。 有效值是：<br /><br /> Strict. 项按照“先声明后使用”的一般原则添加到安全性标头中。<br />严格. 项以任何符合 WSS: SOAP 消息安全的顺序添加到安全头中。<br />- LaxWithTimestampFirst. 项以任何符合 WSS: SOAP 消息安全的顺序添加到安全头中，但安全头中的第一个元素必须是 wsse:Timestamp 元素。<br />- LaxWithTimestampLast. 项以任何符合 WSS: SOAP 消息安全的顺序添加到安全头中，但安全头中的最后一个元素必须是 wsse:Timestamp 元素。<br /><br /> 默认值为 Strict。<br /><br /> 此元素的类型为 <xref:System.ServiceModel.Channels.SecurityHeaderLayout>。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<issuedTokenParameters>](issuedtokenparameters.md)|指定一个当前颁发的令牌。 此元素的类型为 <xref:System.ServiceModel.Configuration.IssuedTokenParametersElement>。|  
|[\<localClientSettings>](localclientsettings-element.md)|指定此绑定的本地客户端安全设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.LocalClientSecuritySettingsElement>。|  
|[\<localServiceSettings>](localservicesettings-element.md)|指定此绑定的本地服务安全设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.LocalServiceSecuritySettingsElement>。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<security>](security-of-custombinding.md)|指定自定义绑定的安全选项。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.LocalServiceSecuritySettingsElement>
- <xref:System.ServiceModel.Channels.SecurityBindingElement.LocalServiceSettings%2A>
- <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [绑定](../../../wcf/bindings.md)
- [扩展绑定](../../../wcf/extending/extending-bindings.md)
- [自定义绑定](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
- [如何：使用 SecurityBindingElement 创建自定义绑定](../../../wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md)
- [自定义绑定安全性](../../../wcf/samples/custom-binding-security.md)
