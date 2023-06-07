---
title: <netTcpBinding>
description: 表示一个安全、可靠且经过优化的绑定，仅用于使用 TCP 的 WCF 跨计算机通信。 默认情况下，可靠消息传递处于关闭状态。
ms.date: 03/30/2017
helpviewer_keywords:
- netTcpBinding Element
ms.assetid: 5c5104a7-8754-4335-8233-46a45322503e
ms.openlocfilehash: a366b26d87c8796822e4fbbb2001823c116f2629
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90556123"
---
# \<netTcpBinding>

指定一种适合于跨计算机通信的安全、可靠且进行了优化的绑定。 默认情况下，它会生成一个运行时通信堆栈，该堆栈包含 Windows Security 以保证消息安全性和进行身份验证，包含 TCP 以便进行消息传递，并且包含二进制消息编码机制。

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<netTcpBinding>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<netTcpBinding>
  <binding closeTimeout="TimeSpan"
           hostNameComparisonMode="StrongWildCard/Exact/WeakWildcard"
           listenBacklog="Integer"
           maxBufferPoolSize="integer"
           maxBufferSize="Integer"
           maxConnections="Integer"
           maxReceivedMessageSize="Integer"
           name="string"
           openTimeout="TimeSpan"
           portSharingEnabled="Boolean"
           receiveTimeout="TimeSpan"
           sendTimeout="TimeSpan"
           transactionFlow="Boolean"
           transactionProtocol="OleTransactions/WSAtomicTransactionOctober2004"
           transferMode="Buffered/Streamed/StreamedRequest/StreamedResponse">
    <reliableSession ordered="Boolean"
                     inactivityTimeout="TimeSpan"
                     enabled="Boolean" />
    <security mode="None/Transport/Message/Both">
      <message clientCredentialType="None/Windows/UserName/Certificate/CardSpace"
               algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15" />
      <transport clientCredentialType="None/Windows/Certificate"
                 protectionLevel="None/Sign/EncryptAndSign" />
    </security>
    <readerQuotas maxArrayLength="Integer"
                  maxBytesPerRead="Integer"
                  maxDepth="Integer"
                  maxNameTableCharCount="Integer"
                  maxStringContentLength="Integer" />
  </binding>
