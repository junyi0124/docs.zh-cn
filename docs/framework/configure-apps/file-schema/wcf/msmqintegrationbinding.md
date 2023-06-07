---
title: <msmqIntegrationBinding>
ms.date: 03/30/2017
helpviewer_keywords:
- msmqIntegrationBinding Element
ms.assetid: edf277f3-e3bf-4ed8-9f55-83b5788430a7
ms.openlocfilehash: bc2b1648ad404ba13920d9f276c299756554b5d4
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204666"
---
# \<msmqIntegrationBinding>

定义一个绑定，此绑定通过利用 MSMQ 路由消息来提供队列支持。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<msmqIntegrationBinding>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<msmqIntegrationBinding>
  <binding closeTimeout="TimeSpan"
           customDeadLetterQueue="Uri"
           deadLetterQueue="Uri"
           durable="Boolean"
           exactlyOnce="Boolean"
           maxReceivedMessageSize="Integer"
           maxRetryCycles="Integer"
           name="String"
           openTimeout="TimeSpan"
           receiveContextEnabled="Boolean"
           receiveErrorHandling="Drop/Fault/Move/Reject"
           receiveTimeout="TimeSpan"
           receiveRetryCount="Integer"
           retryCycleDelay="TimeSpan"
           sendTimeout="TimeSpan"
           serializationFormat="XML/Binary/ActiveX/ByteArray/Stream"
           timeToLive="TimeSpan"
           useMsmqTracing="Boolean"
           useSourceJournal="Boolean">
  </binding>
</msmqIntegrationBinding>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 以下几节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|closeTimeout|一个 <xref:System.TimeSpan> 值，指定为完成关闭操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|customDeadLetterQueue|一个 URI，包含每个应用程序的死信队列（该队列用于放置已过期的消息或者放置传输或传递失败的消息）的位置。<br /><br /> 死信队列是发送应用程序的队列管理器中的一个队列，用于放置传递失败的过期消息。<br /><br /> <xref:System.ServiceModel.MsmqBindingBase.CustomDeadLetterQueue%2A> 指定的 URI 必须使用 net.msmq 方案。|  
|deadLetterQueue|一个 <xref:System.ServiceModel.MsmqBindingBase.DeadLetterQueue%2A> 值，指定要使用的死信队列（如果有）的类型。<br /><br /> 死信队列是无法传递到应用程序的消息所要传输到的位置。<br /><br /> 对于需要 exactlyOnce 保证（即，`exactlyOnce` 属性设置为 `true`）的消息，此属性默认为 MSMQ 中系统级事务性死信队列。<br /><br /> 对于不需要保证的消息，此属性默认为 `null`。|  
|durable|一个布尔值，指示消息在队列中是持久的还是可变的。 持久消息能够在队列管理器崩溃后保留下来，而可变消息则不能。 当应用程序需要较低的延迟并且可以容忍偶尔丢失消息时，可变消息是有用的。 如果 `exactlyOnce` 属性设置为 `true`，则消息必须为持久的消息。 默认为 `true`。|  
|exactlyOnce|一个布尔值，指示每个消息是否只传递一次。 然后，将通知发送方有关传递失败的信息。 如果 `durable` 为 `false`，则将忽略此属性并且传输消息，而不会提供传递保证。 默认值为 `true`。 有关详细信息，请参阅 <xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A>。|  
|maxReceivedMessageSize|一个正整数，定义此绑定所处理的最大消息大小（以字节为单位），其中包括标头。 如果消息超出此限制，则发送方将收到 SOAP 错误。 接收方将删除该消息，并在跟踪日志中创建事件项。 默认值为 65536。 对消息大小进行的此限制旨在降低遭受拒绝服务 (DoS) 攻击的可能性。|  
|maxRetryCycles|一个整数，指示病毒消息检测功能所使用的重试周期数。 如果所有周期的所有传递尝试均失败，则消息将变为病毒消息。 默认值为 2。 有关详细信息，请参阅 <xref:System.ServiceModel.MsmqBindingBase.MaxRetryCycles%2A>。|  
|name|一个包含绑定的配置名称的字符串。 因为此值用作绑定的标识，所以它应该是唯一的。 从 .NET Framework 4 开始，绑定和行为不需要具有名称。 有关默认配置和无值绑定和行为的详细信息，请参阅[WCF 服务的](../../../wcf/samples/simplified-configuration-for-wcf-services.md)[简化配置](../../../wcf/simplified-configuration.md)和简化配置。|  
|openTimeout|一个 <xref:System.TimeSpan> 值，指定为完成打开操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|receiveErrorHandling|一个 <xref:System.ServiceModel.ReceiveErrorHandling> 值，指定如何处理病毒消息和不可调度的消息。|  
|receiveRetryCount|一个整数，指定在从应用程序队列到应用程序的消息传输失败时，队列管理器应尝试的最大立即重试次数。<br /><br /> 如果达到尝试传递的最大次数且应用程序仍未访问消息，则会将消息发送到重试队列，以便在以后重新进行传递。 将消息传输回发送队列之前的时间量由 `retryCycleDelay` 控制。 如果重试周期达到 `maxRetryCycles` 值，则或者将消息发送到病毒消息队列，或者将否定确认发送回发送方。|  
|receiveTimeout|一个 <xref:System.TimeSpan> 值，指定为完成接收操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:10:00。|  
|receiveContextEnabled|一个布尔值，指定是否启用接收上下文来处理队列中的消息。 当此设置为时 `true` ，服务可以 "扫视" 队列中的消息以开始处理它，如果发生任何错误并且引发异常，则该消息将保留在队列中。 服务还可 "锁定" 消息以便在稍后的某个时间点重试处理。 ReceiveContext 提供了一种机制，用于在处理后 "完成" 消息，以便可以从队列中删除该消息。消息将不再被读取并重新写入到网络上的队列中，并且在处理过程中各个消息不会在不同的服务实例之间切换。|  
|retryCycleDelay|一个 TimeSpan 值，指定尝试传递无法立即传递的消息时，各个重试周期之间的时间延迟。 该值只定义最小等待时间，因为实际等待时间可能较长。 默认值为 00:30:00。 有关详细信息，请参阅 <xref:System.ServiceModel.MsmqBindingBase.RetryCycleDelay%2A>。|  
|sendTimeout|一个 <xref:System.TimeSpan> 值，指定为完成发送操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|serializationFormat|定义用于消息正文序列化的格式。 此属性的类型为 <xref:System.ServiceModel.MsmqIntegration.MsmqMessageSerializationFormat>。|  
|timeToLive|一个 TimeSpan 值，指定在消息过期并放入死信队列之前消息保持有效的持续时间。 默认值为 1.00:00:00。<br /><br /> 设置此属性可以确保具有时效性的消息不会在由接收应用程序进行处理之前过时。 如果队列中的消息在指定时间间隔内未被接收应用程序进行处理，则称该消息为过时消息。 过时消息被发送到称为“死信队列”的特殊队列中。 死信队列的位置通过 `DeadLetterQueue` 属性进行设置，或基于保证设置为适当的默认值。|  
|useMsmqTracing|一个布尔值，指定是否应跟踪由此绑定处理的消息。 默认为 `false`。 如果启用了跟踪，则每当消息离开或到达消息队列计算机时，都会创建报告消息并将其发送到报告队列。|  
|useSourceJournal|一个布尔值，指定是否应将由此绑定处理的消息的副本存储在源日志中。 默认为 `false`。<br /><br /> 如果排队应用程序要保留已离开计算机传出队列的消息的记录，则可以将这些消息复制到日志队列。 在消息离开传出队列，并且接收到目标计算机已接收该消息的确认后，该消息的副本就会保留在发送计算机的系统日志队列中。|  
  
