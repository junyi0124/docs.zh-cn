---
title: <wsDualHttpBinding>
ms.date: 03/30/2017
helpviewer_keywords:
- wsDualHttpBinding Element
ms.assetid: fd8ac4e2-5641-473b-9115-73f14ab1c065
ms.openlocfilehash: 114543e43306e4195f92ca625e894a532084be6b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91177769"
---
# \<wsDualHttpBinding>

定义一个安全可靠且可互操作的绑定，该绑定适合于双工服务协定或通过 SOAP 中介进行的通信。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<wsDualHttpBinding>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<wsDualHttpBinding>
  <binding name="String"
          closeTimeout="TimeSpan"
          openTimeout="TimeSpan"
          receiveTimeout="TimeSpan"
          sendTimeout="TimeSpan"
          bypassProxyOnLocal="Boolean"
          clientBaseAddress="URI"
          transactionFlow="Boolean"
          hostNameComparisonMode="StrongWildCard/Exact/WeakWildcard"
          maxBufferPoolSize="integer"
          maxReceivedMessageSize="Integer"
          messageEncoding="Text/Mtom"
          proxyAddress="URI"
          textEncoding="Unicode/BigEndianUnicode/UTF8"
          useDefaultWebProxy="Boolean">
    <reliableSession ordered="Boolean"
                     inactivityTimeout="TimeSpan" />
    <security mode="None/Message">
      <message clientCredentialType="None/Windows/UserName/Certificate/CardSpace"
               negotiateServiceCredential="Boolean"
               algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15" />
    </security>
    <readerQuotas maxArrayLength="Integer"
                  maxBytesPerRead="Integer"
                  maxDepth="Integer"
                  maxNameTableCharCount="Integer"
                  maxStringContentLength="Integer" />
  </binding>
</wsDualHttpBinding>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 以下几节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|bypassProxyOnLocal|一个布尔值，指示是否对本地地址不使用代理服务器。 默认为 `false`。|  
|clientBaseAddress|一个 URI，设置客户端为接收来自服务的响应消息而侦听的基址。 如果指定了此地址，则此地址（以及每个通道的 GUID）将用于侦听。 如果未指定此值，则将以传输特定的方式生成客户端基址。 默认为 `null`。|  
|closeTimeout|一个 <xref:System.TimeSpan> 值，指定为完成关闭操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|hostnameComparisonMode|指定用于分析 URI 的 HTTP 主机名比较模式。 此属性的类型为 <xref:System.ServiceModel.HostNameComparisonMode>，指示在对 URI 进行匹配时，是否使用主机名来访问服务。 默认值为 <xref:System.ServiceModel.HostNameComparisonMode.StrongWildcard>，表示忽略匹配项中的主机名。|  
|maxBufferPoolSize|一个整数，指定此绑定的最大缓冲池大小。 默认值为 524,288 字节 (512 * 1024)。 Windows Communication Foundation (WCF) 的许多部件使用缓冲区。 每次使用缓冲区时，创建和销毁它们都将占用大量资源，而缓冲区的垃圾回收过程也是如此。 利用缓冲池，可以从缓冲池中获得缓冲区，使用缓冲区，然后在完成工作后将其返回给缓冲池。 这样就避免了创建和销毁缓冲区的系统开销。|  
|maxReceivedMessageSize|一个正整数，指定采用此绑定配置的通道上可以接收的最大消息大小（字节），包括消息头。 如果消息超出此限制，则发送方将收到 SOAP 错误。 接收方将删除该消息，并在跟踪日志中创建事件项。 默认值为 65536。|  
|messageEncoding|定义用于对消息进行编码的编码器。 有效值包括以下值：<br /><br /> -Text：使用文本消息编码器。<br />-Mtom：使用消息传输组织机制 1.0 (MTOM) 编码器。<br />-默认值为 Text。<br /><br /> 此属性的类型为 <xref:System.ServiceModel.WSMessageEncoding>。|  
|name|一个包含绑定的配置名称的字符串。 因为此值用作绑定的标识，所以它应该是唯一的。 从 .NET Framework 4 开始，绑定和行为不需要具有名称。 有关默认配置和无值绑定和行为的详细信息，请参阅[WCF 服务的](../../../wcf/samples/simplified-configuration-for-wcf-services.md)[简化配置](../../../wcf/simplified-configuration.md)和简化配置。|  
|openTimeout|一个 <xref:System.TimeSpan> 值，指定为完成打开操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|proxyAddress|一个指定 HTTP 代理的地址的 URI。 如果 `useDefaultWebProxy` 为 `true`，则此设置必须为 `null`。 默认为 `null`。|  
|receiveTimeout|一个 <xref:System.TimeSpan> 值，指定为完成接收操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|sendTimeout|一个 <xref:System.TimeSpan> 值，指定为完成发送操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|textEncoding|设置要用来在绑定上发出消息的字符集编码。 有效值包括以下值：<br /><br /> -BigEndianUnicode： Unicode BigEndian 编码。<br />-Unicode：16位编码。<br />-UTF8：8位编码<br /><br /> 默认编码为 UTF8。 此属性的类型为 <xref:System.Text.Encoding>。|  
|transactionFlow|一个布尔值，指定绑定是否支持流动 WS-Transactions。 默认为 `false`。|  
|useDefaultWebProxy|一个布尔值，指示是否使用系统的自动配置 HTTP 代理。 如果此属性为 `null`，则代理地址必须为 `true`（即，不设置代理地址）。 默认为 `true`。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<security>](security-of-wsdualhttpbinding.md)|定义绑定的安全设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.WSDualHttpSecurityElement>。|  
|[\<readerQuotas>](/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|定义可由采用此绑定配置的终结点进行处理的 SOAP 消息的复杂性约束。 此元素的类型为 <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement>。|  
|[\<reliableSession>](/previous-versions/ms731375(v=vs.90))|指定是否在通道终结点之间建立可靠会话。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<bindings>](bindings.md)|此元素包含标准绑定和自定义绑定的集合。|  
  
