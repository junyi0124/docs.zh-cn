---
title: <netNamedPipeBinding>
ms.date: 03/30/2017
ms.assetid: 00a8580b-face-47a4-838d-b9fed48e72df
ms.openlocfilehash: f1ec6091d72c1d1c6d75c44dd1f98d6d4e10ea12
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204562"
---
# \<netNamedPipeBinding>

定义一个既安全又可靠且针对计算机上跨进程通信进行了优化的绑定。 默认情况下，此绑定生成一个运行时通信堆栈，该堆栈包含 WS-ReliableMessaging 以保证可靠性，包含传输安全机制以保证传递的安全性，包含命名管道以便进行消息传递，并且包含二进制消息编码机制。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<netNamedPipeBinding>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<netNamedPipeBinding>
  <binding closeTimeout="TimeSpan"
           hostNameComparisonMode="StrongWildCard/Exact/WeakWildcard"
           maxBufferPoolSize="Integer"
           maxBufferSize="Integer"
           maxConnections="Integer"
           maxReceivedMessageSize="Integer"
           name="String"
           openTimeout="TimeSpan"
           receiveTimeout="TimeSpan"
           sendTimeout="TimeSpan"
           transactionFlow="Boolean"
           transactionProtocol="OleTransactions/WS-AtomicTransactionOctober2004"
           transferMode="Buffered/Streamed/StreamedRequest/StreamedResponse">
    <security mode="None/Transport">
      <transport protectionLevel="None/Sign/EncryptAndSign" />
    </security>
    <readerQuotas maxArrayLength="Integer"
                  maxBytesPerRead="Integer"
                  maxDepth="Integer"
                  maxNameTableCharCount="Integer"
                  maxStringContentLength="Integer" />
  </binding>
</netNamedPipeBinding>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 以下几节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|closeTimeout|一个 <xref:System.TimeSpan> 值，指定为完成关闭操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|hostNameComparisonMode|指定用于分析 URI 的 HTTP 主机名比较模式。 此属性的类型为 <xref:System.ServiceModel.HostNameComparisonMode>，指示在对 URI 进行匹配时，是否使用主机名来访问服务。 默认值为 <xref:System.ServiceModel.HostNameComparisonMode.StrongWildcard>，表示忽略匹配项中的主机名。|  
|maxBufferPoolSize|一个整数，指定此绑定的最大缓冲池大小。 默认值为 524,288 字节 (512 * 1024)。 Windows Communication Foundation (WCF) 的许多部件使用缓冲区。 每次使用缓冲区时，创建和销毁它们都将占用大量资源，而缓冲区的垃圾回收过程也是如此。 利用缓冲池，可以从缓冲池中获得缓冲区，使用缓冲区，然后在完成工作后将其返回给缓冲池。 这样就避免了创建和销毁缓冲区的系统开销。|  
|maxBufferSize|一个正整数，指定内存中用于存储消息的缓冲区的最大大小（字节）。 如果缓冲区已满，则多余的数据会保留在基础套接字中，直到缓冲区重新具有可用空间。 该值不能小于 `maxReceivedMessageSize` 属性。 默认值为 65536。 有关详细信息，请参阅 <xref:System.ServiceModel.Configuration.NetNamedPipeBindingElement.MaxBufferSize%2A>。|  
|maxConnections|一个整数，指定服务将创建/接受的最大出站和入站连接数。 传入和传出连接分别根据此属性指定的限制进行计数。<br /><br /> 超出此限制的入站连接需要排队，直到连接数低于限制值。<br /><br /> 超出此限制的出站连接需要排队，直到连接数低于限制值。<br /><br /> 默认值为 10。|  
|maxReceivedMessageSize|一个正整数，指定采用此绑定配置的通道上可以接收的最大消息大小（字节），包括消息头。 如果消息超出此限制，则发送方将收到 SOAP 错误。 接收方将删除该消息，并在跟踪日志中创建事件项。 默认值为 65536。|  
|name|一个包含绑定的配置名称的字符串。 因为此值用作绑定的标识，所以它应该是唯一的。 从 .NET Framework 4 开始，绑定和行为不需要具有名称。 有关默认配置和无值绑定和行为的详细信息，请参阅[WCF 服务的](../../../wcf/samples/simplified-configuration-for-wcf-services.md)[简化配置](../../../wcf/simplified-configuration.md)和简化配置。|  
|openTimeout|一个 <xref:System.TimeSpan> 值，指定为完成打开操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|receiveTimeout|一个 <xref:System.TimeSpan> 值，指定为完成接收操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:10:00。|  
|sendTimeout|一个 <xref:System.TimeSpan> 值，指定为完成发送操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|transactionFlow|一个布尔值，指定绑定是否支持流动 WS-Transactions。 默认为 `false`。|  
|transactionProtocol|指定与此绑定一起使用的事务处理协议。 有效值为<br /><br /> -OleTransactions<br />-AtomicTransactionOctober2004<br /><br /> 默认值为 OleTransactions。 此属性的类型为 <xref:System.ServiceModel.TransactionProtocol>。|  
|transferMode|一个 <xref:System.ServiceModel.TransferMode> 值，指定为请求或响应对消息进行缓冲处理还是流式处理。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<security>](security-of-netnamedpipebinding.md)|定义绑定的安全设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.NetNamedPipeBindingElement>。|  
|[\<readerQuotas>](/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|定义可由采用此绑定配置的终结点进行处理的 SOAP 消息的复杂性约束。 此元素的类型为 <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement>。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<bindings>](bindings.md)|此元素包含标准绑定和自定义绑定的集合。|  
  
