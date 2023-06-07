---
title: <localClientSettings> 元素
ms.date: 03/30/2017
ms.assetid: 4680ace5-f4e1-4fcb-b9d8-a4a4af5cd7ae
ms.openlocfilehash: 19eaea71fdaad1b945524cca5cf15634e0b0fa14
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91158729"
---
# <a name="localclientsettings-element"></a>\<localClientSettings> 元素

指定此绑定的本地客户端安全设置。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<localClientSettings>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<security>
   <localClientSettings cacheCookies="Boolean"
                        cookieRenewalThresholdPercentage="Integer"
                        detectReplays="Boolean"
                        maxClockSkew="TimeSpan"
                        maxCookieCachingTime="TimeSpan"
                        reconnectTransportOnFailure="Boolean"
                        replayCacheSize="Integer"
                        replayWindow="TimeSpan"
                        sessionKeyRenewalInterval="TimeSpan"
                        sessionKeyRolloverInterval="TimeSpan"
                        timestampValidityDuration="TimeSpan" />
</security>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`cacheCookies`|一个布尔值，指定是否启用 Cookie 缓存。 默认为 `false`。|  
|`cookieRenewalThresholdPercentage`|一个整数，指定可续订的最大 Cookie 百分比。 该值应介于 0 至 100 之间（包括这两个数）。 默认值为 90。|  
|`detectReplays`|一个布尔值，指定是否自动检测和处理针对通道的重放攻击。 默认为 `false`。|  
|`maxClockSkew`|一个 <xref:System.TimeSpan>，指定通信双方的系统时钟之间的最大时间差异。 默认值为“00:05:00”。<br /><br /> 当此值被设置为默认值时，接收方所接受的消息的发送时间时间戳最多可比消息接收时间晚或早 5 分钟。 未通过发送时间测试的消息会被拒绝。 此设置与 `replayWindow` 属性结合使用。|  
|`maxCookieCachingTime`|一个 <xref:System.TimeSpan>，指定 Cookie 的最长生存期。 默认值为“10675199.02:48:05.4775807”。|  
|`reconnectTransportOnFailure`|一个布尔值，指定使用 WS-ReliableMessaging 的连接是否将在传输失败后尝试重新连接。 默认值为 `true`，表示将进行无限次重新连接尝试。 非活动超时能够打断此循环；非活动超时在无法重新连接通道时使其引发异常。|  
|`replayCacheSize`|一个正整数，指定用于重播检测的缓存 Nonce 的数目。 如果超出此限制，则会移除最旧的 Nonce，并且为新消息创建一个新的 Nonce。 默认值为 500000。|  
|`replayWindow`|一个 <xref:System.TimeSpan>，指定单个消息 Nonce 有效的持续时间。<br /><br /> 在此持续时间之后，如果发送的消息与之前发送的消息具有相同的 Nonce，则该消息将被拒绝。 此属性与 `maxClockSkew` 属性结合使用，以防止遭受重放攻击。 攻击者可以在其重放时间窗口过期之后重放消息。 但是，该消息将无法通过 `maxClockSkew` 测试；如果消息的发送时间时间戳比消息接收时间晚或早指定的时间，则该测试会拒绝消息。|  
|`sessionKeyRenewalInterval`|一个 <xref:System.TimeSpan>，指定一段持续时间，发起方在此段时间之后将续订安全会话的密钥。 默认值为“10:00:00”。|  
|`sessionKeyRolloverInterval`|一个 <xref:System.TimeSpan>，指定在密钥续订期间，上一个会话密钥对于传入消息有效的时间间隔。 默认值为“00:05:00”。<br /><br /> 在密钥续订期间，客户端和服务器必须总是使用最新的可用密钥来发送消息。 在延期时间到期前，双方都可以接受以上一个会话密钥加密的传入消息。|  
|`timestampValidityDuration`|一个值为正的 <xref:System.TimeSpan>，指定时间戳有效的持续时间。 默认值为“00:15:00”。|  
  
### <a name="child-elements"></a>子元素  

 无  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<security>](security-of-custombinding.md)|指定自定义绑定的安全选项。|  
|[\<secureConversationBootstrap>](secureconversationbootstrap.md)|指定用于启动安全对话服务的默认值。|  
  
## <a name="remarks"></a>备注  

 这些设置不是从服务的安全策略派生而来的，从这个意义上说，它们是本地的。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.LocalClientSecuritySettingsElement>
- <xref:System.ServiceModel.Configuration.SecurityElementBase.LocalClientSettings%2A>
- <xref:System.ServiceModel.Channels.SecurityBindingElement.LocalClientSettings%2A>
- <xref:System.ServiceModel.Channels.LocalClientSecuritySettings>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [绑定](../../../wcf/bindings.md)
- [扩展绑定](../../../wcf/extending/extending-bindings.md)
- [自定义绑定](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
- [如何：使用 SecurityBindingElement 创建自定义绑定](../../../wcf/feature-details/how-to-create-a-custom-binding-using-the-securitybindingelement.md)
- [自定义绑定安全性](../../../wcf/samples/custom-binding-security.md)
