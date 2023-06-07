---
title: <netHttpsBinding>
ms.date: 03/30/2017
ms.assetid: ff122116-6042-4792-9f21-275b4f97a105
ms.openlocfilehash: f6eca792f9bae6712a327fbcb5bdf47be9699062
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204588"
---
# \<netHttpsBinding>

表示 Windows Communication Foundation (WCF) 服务可用于配置和公开能够通过 HTTPS 进行通信的终结点的绑定。 如果用于双工协定，将使用 Web Sockets，否则将使用 HTTPS。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<netHttpsBinding>**  

## <a name="syntax"></a>语法  
  
```xml  
<netHttpsBinding>
  <binding allowCookies="Boolean"
           bypassProxyOnLocal="Boolean"
           closeTimeout="TimeSpan"
           hostNameComparisonMode="StrongWildCard/Exact/WeakWildcard"
           maxBufferPoolSize="Integer"
           maxBufferSize="Integer"
           maxReceivedMessageSize="Integer"
           messageEncoding="Binary/Text/Mtom"
           name="string"
           openTimeout="TimeSpan"
           proxyAddress="URI"
           receiveTimeout="TimeSpan"
           sendTimeout="TimeSpan"
           textEncoding="UnicodeFffeTextEncoding/Utf16TextEncoding/Utf8TextEncoding"
           transferMode="Buffered/Streamed/StreamedRequest/StreamedResponse"
           useDefaultWebProxy="Boolean">
    <security mode="None/Transport/Message/TransportWithMessageCredential/TransportCredentialOnly">
      <transport clientCredentialType="None/Basic/Digest/Ntlm/Windows/Certificate"
                 proxyCredentialType="None/Basic/Digest/Ntlm/Windows"
                 realm="string" />
      <message algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
               clientCredentialType="UserName/Certificate" />
    </security>
    <readerQuotas maxArrayLength="Integer"
                  maxBytesPerRead="Integer"
                  maxDepth="Integer"
                  maxNameTableCharCount="Integer"
                  maxStringContentLength="Integer" />
  </binding>
</netHttpsBinding>
```  
  
## <a name="type"></a>类型  

 `Type`  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|`allowCookies`|一个布尔值，指示客户端是否接受 Cookie 并在今后的请求中传播这些 Cookie。 默认值为 `false`。<br /><br /> 在与使用 Cookie 的 ASMX Web 服务进行交互时，可以使用此属性。 通过这种方式，可以确保从服务器返回的 Cookie 自动复制到客户端今后对该服务的所有请求。|  