## <a name="serializationformat-attribute"></a>{serializationFormat} 属性  
  
|值|描述|  
|-----------|-----------------|  
|Xml|XML 格式|  
|二进制|二进制格式|  
|ActiveX|ActiveX 格式|  
|ByteArray|将对象序列化为字节数组。|  
|Stream|设置为流格式的正文|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<security>](security-of-msmqintegrationbinding.md)|定义绑定的安全设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.MsmqIntegrationSecurityElement>。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<bindings>](bindings.md)|此元素包含标准绑定和自定义绑定的集合。|  
  
## <a name="remarks"></a>备注  

 此绑定元素可用于启用 Windows Communication Foundation (WCF) 应用程序向使用 COM、MSMQ 本机 Api 或在命名空间中定义的类型的现有 MSMQ 应用程序发送消息和从中接收消息 <xref:System.Messaging?displayProperty=nameWithType> 。可以使用此配置元素指定对队列进行寻址的方式、传输保证、是否必须持久存储消息以及应如何对消息进行保护和身份验证。 有关详细信息，请参阅 [如何：与 WCF 终结点和消息队列应用程序交换消息](../../../wcf/feature-details/how-to-exchange-messages-with-wcf-endpoints-and-message-queuing-applications.md)。  
  
## <a name="example"></a>示例  
  
```xml  
<configuration>
  <system.ServiceModel>
    <bindings>
      <msmqIntegrationBinding>
        <binding closeTimeout="00:00:10"
                 openTimeout="00:00:20"
                 receiveTimeout="00:00:30"
                 sendTimeout="00:00:40"
                 deadLetterQueue="net.msmq://localhost/blah"
                 durable="true"
                 exactlyOnce="true"
                 maxReceivedMessageSize="1000"
                 maxImmediateRetries="11"
                 maxRetryCycles="12"
                 poisonMessageHandling="Disabled"
                 rejectAfterLastRetry="false"
                 retryCycleDelay="00:05:55"
                 timeToLive="00:11:11"
                 useSourceJournal="true"
                 useMsmqTracing="true"
                 serializationFormat="Binary">
          <security mode="None" />
        </binding>
      </msmqIntegrationBinding>
    </bindings>
  </system.ServiceModel>
</configuration>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.MsmqIntegrationBindingElement>
- <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding>
- <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBindingElement>
- [\<binding>](bindings.md)
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [WCF 中的队列](../../../wcf/feature-details/queues-in-wcf.md)
