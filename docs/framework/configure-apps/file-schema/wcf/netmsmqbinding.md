---
title: <netMsmqBinding>
ms.date: 03/30/2017
ms.assetid: a68b44d7-7799-43a3-9e63-f07c782810a6
ms.openlocfilehash: 7dca85e4095e581c262611f0120ffb85e2174fee
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204575"
---
# \<netMsmqBinding>

定义适用于跨计算机通信的排队绑定。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<netMsmqBinding>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<netMsmqBinding>
  <binding closeTimeout="TimeSpan"
           customDeadLetterQueue="Uri"
           deadLetterQueue="Uri"
           durable="Boolean"
           exactlyOnce="Boolean"
           maxBufferPoolSize="Integer"
           maxReceivedMessageSize="Integer"
           maxRetryCycles="Integer"
           name="String"
           openTimeout="TimeSpan"
           poisonMessageHandling="Disabled/EnabledIfSupported"
           queueTransferProtocol="Native/Srmp/SrmpSecure"
           receiveErrorHandling="Drop/Fault/Move/Reject"
           receiveTimeout="TimeSpan"
           receiveRetryCount="Integer"
           rejectAfterLastRetry="Boolean"
           retryCycleDelay="TimeSpan"
           sendTimeout="TimeSpan"
           timeToLive="TimeSpan"
           useActiveDirectory="Boolean"
           useMsmqTracing="Boolean"
           useSourceJournal="Boolean">
    <security>
      <message algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
               clientCredentialType="None/Windows/UserName/Certificate/InfoCard" />
      <transport msmqAuthenticationMode="None/WindowsDomain/Certificate"
                 msmqEncryptionAlgorithm="RC4Stream/AES"
                 msmqProtectionLevel="None/Sign/EncryptAndSign"
                 msmqSecureHashAlgorithm="MD5/SHA1/SHA256/SHA512" />
    </security>
    <readerQuotas maxArrayLength="Integer"
                  maxBytesPerRead="Integer"
                  maxDepth="Integer"
                  maxNameTableCharCount="Integer"
                  maxStringContentLength="Integer" />
  </binding>