|`bypassProxyOnLocal`|一个布尔值，指示是否对本地地址不使用代理服务器。 默认为 `false`。<br /><br /> 如果 Internet 资源具有本地地址，则该资源是本地资源。 本地地址是在同一台计算机上，本地 LAN 或 intranet 上的地址，在语法上，在 ) 缺少句点 `http://webserver/` (。 `http://localhost/`<br /><br /> 通过设置此属性，可以确定在访问本地资源时，采用 BasicHttpBinding 配置的终结点是否使用代理服务器。 如果此属性为 `true`，则对本地 Internet 资源的请求不使用代理服务器。 当此属性设置为 `true` 时，如果希望客户端在与同一台计算机上的服务通话时使用代理，请使用主机名称（而非 localhost）。<br /><br /> 当此属性为 `false` 时，所有 Internet 请求都通过代理服务器发出。|  
|`closeTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成关闭操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|`hostNameComparisonMode`|指定用于分析 URI 的 HTTP 主机名比较模式。 此属性的类型为 <xref:System.ServiceModel.HostNameComparisonMode>，指示在对 URI 进行匹配时，是否使用主机名来访问服务。 默认值为 <xref:System.ServiceModel.HostNameComparisonMode.StrongWildcard>，表示忽略匹配项中的主机名。|  
|`maxBufferPoolSize`|一个整数值，指定为从通道接收消息的消息缓冲区管理器分配并供其使用的最大内存量。 默认值为 524288 (0x80000) 字节。<br /><br /> 通过使用缓冲池，缓冲区管理器可将使用缓冲区的开销降到最低。 当消息离开通道时，服务需要使用缓冲区来处理这些消息。 如果缓冲池中的内存不够用来处理消息负载，则缓冲区管理器必须从 CLR 堆分配更多内存，而这会增加垃圾回收的系统开销。 从 CLR 垃圾堆进行大量分配表明缓冲池太小，可以通过提高此属性指定的限制来实现更大的内存分配，从而提高性能。|  
|`maxBufferSize`|一个整数值，指定为采用此绑定配置的终结点处理消息时存储消息的缓冲区的最大大小（字节）。 默认值为 65,536 字节。|  
|`maxReceivedMessageSize`|一个正整数，定义在采用此绑定配置的通道上可以接收的消息的最大消息大小（字节），包括消息头。 如果消息对于接收方而言太大，则发送方将收到 SOAP 错误。 接收方将删除该消息，并在跟踪日志中创建事件项。 默认值为 65,536 字节。|  
|`messageEncoding`|定义用于对 SOAP 消息进行编码的编码器。 有效值包括以下值：<br /><br /> -Text：使用文本消息编码器。<br />-Mtom：使用消息传输组织机制 1.0 (MTOM) 编码器。<br /><br /> 默认值为 Text。 此属性的类型为 <xref:System.ServiceModel.WSMessageEncoding>。|  
|`name`|一个包含绑定的配置名称的字符串。 因为此值用作绑定的标识，所以它应该是唯一的。 从 .NET Framework 4 开始，绑定和行为不需要具有名称。 有关默认配置和无值绑定和行为的详细信息，请参阅[WCF 服务的](../../../wcf/samples/simplified-configuration-for-wcf-services.md)[简化配置](../../../wcf/simplified-configuration.md)和简化配置。|  
|`openTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成打开操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|`proxyAddress`|一个包含 HTTP 代理地址的 URI。 如果 `useSystemWebProxy` 设置为 `true`，则此设置必须为 `null`。 默认为 `null`。|  
|`receiveTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成接收操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:10:00。|  
|`sendTimeout`|一个 <xref:System.TimeSpan> 值，指定为完成发送操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|`textEncoding`|设置要用来在绑定上发出消息的字符集编码。 有效值包括以下值：<br /><br /> -BigEndianUnicode： Unicode BigEndian 编码。<br />-Unicode：16位编码。<br />-UTF8：8位编码<br /><br /> 默认编码为 UTF8。 此属性的类型为 <xref:System.Text.Encoding>。|  
|`transferMode`|一个有效的 <xref:System.ServiceModel.TransferMode> 值，指定为请求或响应对消息进行缓冲处理还是流式处理。|  
|`useDefaultWebProxy`|一个布尔值，指定是否应在可用时使用系统的自动配置 HTTP 代理。 默认为 `true`。|  
|||  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<security>](security-of-nethttpbinding.md)|定义绑定的安全设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.BasicHttpsSecurityElement>。 |  
|[\<readerQuotas>](/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|定义可由采用此绑定配置的终结点进行处理的 SOAP 消息的复杂性约束。 此元素的类型为 <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement>。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<bindings>](bindings.md)|此元素包含标准绑定和自定义绑定的集合。|  
  
## <a name="remarks"></a>备注  

 NetHttpsBinding 使用 HTTPS 作为传输协议来发送消息。 如果用于双工协定，将使用 Web Sockets。  如果用于请求-答复协定，则 NetHttpsBinding 的行为将类似于具有二进制编码器的 BasicHttpsBinding。  
  
 默认情况下，安全设置处于关闭状态，但可以将子元素的模式属性设置 [\<security>](security-of-basichttpbinding.md) 为以外的值 `None` 。 默认情况下，它使用“Text”消息编码和 UTF-8 文本编码。  
  
## <a name="example"></a>示例  

 下面的示例演示如何使用 <xref:System.ServiceModel.NetHttpBinding>，它提供 HTTPS 通信以及与第一代和第二代 Web 服务的最大互操作性。 绑定是在客户端和服务的配置文件中指定的。 绑定类型是使用 `binding` 元素的 `<endpoint>` 属性指定的。 如果要配置基本绑定并更改它的某些设置，则必须定义一个绑定配置。 终结点必须使用 `bindingConfiguration` 元素的 `<endpoint>` 属性按名称引用绑定配置，如以下服务配置代码所示。  
  
```xml  
<system.serviceModel>
  <services>
    <service type="Microsoft.ServiceModel.Samples.CalculatorService"
             behaviorConfiguration="CalculatorServiceBehavior">
      <endpoint address=""
                binding="netHttpsBinding"
                bindingConfiguration="Binding1"
                contract="Microsoft.ServiceModel.Samples.ICalculator" />
    </service>
  </services>
  <bindings>
    <netHttpsBinding>
      <binding name="Binding1"
               hostNameComparisonMode="StrongWildcard"
               receiveTimeout="00:10:00"
               sendTimeout="00:10:00"
               openTimeout="00:10:00"
               closeTimeout="00:10:00"
               maxReceivedMessageSize="65536"
               maxBufferSize="65536"
               maxBufferPoolSize="524288"
               transferMode="Buffered"
               messageEncoding="Binary"
               textEncoding="utf-8"
               bypassProxyOnLocal="false"
               useDefaultWebProxy="true">
        <security mode="None" />
      </binding>
    </netHttpsBinding>
  </bindings>
</system.serviceModel>
```  
  
## <a name="example"></a>示例  

 从 .NET Framework 4 开始，绑定和行为不需要具有名称。 可以通过从终结点地址和绑定的名称中删除 bindingConfiguration 来完成上一示例中的功能。  
  
```xml  
<system.serviceModel>
  <services>
    <service type="Microsoft.ServiceModel.Samples.CalculatorService"
             behaviorConfiguration="CalculatorServiceBehavior">
      <endpoint address=""
                binding="netHttpsBinding"
                contract="Microsoft.ServiceModel.Samples.ICalculator" />
    </service>
  </services>
  <bindings>
    <netHttpsBinding>
      <binding hostNameComparisonMode="StrongWildcard"
               receiveTimeout="00:10:00"
               sendTimeout="00:10:00"
               openTimeout="00:10:00"
               closeTimeout="00:10:00"
               maxReceivedMessageSize="65536"
               maxBufferSize="65536"
               maxBufferPoolSize="524288"
               transferMode="Buffered"
               messageEncoding="Binary"
               textEncoding="utf-8"
               bypassProxyOnLocal="false"
               useDefaultWebProxy="true">
        <security mode="None" />
      </binding>
    </netHttpsBinding>
  </bindings>
</system.serviceModel>
```  
  
 有关默认配置和无值绑定和行为的详细信息，请参阅[WCF 服务的](../../../wcf/samples/simplified-configuration-for-wcf-services.md)[简化配置](../../../wcf/simplified-configuration.md)和简化配置。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Channels.Binding>
- <xref:System.ServiceModel.Channels.BindingElement>
- <xref:System.ServiceModel.BasicHttpBinding>
- <xref:System.ServiceModel.Configuration.BasicHttpBindingElement>
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