## <a name="remarks"></a>备注  

 默认情况下，`NetNamedPipeBinding` 会生成一个运行时通信堆栈，该堆栈可使用传输安全、用于消息传递的命名管道和二进制消息编码。 此绑定是 Windows Communication Foundation (WCF) 系统提供的一个相应选项，可用于计算机上的通信。 它还支持事务。  
  
 `NetNamedPipeBinding` 的默认配置与 `NetTcpBinding` 所提供的配置类似，但要简单一些，因为 WCF 实现仅计划在计算机上使用，因此公开的功能就更少。 其中最显著的差异在于 `securityMode` 设置只提供 `None` 和 `Transport` 选项。 不包括 SOAP 安全支持选项。 可以使用可选的 `securityMode` 属性配置安全行为。  
  
## <a name="example"></a>示例  

 下面的示例演示在同一台计算机上提供跨进程通信的 netNamedPipeBinding 绑定。 命名管道不能跨计算机工作。  
  
 绑定是在客户端和服务的配置文件中指定的。 绑定类型是在 `binding` 元素的 `<endpoint>` 属性中指定的。 如果要配置 netNamedPipeBinding 绑定并更改它的一些设置，则必须定义绑定配置。 终结点必须使用 `bindingConfiguration` 属性按名称来引用绑定配置。 在此示例中，此绑定配置被命名为 Binding1。  
  
```xml  
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceModel.Samples.CalculatorService"
               behaviorConfiguration="CalculatorServiceBehavior">
        <host>
          <baseAddresses>
            <add baseAddress="http://localhost:8000/ServiceModelSamples/service" />
          </baseAddresses>
        </host>
        <!-- this endpoint is exposed at the base address provided by host: net.pipe://localhost/ServiceModelSamples/service  -->
        <endpoint address="net.pipe://localhost/ServiceModelSamples/service"
                  binding="netNamedPipeBinding"
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />
        <!-- the mex endpoint is exposed at http://localhost:8000/ServiceModelSamples/service/mex -->
        <endpoint address="mex"
                  binding="mexHttpBinding"
                  contract="IMetadataExchange" />
      </service>
    </services>
    <bindings>
      <netNamedPipeBinding>
        <binding closeTimeout="00:01:00"
                 openTimeout="00:01:00"
                 receiveTimeout="00:10:00"
                 sendTimeout="00:01:00"
                 transactionFlow="false"
                 transferMode="Buffered"
                 transactionProtocol="OleTransactions"
                 hostNameComparisonMode="StrongWildcard"
                 maxBufferPoolSize="524288"
                 maxBufferSize="65536"
                 maxConnections="10"
                 maxReceivedMessageSize="65536">
          <security mode="Transport">
            <transport protectionLevel="EncryptAndSign" />
          </security>
        </binding>
      </netNamedPipeBinding>
    </bindings>
    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->
    <behaviors>
      <serviceBehaviors>
        <behavior name="CalculatorServiceBehavior">
          <serviceMetadata httpGetEnabled="True" />
          <serviceDebug includeExceptionDetailInFaults="False" />
        </behavior>
      </serviceBehaviors>
    </behaviors>
  </system.serviceModel>
</configuration>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.NetNamedPipeBindingElement>
- <xref:System.ServiceModel.NetNamedPipeBinding>
- [\<binding>](bindings.md)
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