## <a name="remarks"></a>备注  

 `WSDualHttpBinding` 提供与 `WSHttpBinding` 相同的 Web 服务协议支持，但用于双工协定。 `WSDualHttpBinding` 仅支持 SOAP 安全，且需要可靠的消息传递。 此绑定要求客户端具有可为服务提供回调终结点的公共 URI。 此元素由 `clientBaseAddress` 属性提供。 双向绑定向服务公开客户端的 IP 地址。 客户端应使用安全来确保仅连接到自己信任的服务。  
  
 此绑定可用于通过一个或多个 SOAP 媒介可靠地进行通信。  
  
 默认情况下，此绑定会生成一个运行时堆栈，该堆栈包含 WS-ReliableMessaging 以保证可靠性，包含 WS-Security 以保证消息安全性和进行身份验证，包含 HTTP 以便进行消息传递，并且包含 Text/XML 消息编码功能。  
  
## <a name="example"></a>示例  
  
```xml  
<configuration>
  <system.ServiceModel>
    <bindings>
      <wsDualHttpBinding>
        <binding closeTimeout="00:00:10"
                 openTimeout="00:00:20"
                 receiveTimeout="00:00:30"
                 sendTimeout="00:00:40"
                 bypassProxyOnLocal="false"
                 clientBaseAddress="http://localhost:8001/client/"
                 transactionFlow="true"
                 hostNameComparisonMode="WeakWildcard"
                 maxReceivedMessageSize="1000"
                 messageEncoding="Mtom"
                 proxyAddress="http://foo/bar"
                 textEncoding="utf-16"
                 useDefaultWebProxy="false">
          <reliableSession ordered="false"
                           inactivityTimeout="00:02:00" />
          <security mode="None">
            <message clientCredentialType="None"
                     negotiateServiceCredential="false"
                     algorithmSuite="Aes128" />
          </security>
        </binding>
      </wsDualHttpBinding>
    </bindings>
  </system.ServiceModel>
</configuration>
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.WSDualHttpBinding>
- <xref:System.ServiceModel.Configuration.WSDualHttpBindingElement>
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
