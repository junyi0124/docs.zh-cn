---
title: <wsHttpContextBinding>
ms.date: 03/30/2017
ms.assetid: 1e40b5c9-0df2-4d66-afc5-a99d0e4ae7a4
ms.openlocfilehash: 4b611443e836773ad615f919921c8e7ddb29cba9
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91181279"
---
# \<wsHttpContextBinding>

为要求对保护级别进行签名的 <xref:System.ServiceModel.WSHttpBinding> 提供上下文。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<wsHttpContextBinding>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<wsHttpContextBinding>
  <binding allowCookies="Boolean"
           bypassProxyOnLocal="Boolean"
           closeTimeout="TimeSpan"
           contextProtectionLevel="EncryptAndSign/None/Sign"
           hostNameComparisonMode="StrongWildCard/Exact/WeakWildcard"
           maxBufferPoolSize="integer"
           maxReceivedMessageSize="Integer"
           messageEncoding="Text/Mtom"
           name="string"
           openTimeout="TimeSpan"
           proxyAddress="URI"
           receiveTimeout="TimeSpan"
           sendTimeout="TimeSpan"
           textEncoding="UnicodeFffeTextEncoding/Utf16TextEncoding/Utf8TextEncoding"
           transactionFlow="Boolean"
           useDefaultWebProxy="Boolean">
    <reliableSession ordered="Boolean"
                     inactivityTimeout="TimeSpan"
                     enabled="Boolean" />
    <security mode="Message/None/Transport/TransportWithCredential">
      <transport clientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
                 proxyCredentialType="Basic/Digest/None/Ntlm/Windows"
                 realm="string"
                 defaultClientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
                 defaultProxyCredentialType="Basic/Digest/None/Ntlm/Windows"
                 defaultRealm="string" />
      <message clientCredentialType="Certificate/IssuedToken/None/UserName/Windows"
               algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
               establishSecurityContext="Boolean"
               negotiateServiceCredential="Boolean" />
    </security>
    <readerQuotas maxArrayLength="Integer"
                  maxBytesPerRead="Integer"
                  maxDepth="Integer"
                  maxNameTableCharCount="Integer"
                  maxStringContentLength="Integer" />
  </binding>
</wsHttpContextBinding>
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 以下几节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|allowCookies|一个布尔值，指示客户端是否接受 Cookie 并在今后的请求中传播这些 Cookie。 默认为 `false`。<br /><br /> 当 `allowCookies` 设置为 `true` 时，contextChannel 将使用 httpCookies 作为交换上下文的模式。 当此属性设置为 `false` 时，将上下文作为 soap 标头进行交换。<br /><br /> 默认值是 `false`。<br /><br /> 在与使用 Cookie 的 ASMX Web 服务进行交互时，可以使用此属性。 通过这种方式，可以确保从服务器返回的 Cookie 自动复制到客户端今后对该服务的所有请求。|  
|bypassProxyOnLocal|一个布尔值，指示是否对本地地址不使用代理服务器。 默认为 `false`。|  
|closeTimeout|一个 <xref:System.TimeSpan> 值，指定为完成关闭操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|contextProtectionLevel|一个有效的 <xref:System.Net.Security.ProtectionLevel> 值，指定用于传播上下文信息的 SOAP 标头的所需保护级别。  默认值是 `Sign`。|  
|hostnameComparisonMode|指定用于分析 URI 的 HTTP 主机名比较模式。 此属性的类型为 <xref:System.ServiceModel.HostNameComparisonMode>，指示在对 URI 进行匹配时，是否使用主机名来访问服务。 默认值为 <xref:System.ServiceModel.HostNameComparisonMode.StrongWildcard>，表示忽略匹配项中的主机名。|  
|maxBufferPoolSize|一个整数，指定此绑定的最大缓冲池大小。 默认值为 524,288 字节 (512 * 1024)。 Windows Communication Foundation (WCF) 的许多部件使用缓冲区。 每次使用缓冲区时，创建和销毁它们都将占用大量资源，而缓冲区的垃圾回收过程也是如此。 利用缓冲池，可以从缓冲池中获得缓冲区，使用缓冲区，然后在完成工作后将其返回给缓冲池。 这样就避免了创建和销毁缓冲区的系统开销。|  
|maxReceivedMessageSize|一个正整数，指定采用此绑定配置的通道上可以接收的最大消息大小（字节），包括消息头。 如果消息超出此限制，则发送方将收到 SOAP 错误。 接收方将删除该消息，并在跟踪日志中创建事件项。 默认值为 65536。|  
|messageEncoding|定义用于对消息进行编码的编码器。 有效值包括以下值：<br /><br /> -Text：使用文本消息编码器。<br />-Mtom：使用消息传输组织机制 1.0 (MTOM) 编码器。<br />-默认值为 Text。<br /><br /> 此属性的类型为 <xref:System.ServiceModel.WSMessageEncoding>。|  
|name|一个包含绑定的配置名称的字符串。 因为此值用作绑定的标识，所以它应该是唯一的。 从 .NET Framework 4 开始，绑定和行为不需要具有名称。 有关默认配置和无值绑定和行为的详细信息，请参阅[WCF 服务的](../../../wcf/samples/simplified-configuration-for-wcf-services.md)[简化配置](../../../wcf/simplified-configuration.md)和简化配置。|  
|openTimeout|一个 <xref:System.TimeSpan> 值，指定为完成打开操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|proxyAddress|一个指定 HTTP 代理的地址的 URI。 如果 `useSystemWebProxy` 为 `true`，则此设置必须为 `null`。 默认为 `null`。|  
|receiveTimeout|一个 <xref:System.TimeSpan> 值，指定为完成接收操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|sendTimeout|一个 <xref:System.TimeSpan> 值，指定为完成发送操作提供的时间间隔。 此值应大于或等于 <xref:System.TimeSpan.Zero>。 默认值为 00:01:00。|  
|textEncoding|指定要用来在绑定上发出消息的字符集编码。 有效值包括以下值：<br /><br /> -UnicodeFffeTextEncoding： Unicode BigEndian 编码。<br />-Utf16TextEncoding：16位编码。<br />-Utf8TextEncoding：8位编码。<br /><br /> 默认值为 Utf8TextEncoding。<br /><br /> 此属性的类型为 <xref:System.Text.Encoding>。|  
|transactionFlow|一个布尔值，指定绑定是否支持流动 WS-Transactions。 默认为 `false`。|  
|useDefaultWebProxy|一个布尔值，指定是否使用系统的自动配置 HTTP 代理。 默认为 `true`。|  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<security>](security-of-wshttpbinding.md)|定义绑定的安全设置。 此元素的类型为 <xref:System.ServiceModel.Configuration.WSHttpSecurityElement>。|  
|[\<readerQuotas>](/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|定义可由采用此绑定配置的终结点进行处理的 SOAP 消息的复杂性约束。 此元素的类型为 <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement>。|  
|[\<reliableSession>](/previous-versions/ms731375(v=vs.90))|指定是否在通道终结点之间建立可靠会话。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<bindings>](bindings.md)|此元素包含标准绑定和自定义绑定的集合。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.WSHttpBinding>
- <xref:System.ServiceModel.WSHttpContextBinding>
- <xref:System.ServiceModel.Configuration.WSHttpContextBindingElement>
- <xref:System.ServiceModel.Channels.ContextBindingElement>
- [绑定](../../../wcf/bindings.md)
- [配置系统提供的绑定](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [使用绑定配置服务和客户端](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
- [\<wsHttpBinding>](wshttpbinding.md)