</netTcpBinding>
```  
  
## <a name="attributes-and-elements"></a>特性和元素

下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|说明|  
|---------------|-----------------|  
|`closeTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成关闭操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|`hostNameComparisonMode`|指定用于分析 URI 的 HTTP 主机名比较模式。 此属性的类型为 <xref:System.ServiceModel.HostNameComparisonMode>，指示在对 URI 进行匹配时，是否使用主机名来访问服务。 默认值为 <xref:System.ServiceModel.HostNameComparisonMode.StrongWildcard>，表示忽略匹配项中的主机名。|  
|`listenBacklog`|一个正整数，指定侦听器上等待接受的最大通道数。 超出此限制的连接会被排队，直到连接数低于限制值。 `connectionTimeout` 属性限制客户端在引发连接异常之前将等待连接的时间。 默认值为 10。|  
|`maxBufferPoolSize`|一个整数，指定此绑定的最大缓冲池大小。 默认值为 512 * 1024 字节。 Windows Communication Foundation (WCF) 的许多部件使用缓冲区。 每次使用缓冲区时，创建和销毁它们都将占用大量资源，而缓冲区的垃圾回收过程也是如此。 利用缓冲池，可以从缓冲池中获得缓冲区，使用缓冲区，然后在完成工作后将其返回给缓冲池。 这样就避免了创建和销毁缓冲区的系统开销。|  
|`maxBufferSize`|一个正整数，指定内存中用于存储消息的缓冲区的最大大小（字节）。<br /><br /> 如果 `transferMode` 属性等于 `Buffered`，则此属性应等于 `maxReceivedMessageSize` 属性值。<br /><br /> 如果 `transferMode` 属性等于 `Streamed`，则此属性不能大于 `maxReceivedMessageSize` 属性值，应当至少为标头的大小。<br /><br /> 默认值为 65536。 有关详细信息，请参阅 <xref:System.ServiceModel.Configuration.NetNamedPipeBindingElement.MaxBufferSize%2A>。|  
|`maxConnections`|一个整数，指定服务将创建/接受的最大出站和入站连接数。 传入和传出连接分别根据此属性指定的限制进行计数。<br /><br /> 超出此限制的入站连接需要排队，直到连接数低于限制值。<br /><br /> 超出此限制的出站连接需要排队，直到连接数低于限制值。<br /><br /> 默认值为 10。|  
|`maxReceivedMessageSize`|一个正整数，指定采用此绑定配置的通道上可以接收的最大消息大小（字节），包括消息头。 如果消息超出此限制，则发送方将收到 SOAP 错误。 接收方将删除该消息，并在跟踪日志中创建事件项。 默认值为 65536。|  
|`name`|一个包含绑定的配置名称的字符串。 因为此值用作绑定的标识，所以它应该是唯一的。 从 .NET Framework 4 开始，绑定和行为不需要具有名称。 有关默认配置和无值绑定和行为的详细信息，请参阅[WCF 服务的](../../../wcf/samples/simplified-configuration-for-wcf-services.md)[简化配置](../../../wcf/simplified-configuration.md)和简化配置。|  
|`openTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成打开操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|`portSharingEnabled`|一个布尔值，指定是否为此连接启用 TCP 端口共享。 如果此值为 `false`，则每个绑定都使用自己的独占端口。 此设置只与服务相关，因为客户端不受影响。|  
|`receiveTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成接收操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:10:00。|  
|`sendTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成发送操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|`transactionFlow`|一个布尔值，指定绑定是否支持流动 WS-Transactions。 默认值为 `false`。|  
|`transactionProtocol`|指定与此绑定一起使用的事务处理协议。 有效值为<br /><br /> -OleTransactions<br />-WSAtomicTransactionOctober2004<br /><br /> 默认值为 OleTransactions。 此属性的类型为 <xref:System.ServiceModel.TransactionProtocol>。|  
|`transferMode`|一个 <xref:System.ServiceModel.TransferMode> 值，指定为请求或响应对消息进行缓冲处理还是流式处理。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|说明|  
|-------------|-----------------|  
|[\<security>](security-of-nettcpbinding.md)|定义绑定的安全设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.NetTcpSecurityElement>。|  
|[\<readerQuotas>](/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|定义可由采用此绑定配置的终结点进行处理的 SOAP 消息的复杂性约束。 此元素的类型为 <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement>。|  
|[\<reliableSession>](/previous-versions/ms731375(v=vs.90))|指定是否在通道终结点之间建立可靠会话。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|说明|  
|-------------|-----------------|  
|[\<bindings>](bindings.md)|此元素包含标准绑定和自定义绑定的集合。|  
  
## <a name="remarks"></a>备注

默认情况下，此绑定会生成一个运行时通信堆栈，该堆栈使用传输安全、用于消息传递的 TCP 和二进制消息编码机制。 此绑定是一个适当的 Windows Communication Foundation， (WCF) 系统提供的选择，用于通过 Intranet 进行通信。  
  
 的默认配置 `netTcpBinding` 比提供的配置更快 `wsHttpBinding` ，但它仅适用于 WCF 通信。 可以使用可选的 `securityMode` 属性配置安全行为。 使用可选的 `reliableSessionEnabled` 属性可以配置 WS-ReliableMessaging 的用法。 但是在默认情况下可靠消息传递为关闭状态。 一般来说，HTTP 系统提供的绑定（如 `wsHttpBinding` 和 `basicHttpBinding`）是默认配置为打开事项，而 `netTcpBinding` 绑定是默认配置为关闭事项，因此，你必须选择性加入所需事项才能获取支持，例如获取对一种 WS-* 规范的支持。 这意味着在终结点之间交换消息时，TCP 的默认配置比 HTTP 绑定的默认配置更快。  
  
## <a name="example"></a>示例

绑定是在客户端和服务的配置文件中指定的。 绑定类型是在 `binding` 元素的 `<endpoint>` 属性中指定的。 如果要配置 netTcpBinding 绑定并更改它的一些设置，则必须定义绑定配置。 终结点必须使用 `bindingConfiguration` 特性来引用绑定配置。 在下面的示例中，定义了一个绑定配置。  
  
```xml  
<services>
  <service name="Microsoft.ServiceModel.Samples.CalculatorService"
           behaviorConfiguration="CalculatorServiceBehavior">
    ...
    <endpoint address=""
              binding="netTcpBinding"
              contract="Microsoft.ServiceModel.Samples.ICalculator" />
    ...
  </service>
</services>
<bindings>
  <netTcpBinding>
    <binding closeTimeout="00:01:00"
             openTimeout="00:01:00"
             receiveTimeout="00:10:00"
             sendTimeout="00:01:00"
             transactionFlow="false"
             transferMode="Buffered"
             transactionProtocol="OleTransactions"
             hostNameComparisonMode="StrongWildcard"
             listenBacklog="10"
             maxBufferPoolSize="524288"
             maxBufferSize="65536"
             maxConnections="10"
             maxReceivedMessageSize="65536">
      <readerQuotas maxDepth="32"
                    maxStringContentLength="8192"
                    maxArrayLength="16384"
                    maxBytesPerRead="4096"
                    maxNameTableCharCount="16384" />
      <reliableSession ordered="true"
                       inactivityTimeout="00:10:00"
                       enabled="false" />
      <security mode="Transport">
        <transport clientCredentialType="Windows" protectionLevel="EncryptAndSign" />
      </security>
    </binding>
  </netTcpBinding>
</bindings>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.NetTcpBinding>
- <xref:System.ServiceModel.Configuration.NetTcpBindingElement>
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
