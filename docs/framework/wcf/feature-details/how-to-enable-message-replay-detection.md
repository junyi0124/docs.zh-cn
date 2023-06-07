---
title: 如何：启用消息重放检测
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF security
- replay detection [WCF]
- WCF, custom bindings
- WCF, security
ms.assetid: 8b847e91-69a3-49e1-9e5f-0c455e50d804
ms.openlocfilehash: 4a4d304a1316fe534e09f02ac1cd2900bf798011
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96265526"
---
# <a name="how-to-enable-message-replay-detection"></a>如何：启用消息重放检测

当攻击者复制双方之间的消息流并将该消息流向一方或多方重播时，将发生重播攻击。 除非攻击程度降低，否则受到攻击的计算机会将该流处理为合法消息，从而导致产生大量不良结果，例如某项的冗余排序。  
  
 有关消息重播检测的详细信息，请参阅 [消息重播检测](/previous-versions/msp-n-p/ff649371(v=pandp.10))。  
  
 下面的过程演示了各种属性，这些属性可用于使用 Windows Communication Foundation (WCF) 控制重播检测。  
  
### <a name="to-control-replay-detection-on-the-client-using-code"></a>使用代码在客户端上控制重播检测  
  
1. 创建要在 <xref:System.ServiceModel.Channels.SecurityBindingElement> 中使用的 <xref:System.ServiceModel.Channels.CustomBinding>。 有关详细信息，请参阅 [如何：使用 SecurityBindingElement 创建自定义绑定](how-to-create-a-custom-binding-using-the-securitybindingelement.md)。 下面的示例使用通过 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 类的 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateKerberosBindingElement%2A> 创建的 <xref:System.ServiceModel.Channels.SecurityBindingElement>。  
  
2. 使用 <xref:System.ServiceModel.Channels.SecurityBindingElement.LocalClientSettings%2A> 属性返回对 <xref:System.ServiceModel.Channels.LocalClientSecuritySettings> 类的引用，并根据需要设置任何下列属性：  
  
    1. `DetectReplay`. 一个布尔值。 该值控制客户端是否应当检测来自服务器的重播。 默认值为 `true`。  
  
    2. `MaxClockSkew`. 一个 <xref:System.TimeSpan> 值。 控制重播机制在客户端和服务器之间可以容忍多大程度的时间偏差。 该安全机制检查发送的时间戳，并确定在过去它是否返回过快。 默认为 5 分钟。  
  
    3. `ReplayWindow`. 一个 `TimeSpan` 值。 该值控制某消息在由服务器（通过中间方）发送之后、到达客户端之前在网络中存留的时间。 客户端跟踪在最后一个 `ReplayWindow` 中发送的消息的签名，以进行重播检测。  
  
    4. `ReplayCacheSize`. 一个整数值。 客户端在缓存中存储消息的签名。 此设置指定缓存中可以存储的签名数目。 如果最后一个重播窗口中发送的消息数达到缓存限制，则拒绝新消息，直到最旧的缓存签名达到时间限制为止。 默认值为500000。  
  
### <a name="to-control-replay-detection-on-the-service-using-code"></a>使用代码在服务上控制重播检测  
  
1. 创建要在 <xref:System.ServiceModel.Channels.SecurityBindingElement> 中使用的 <xref:System.ServiceModel.Channels.CustomBinding>。  
  
2. 使用 <xref:System.ServiceModel.Channels.SecurityBindingElement.LocalServiceSettings%2A> 属性返回对 <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings> 类的引用，并按照上面所述设置这些属性。  
  
### <a name="to-control-replay-detection-in-configuration-for-the-client-or-service"></a>在客户端或服务的配置中控制重播检测  
  
1. 创建一个 [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) 。  
  
2. 创建 `<security>` 元素。  
  
3. 创建 [\<localClientSettings>](../../configure-apps/file-schema/wcf/localclientsettings-element.md) 或 [\<localServiceSettings>](../../configure-apps/file-schema/wcf/localservicesettings-element.md) 。  
  
4. 根据需要，设置下列属性值：`detectReplays`、`maxClockSkew`、`replayWindow` 和 `replayCacheSize`。 下面的示例设置 `<localServiceSettings>` 元素的属性：  
  
    ```xml  
    <customBinding>  
      <binding name="NewBinding0">  
       <textMessageEncoding />  
        <security>  
         <localClientSettings
          replayCacheSize="800000"
          maxClockSkew="00:03:00"  
          replayWindow="00:03:00" />  
         <localServiceSettings
          replayCacheSize="800000"
          maxClockSkew="00:03:00"  
          replayWindow="00:03:00" />  
        <secureConversationBootstrap />  
       </security>  
      <httpTransport />  
     </binding>  
    </customBinding>  
    ```  
  
## <a name="example"></a>示例  

 下面的示例使用 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 方法创建 <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateKerberosBindingElement%2A>，并设置绑定的重播属性。  
  
 [!code-csharp[c_ReplayDetection#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_replaydetection/cs/source.cs#1)]
 [!code-vb[c_ReplayDetection#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_replaydetection/vb/source.vb#1)]  
  
## <a name="scope-of-replay-message-security-only"></a>重播范围：仅限消息安全  

 请注意，下面的过程仅适用于“消息安全”模式。 对于“传输”和“使用消息凭据传输”模式，传输机制将检测重播。  
  
## <a name="secure-conversation-notes"></a>安全对话说明  

 对于启用安全对话的绑定，可以针对应用程序通道和安全对话引导绑定来调整这些设置。 例如，可以对应用程序通道关闭重播，但是对建立安全对话的引导通道启用重播。  
  
 如果不使用安全对话会话，则重播检测不保证在服务器场方案中和回收进程时检测重播。 这适用于系统提供的下列绑定：  
  
- <xref:System.ServiceModel.BasicHttpBinding>.  
  
- <xref:System.ServiceModel.WSHttpBinding>，其 <xref:System.ServiceModel.NonDualMessageSecurityOverHttp.EstablishSecurityContext%2A> 属性设置为 `false`。  
  
## <a name="compiling-the-code"></a>编译代码  
  
- 编译该代码需要以下命名空间：  
  
- <xref:System>  
  
- <xref:System.ServiceModel>  
  
- <xref:System.ServiceModel.Channels>  
  
## <a name="see-also"></a>另请参阅

- <xref:System.ServiceModel.Channels.LocalClientSecuritySettings>
- <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings>
- [安全对话和安全会话](secure-conversations-and-secure-sessions.md)
- [\<localClientSettings>](../../configure-apps/file-schema/wcf/localclientsettings-element.md)
- [如何：使用 SecurityBindingElement 创建自定义绑定](how-to-create-a-custom-binding-using-the-securitybindingelement.md)
