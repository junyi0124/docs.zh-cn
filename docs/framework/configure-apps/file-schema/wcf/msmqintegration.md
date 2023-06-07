---
title: <msmqIntegration>
ms.date: 03/30/2017
ms.assetid: ab677405-1ffe-457a-803f-00c1770e51e2
ms.openlocfilehash: 66f7e3ba145441926ed11227a0ba82ff21925cf7
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204679"
---
# \<msmqIntegration>

指定自定义绑定的 MSMQ 传输。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<msmqIntegration>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<msmqIntegration customDeadLetterQueue="Uri"
                 deadLetterQueue="Custom/None/System"
                 durable="Boolean"
                 exactlyOnce="Boolean"
                 manualAddressing="Boolean"
                 maxBufferPoolSize="Integer"
                 maxImmediateRetries="Integer"
                 maxReceivedMessageSize="Integer"
                 maxRetryCycles="Integer"
                 rejectAfterLastRetry="Boolean"
                 retryCycleDelay="TimeSpan"
                 serializationFormat="XML/Binary/ActiveX/ByteArray/Stream"
                 timeToLive="TimeSpan"
                 useSourceJournal="Boolean"
                 useMsmqTracing="Boolean">
  <msmqTransportSecurity>
  </msmqTransportSecurity>
</msmqIntegration>
```  
  
## <a name="type"></a>类型  

 `Type`  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|customDeadLetterQueue|一个 URI，指示每个应用程序的死信队列（该队列用于传输已过期的或无法传递到应用程序的消息）的位置。<br /><br /> 对于需要 ExactlyOnce 保证的消息（即 `exactlyOnce` 设置为 `true`），此属性默认为 MSMQ 中系统级事务性死信队列。<br /><br /> 对于不需要保证的消息（即 `exactlyOnce` 设置为 `false`），此属性默认为 `null`。<br /><br /> 该值必须使用 net.msmq 方案。 默认为 `null`。<br /><br /> 如果 `deadLetterQueue` 设置为 `None` 或 `System`，则此属性必须设置为 `null`。 如果此属性不为 `null`，则 `deadLetterQueue` 必须设置为 `Custom`。|  
|deadLetterQueue|指定要使用的死信队列类型。<br /><br /> 有效值包括<br /><br /> -Custom：自定义死信队列。<br />-None：不使用死信队列。<br />-System：使用 system 死信队列。<br /><br /> 此属性的类型为 DeadLetterQueue。|  
|durable|一个布尔值，指定此绑定处理的消息是持久的还是可变的。 默认值为 `true`。<br /><br /> 持久消息能够在队列管理器崩溃后保留下来，而可变消息则不能。 当应用程序需要较低的延迟并且可以容忍偶尔丢失消息时，可变消息是有用的。<br /><br /> 如果 `exactlyOnce` 设置为 `true`，则消息必须为持久的。|  
|exactlyOnce|一个布尔值，指定是否将只接收一次此绑定处理的消息。 默认值为 `true`。<br /><br /> 发送的消息可以包含保证，也可以不包含保证。 应用程序可以使用保证来确保发送的消息到达接收消息队列；如果消息未能到达，则应用程序可以通过读取死信队列进行确定。<br /><br /> 当 `exactlyOnce` 设置为 `true` 时，指示 MSMQ 应确保将发送的消息传递到接收消息队列一次且只有一次。如果传递失败，则会将消息发送到死信队列。<br /><br /> 将 `exactlyOnce` 设置为 `true` 时发送的消息必须只发送到事务性队列。|  
|manualAddressing|一个使用户能够控制消息寻址的布尔值。 此属性通常用于路由器方案。在该方案中，应用程序确定将消息发送到若干目标中的哪一个。<br /><br /> 如果设置为 `true`，则通道假定已对消息进行寻址，而不再向其添加其他任何信息。 然后，用户可以单独对每个消息进行寻址。<br /><br /> 如果设置为 `false`，则默认的 Windows Communication Foundation (WCF) 寻址机制将为所有消息自动创建地址。<br /><br /> 默认为 `false`。|  
|maxBufferPoolSize|一个正整数，指定缓冲池的最大大小。 默认值为 524288。<br /><br /> WCF 的许多组件使用缓冲区。 每次使用缓冲区时，创建和销毁它们都将占用大量资源，而缓冲区的垃圾回收过程也是如此。 利用缓冲池，可以从缓冲池中获得缓冲区，使用缓冲区，然后在完成工作后将其返回给缓冲池。 这样就避免了创建和销毁缓冲区的系统开销。|  
|maxImmediateRetries|一个整数，指定从应用程序队列读取的消息的最大立即重试次数。 默认值为 5。<br /><br /> 如果对消息进行了最大立即重试次数的尝试且应用程序未处理消息，则会将消息发送到重试队列，以便在将来某个时刻进行重试。 如果未指定重试周期，则将消息发送到病毒消息队列，或将否定确认发送回发送方。|  
|maxReceivedMessageSize|一个正整数，指定最大消息大小（以字节为单位），其中包括标头。 如果消息对于接收方而言太大，则消息发送方将收到 SOAP 错误。 接收方将删除该消息，并在跟踪日志中创建事件项。 默认值为 65536。|  
|maxRetryCycles|一个整数，指定尝试向接收应用程序传递消息的最大重试周期数。 默认为 <xref:System.Int32.MaxValue>。<br /><br /> 单个重试周期按指定次数尝试向应用程序传递消息。 尝试次数由 `maxImmediateRetries` 属性设置。 如果在耗尽传递尝试次数后，应用程序仍未能处理消息，则将消息发送到重试队列。 后续的重试周期包括在经过由 `retryCycleDelay`属性指定的延迟后，从重试队列返回应用程序队列以再次尝试传递给接收应用程序的消息。 `maxRetryCycles` 属性指定应用程序用于尝试传递消息的重试周期数。|  
|rejectAfterLastRetry|一个布尔值，指定对已尝试过最大重试次数后仍无法传递的消息所采取的操作。<br /><br /> `true` 表示将否定确认返回到发送方且丢弃消息；`false` 表示将消息发送到病毒消息队列。 默认为 `false`。<br /><br /> 如果值为 `false`，则接收应用程序可以读取病毒消息队列以处理病毒消息（即传递失败的消息）。<br /><br /> MSMQ 3.0 不支持向发送方返回否定确认，因此该属性在 MSMQ 3.0 中将被忽略。|  
|retryCycleDelay|一个 <xref:System.TimeSpan>，指定尝试传递无法立即传递的消息时各个重试周期之间的时间延迟。 默认值为 00:10:00。<br /><br /> 单个重试周期尝试按指定次数向接收应用程序传递消息。 尝试次数由 `maxImmediateRetries` 属性指定。 如果在连续重试了指定次数后，应用程序仍未能处理消息，则将消息发送到重试队列。 后续的重试周期包括在经过由 `retryCycleDelay`属性指定的延迟后，从重试队列返回应用程序队列以再次尝试传递给接收应用程序的消息。 重试周期数由 `maxRetryCycles` 属性指定。|  
|serializationFormat|指定用于序列化作为 MSMQ 消息的一部分发送的对象的格式化程序。 有效值为<br /><br /> -ActiveX：在序列化 COM 对象时使用 ActiveX 格式化程序。<br />-Binary：将对象序列化为二进制数据包。<br />-ByteArray：将对象序列化为字节数组。<br />-Stream：将对象序列化为流。<br />-Xml：将对象序列化为 XML 数据包。 默认值为 XML。<br /><br /> 此属性的类型为 <xref:System.ServiceModel.MsmqIntegration.MsmqMessageSerializationFormat>。|  
|timeToLive|一个 <xref:System.TimeSpan>，指定消息在过期并放入死信队列之前保持有效的持续时间。 默认值为 1.00:00:00，表示 1 天。<br /><br /> 设置此属性可以确保具有时效性的消息不会在由接收应用程序进行处理之前过时。 如果队列中的消息在指定时间间隔内未被接收应用程序进行处理，则称该消息为过时消息。 过时消息被发送到称为“死信队列”的特殊队列中。 死信队列的位置通过 `customDeadLetterQueue` 属性进行设置，或基于保证设置为适当的默认值。|  
|useMsmqTracing|一个布尔值，指定是否应跟踪由此绑定处理的消息。 默认为 `false`。<br /><br /> 如果启用了跟踪，则每当消息离开或到达消息队列计算机时，都会创建报告消息并将其发送到报告队列。|  
|useSourceJournal|一个布尔值，指定此绑定所处理的消息的副本是否应存储在源日志队列中。 默认为 `false`。<br /><br /> 如果排队应用程序要保留已离开计算机传出队列的消息的记录，则可以将这些消息复制到日志队列。 在消息离开传出队列，并且接收到目标计算机已接收该消息的确认后，该消息的副本就会保留在发送计算机的系统日志队列中。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|msmqTransportSecurity|指定此绑定的传输安全设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.MsmqTransportSecurityElement>。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<binding>](bindings.md)|定义自定义绑定的所有绑定功能。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.MsmqIntegrationElement>
- <xref:System.ServiceModel.Channels.TransportBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [传输](../../../wcf/feature-details/transports.md)
- [WCF 中的队列](../../../wcf/feature-details/queues-in-wcf.md)
- [选择传输方式](../../../wcf/feature-details/choosing-a-transport.md)
- [绑定](../../../wcf/bindings.md)
- [扩展绑定](../../../wcf/extending/extending-bindings.md)
- [自定义绑定](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
