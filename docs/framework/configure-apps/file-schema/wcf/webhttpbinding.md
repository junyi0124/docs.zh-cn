---
title: <webHttpBinding>
ms.date: 03/30/2017
ms.assetid: 84179d77-825d-44b9-895a-ab08e7aa044d
ms.openlocfilehash: 26e0c707422518fbdaf289faa64bb73875f62a58
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91177990"
---
# \<webHttpBinding>

定义一个绑定元素，该元素用于为响应 HTTP 请求（而不是 SOAP 消息）的 Windows Communication Foundation (WCF) Web 服务配置终结点。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<webHttpBinding>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<webHttpBinding>
  <binding allowCookies="Boolean"
           bypassProxyOnLocal="Boolean"
           closeTimeout="TimeSpan"
           hostNameComparisonMode="StrongWildCard/Exact/WeakWildcard"
           maxBufferPoolSize="integer"
           maxBufferSize="integer"
           maxReceivedMessageSize="Integer"
           name="string"
           openTimeout="TimeSpan"
           proxyAddress="URI"
           receiveTimeout="TimeSpan"
           sendTimeout="TimeSpan"
           transferMode="Buffered/Streamed/StreamedRequest/StreamedResponse"
           useDefaultWebProxy="Boolean"
           writeEncoding="UnicodeFffeTextEncoding/Utf16TextEncoding/Utf8TextEncoding">
    <security mode="None/Transport/TransportCredentialOnly">
      <transport clientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
                 proxyCredentialType="Basic/Digest/None/Ntlm/Windows"
                 realm="string" />
    </security>
    <readerQuotas maxArrayLength="Integer"
                  maxBytesPerRead="Integer"
                  maxDepth="Integer"
                  maxNameTableCharCount="Integer"
                  maxStringContentLength="Integer" />
  </binding>
</webHttpBinding>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 以下几节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|allowCookies|一个布尔值，指示客户端是否接受 Cookie 并在今后的请求中传播这些 Cookie。 默认值为 false。<br /><br /> 在与使用 Cookie 的 ASMX Web 服务进行交互时，可以使用此属性。 通过这种方式，可以确保从服务器返回的 Cookie 自动复制到客户端今后对该服务的所有请求。|  
|bypassProxyOnLocal|一个布尔值，指示是否对本地地址不使用代理服务器。 默认为 `false`。|  
|closeTimeout|一个 <xref:System.TimeSpan> 值，指定为完成关闭操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|hostnameComparisonMode|指定用于分析 URI 的 HTTP 主机名比较模式。 此属性的类型为 <xref:System.ServiceModel.HostNameComparisonMode>，指示在对 URI 进行匹配时，是否使用主机名来访问服务。 默认值为 <xref:System.ServiceModel.HostNameComparisonMode.StrongWildcard>，表示忽略匹配项中的主机名。|  
|maxBufferPoolSize|一个整数，指定此绑定的最大缓冲池大小。 默认值为 524,288 字节 (512 * 1024)。 Windows Communication Foundation (WCF) 的许多部件使用缓冲区。 每次使用缓冲区时，创建和销毁它们都将占用大量资源，而缓冲区的垃圾回收过程也是如此。 利用缓冲池，可以从缓冲池中获得缓冲区，使用缓冲区，然后在完成工作后将其返回给缓冲池。 这样就避免了创建和销毁缓冲区的系统开销。|  
|maxBufferSize|一个整数，指定为从通道接收消息的消息缓冲区管理器分配并供其使用的最大内存量。 默认值为 524,288 (0x80000) 字节。|  
|maxReceivedMessageSize|一个正整数，指定采用此绑定配置的通道上可以接收的最大消息大小（字节），包括消息头。 如果消息超出此限制，则发送方将收到错误。 接收方将删除该消息，并在跟踪日志中创建事件项。 默认值为 65536。 **注意：**  仅在 ASP.NET 兼容模式下增大此值。 还应增加 (的值， `httpRuntime` 请参阅 [httpRuntime 元素 (ASP.NET 设置架构) ](/previous-versions/dotnet/netframework-4.0/e1f13641(v=vs.100))) 。|  
|name|一个包含绑定的配置名称的字符串。 因为此值用作绑定的标识，所以它应该是唯一的。 从 .NET Framework 4 开始，绑定和行为不需要具有名称。 有关默认配置和无值绑定和行为的详细信息，请参阅[WCF 服务的](../../../wcf/samples/simplified-configuration-for-wcf-services.md)[简化配置](../../../wcf/simplified-configuration.md)和简化配置。|  
|openTimeout|一个 <xref:System.TimeSpan> 值，指定为完成打开操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|proxyAddress|一个指定 HTTP 代理的地址的 URI。 如果 `useSystemWebProxy` 为 `true`，则此设置必须为 `null`。 默认为 `null`。|  
|receiveTimeout|一个 <xref:System.TimeSpan> 值，指定为完成接收操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|sendTimeout|一个 <xref:System.TimeSpan> 值，指定为完成发送操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|transferMode。|一个 <xref:System.ServiceModel.TransferMode> 值，指示采用此绑定配置的服务是使用消息传输的流处理模式还是缓冲模式（还是使用这两种模式）。 默认为 `Buffered`。|  
|useDefaultWebProxy|一个布尔值，指定是否使用系统的自动配置 HTTP 代理。 默认为 `true`。|  
|writeEncoding|指定用于消息文本的字符编码。 有效值包括以下值：<br /><br /> UnicodeFffeTextEncoding：Unicode BigEndian 编码。<br /><br /> Utf16TextEncoding：16 位编码。<br /><br /> Utf8TextEncoding：8 位编码。<br /><br /> 默认值为 Utf8TextEncoding。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<readerQuotas>](/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|定义可由采用此绑定配置的终结点进行处理的 POX 消息的复杂性约束。 此元素的类型为 <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement>。|  
|[\<security>](security-of-webhttpbinding.md)|定义绑定的安全设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.WebHttpSecurityElement>。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<bindings>](bindings.md)|此元素包含标准绑定和自定义绑定的集合。|  
  
## <a name="remarks"></a>备注  

 WCF Web 编程模型允许开发人员通过 HTTP 请求（这些请求使用 "纯旧 XML" (POX) 样式消息，而不是基于 SOAP 的消息）来公开 WCF Web 服务。 要使客户端使用 HTTP 请求与服务进行通信，必须使用附加了服务的来配置服务的终结点 [\<webHttpBinding>](webhttpbinding.md) \<WebHttpBehavior> 。  
  
 WCF 中的联合和 ASP 支持。AJAX 集成是在 Web 编程模型的基础上构建的。 有关模型的详细信息，请参阅 [WCF WEB HTTP 编程模型](../../../wcf/feature-details/wcf-web-http-programming-model.md)。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.WebHttpBinding>
- <xref:System.ServiceModel.Configuration.WebHttpBindingElement>
- [WCF Web HTTP 编程模型](../../../wcf/feature-details/wcf-web-http-programming-model.md)
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