</netMsmqBinding>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`closeTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成关闭操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|`customDeadLetterQueue`|一个 URI，包含每个应用程序的死信队列（该队列用于放置已过期的消息或者放置传输或传递失败的消息）的位置。<br /><br /> 死信队列是发送应用程序的队列管理器中的一个队列，用于放置传递失败的过期消息。<br /><br /> <xref:System.ServiceModel.MsmqBindingBase.CustomDeadLetterQueue%2A> 指定的 URI 必须使用 net.msmq 方案。|  
|`deadLetterQueue`|一个 <xref:System.ServiceModel.MsmqBindingBase.DeadLetterQueue%2A> 值，该值指定所用死信队列（如果有）的类型。<br /><br /> 死信队列是无法传递到应用程序的消息所要传输到的位置。<br /><br /> 对于需要 `exactlyOnce` 保证（即，`exactlyOnce` 属性设置为 `true`）的消息，此属性默认为 MSMQ 中系统级事务性死信队列。<br /><br /> 对于不需要保证的消息，此属性默认为 `null`。|  
|`durable`|一个布尔值，指示消息在队列中是持久的还是可变的。 持久消息能够在队列管理器崩溃后保留下来，而可变消息则不能。 当应用程序需要较低的延迟并且可以容忍偶尔丢失消息时，可变消息是有用的。 如果 `exactlyOnce` 属性设置为 `true`，则消息必须为持久的消息。 默认为 `true`。|  
|`exactlyOnce`|一个布尔值，指示是否只传递一次此绑定所处理的每个消息。 然后，将通知发送方有关传递失败的信息。 如果 `durable` 为 `false`，则将忽略此属性并且传输消息，而不会提供传递保证。 默认值为 `true`。 有关详细信息，请参阅 <xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A>。|  
|`maxBufferPoolSize`|一个整数，指定此绑定的最大缓冲池大小。 默认值为 8。|  
|`maxReceivedMessageSize`|一个正整数，定义此绑定所处理的最大消息大小（以字节为单位），其中包括标头。 如果消息超出此限制，则发送方将收到 SOAP 错误。 接收方将删除该消息，并在跟踪日志中创建事件项。 默认值为 65536。 对消息大小进行的此限制旨在降低遭受拒绝服务 (DoS) 攻击的可能性。|  
|`maxRetryCycles`|一个整数，指示病毒消息检测功能所使用的重试周期数。 如果所有周期的所有传递尝试均失败，则消息将变为病毒消息。 默认值为 3。 有关详细信息，请参阅 <xref:System.ServiceModel.MsmqBindingBase.MaxRetryCycles%2A>。|  
|`name`|必需的特性。 一个包含绑定的配置名称的字符串。 因为此值用作绑定的标识，所以它应该是唯一的。 从 .NET Framework 4 开始，绑定和行为不需要具有名称。 有关默认配置和无值绑定和行为的详细信息，请参阅[WCF 服务的](../../../wcf/samples/simplified-configuration-for-wcf-services.md)[简化配置](../../../wcf/simplified-configuration.md)和简化配置。|  
|`openTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成打开操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|`QueueTransferProtocol`|一个有效的 <xref:System.ServiceModel.QueueTransferProtocol> 值，指定此绑定所使用的排队通信通道传输。 在使用 SOAP 可靠消息协议时，MSMQ 不支持 Active Directory 寻址。 因此， `Srmp` `Srmps` 当 `useActiveDirectory` 特性设置为时，不应将此特性设置为或 `true` 。|  
|`receiveErrorHandling`|一个 <xref:System.ServiceModel.ReceiveErrorHandling> 值，指定如何处理病毒消息和不可调度的消息。|  
|`receiveRetryCount`|一个整数，指定队列管理器在将消息传输到重试队列前可尝试发送该消息的最大次数。|  
|`receiveTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成接收操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:10:00。|  
|`retryCycleDelay`|一个 TimeSpan 值，指定尝试传递无法立即传递的消息时，各个重试周期之间的时间延迟。 该值只定义最小等待时间，因为实际等待时间可能较长。 默认值为 00:10:00。 有关详细信息，请参阅 <xref:System.ServiceModel.MsmqBindingBase.RetryCycleDelay%2A>。|  
|`sendTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成发送操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|`timeToLive`|一个 TimeSpan 值，指定在消息过期并放入死信队列之前消息保持有效的持续时间。 默认值为 1.00:00:00。<br /><br /> 设置此属性可以确保具有时效性的消息不会在由接收应用程序进行处理之前过时。 如果队列中的消息在指定时间间隔内未被接收应用程序进行处理，则称该消息为过时消息。 过时消息被发送到称为“死信队列”的特殊队列中。 死信队列的位置通过 `DeadLetterQueue` 属性进行设置，或基于保证设置为适当的默认值。|  
|`usingActiveDirectory`|一个布尔值，指定是否应该使用 Active Directory 转换队列地址。<br /><br /> MSMQ 队列地址可由路径名或直接格式名组成。 对于直接格式名，MSMQ 会使用 DNS、NetBIOS 或 IP 解析计算机名称。 对于路径名，MSMQ 会使用 Active Directory 解析计算机名称。<br /><br /> 默认情况下，Windows Communication Foundation (WCF) 排队传输会将消息队列的 URI 转换为直接格式名。 通过将 `UseActiveDirectory` 属性设置为 True，应用程序可以指定排队传输应使用 Active Directory 而不是 DNS、NetBIOS 或 IP 来解析计算机名称。|  
|`useMsmqTracing`|一个布尔值，指定是否应跟踪由此绑定处理的消息。 默认为 `false`。 如果启用了跟踪，则每当消息离开或到达消息队列计算机时，都会创建报告消息并将其发送到报告队列。|  
|`useSourceJournal`|一个布尔值，指定是否应将由此绑定处理的消息的副本存储在源日志中。 默认为 `false`。<br /><br /> 如果排队应用程序要保留已离开计算机传出队列的消息的记录，则可以将这些消息复制到日志队列。 在消息离开传出队列，并且接收到目标计算机已接收该消息的确认后，该消息的副本就会保留在发送计算机的系统日志队列中。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<readerQuotas>](/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|定义可由采用此绑定配置的终结点进行处理的 SOAP 消息的复杂性约束。 此元素的类型为 <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement>。|  
|[\<security>](security-of-netmsmqbinding.md)|定义绑定的安全设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.NetMsmqSecurityElement>。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<bindings>](bindings.md)|此元素包含标准绑定和自定义绑定的集合。|  
  
## <a name="remarks"></a>备注  

 `netMsmqBinding` 绑定通过利用 Microsoft 消息队列 (MSMQ) 作为传输来提供队列支持，并且为松耦合应用程序、故障隔离、负载均衡和断开连接的操作提供支持。 有关这些功能的讨论，请参阅 [WCF 中的队列](../../../wcf/feature-details/queues-in-wcf.md)。  
  
## <a name="example"></a>示例  
  
```xml  
<configuration>
  <system.ServiceModel>
    <bindings>
      <netMsmqBinding>
        <binding closeTimeout="00:00:10"
                 openTimeout="00:00:20"
                 receiveTimeout="00:00:30"
                 sendTimeout="00:00:40"
                 deadLetterQueue="net.msmq://localhost/blah"
                 durable="true"
                 exactlyOnce="true"
                 maxReceivedMessageSize="1000"
                 maxRetries="11"
                 maxRetryCycles="12"
                 poisonMessageHandling="Disabled"
                 rejectAfterLastRetry="false"
                 retryCycleDelay="00:05:55"
                 timeToLive="00:11:11"
                 sourceJournal="true"
                 useMsmqTracing="true"
                 useActiveDirectory="true">
          <security>
            <message clientCredentialType="Windows" />
          </security>
        </binding>
      </netMsmqBinding>
    </bindings>
  </system.ServiceModel>
</configuration>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.NetMsmqBinding>
- <xref:System.ServiceModel.Configuration.NetMsmqBindingElement>
- [\<binding>](bindings.md)
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [WCF 中的队列](../../../wcf/feature-details/queues-in-wcf.md)
