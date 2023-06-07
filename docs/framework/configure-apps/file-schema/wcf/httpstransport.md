---
title: <httpsTransport>
ms.date: 03/30/2017
ms.assetid: f6ed4bc0-7e38-4348-9259-30bf61eb9435
ms.openlocfilehash: 07f7b07815454983b0e8f09a5f242a12f4d4bdb3
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91192212"
---
# \<httpsTransport>

为自定义绑定指定用于传输 SOAP 消息的 HTTP 传输。  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<httpsTransport>**  
  
## <a name="syntax"></a>语法  
  
```xml  
<httpsTransport allowCookies="Boolean"
                authenticationScheme="Digest/Negotiate/Ntlm/Basic/Anonymous"
                bypassProxyOnLocal="Boolean"
                hostnameComparisonMode="StrongWildcard/Exact/WeakWildcard"
                manualAddressing="Boolean"
                maxBufferPoolSize="Integer"
                maxBufferSize="Integer"
                maxReceivedMessageSize="Integer"
                proxyAddress="Uri"
                proxyAuthenticationScheme="None/Digest/Negotiate/Ntlm/Basic/Anonymous"
                realm="String"
                requireClientCertificate="Boolean"
                transferMode="Buffered/Streamed/StreamedRequest/StreamedResponse"
                unsafeConnectionNtlmAuthentication="Boolean"
                useDefaultWebProxy="Boolean" />
```  
  
## <a name="attributes-and-elements"></a>特性和元素  

 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
  
|属性|描述|  
|---------------|-----------------|  
|allowCookies|一个布尔值，指定客户端是否接受 Cookie 并在今后的请求中传播这些 Cookie。 默认值为 `false`。<br /><br /> 在与使用 Cookie 的 ASMX Web 服务进行交互时，可以使用此属性。 通过这种方式，可以确保从服务器返回的 Cookie 自动复制到客户端今后对该服务的所有请求。|  
|authenticationScheme|指定用来验证 HTTP 侦听器正在处理的客户端请求的协议。 有效值包括以下值：<br /><br /> -Digest：指定摘要式身份验证。<br />-Negotiate：与客户端协商以确定身份验证方案。 如果客户端和服务器均支持 Kerberos，则使用 Kerberos；否则使用 NTLM。<br />-Ntlm：指定 NTLM 身份验证。<br />-Basic：指定基本身份验证。<br />-Anonymous：指定匿名身份验证。<br /><br /> 默认值为 Anonymous。 此属性的类型为 <xref:System.Net.AuthenticationSchemes>。 此属性只能设置一次。|  
|bypassProxyOnLocal|一个布尔值，指示是否对本地地址不使用代理服务器。 默认值为 `false`。<br /><br /> 本地地址是指位于本地 LAN 或 Intranet 上的地址。<br /><br /> 如果服务地址以开头，则 Windows Communication Foundation (WCF) 始终忽略代理 `http://localhost` 。<br /><br /> 如果希望客户端在与同一台计算机上的服务通话时使用代理，则应使用主机名称而非 localhost。|  
|hostnameComparisonMode|指定用于分析 URI 的 HTTP 主机名比较模式。 有效值为<br /><br /> -StrongWildcard： ( "+" ) 与指定方案、端口和相对 URI 的上下文中所有可能的主机名匹配。<br />-Exact：无通配符<br />-WeakWildcard： ( " \* " ) 匹配指定方案、端口和相对 UIR 的上下文中的所有可能的主机名，这些主机名未显式匹配或通过强通配符机制匹配。<br /><br /> 默认值为 StrongWildcard。 此属性的类型为 `System.ServiceModel.HostnameComparison`。|  
|manualAddressing|一个使用户能够控制消息寻址的布尔值。 此属性通常用于路由器方案。在该方案中，应用程序确定将消息发送到若干目标中的哪一个。<br /><br /> 如果设置为 `true`，则通道假定已对消息进行寻址，而不再向其添加其他任何信息。 然后，用户可以单独对每个消息进行寻址。<br /><br /> 如果设置为 `false`，则默认的 Windows Communication Foundation (WCF) 寻址机制将为所有消息自动创建地址。<br /><br /> 默认为 `false`。|  
|maxBufferPoolSize|一个正整数，指定缓冲池的最大大小。 默认值为 524288。<br /><br /> WCF 的许多组件使用缓冲区。 每次使用缓冲区时，创建和销毁它们都将占用大量资源，而缓冲区的垃圾回收过程也是如此。 利用缓冲池，可以从缓冲池中获得缓冲区，使用缓冲区，然后在完成工作后将其返回给缓冲池。 这样就避免了创建和销毁缓冲区的系统开销。|  
|maxBufferSize|一个正整数，指定缓冲区的最大大小。 默认值为 524288。|  
|maxReceivedMessageSize|一个正整数，指定可允许接收的最大消息大小。 默认值为 65536。|  
|proxyAddress|一个指定 HTTP 代理的地址的 URI。 如果 `useSystemWebProxy` 为 `true`，则此设置必须为 `null`。 默认为 `null`。|  
|proxyAuthenticationScheme|指定用于验证 HTTP 代理正在处理的客户端请求的协议。 有效值包括以下值：<br /><br /> -None：不执行任何身份验证。<br />-Digest：指定摘要式身份验证。<br />-Negotiate：与客户端协商以确定身份验证方案。 如果客户端和服务器均支持 Kerberos，则使用 Kerberos；否则使用 NTLM。<br />-Ntlm：指定 NTLM 身份验证。<br />-Basic：指定基本身份验证。<br />-Anonymous：指定匿名身份验证。<br /><br /> 默认值为 Anonymous。 此属性的类型为 <xref:System.Net.AuthenticationSchemes>。 请注意， <xref:System.Net.AuthenticationSchemes.IntegratedWindowsAuthentication?displayProperty=nameWithType> 不支持。|  
|realm|一个指定要在代理/服务器上使用的领域的字符串。 默认值为空字符串。<br /><br /> 服务器使用领域将受保护的资源分区。 每个分区都可以有自己的身份验证方案和/或授权数据库。 领域仅用于基本和摘要式身份验证。 在客户端成功进行身份验证之后，该身份验证对给定领域内的所有资源都有效。 有关领域的详细说明，请参阅 [IETF 网站](https://www.ietf.org)上的 RFC 2617。|  
|requireClientCertificate|一个布尔值，指定服务器是否要求客户端提供一个客户端证书作为 HTTPS 握手的一部分。 默认为 `false`。|  
|transferMode|指定对消息进行缓冲处理还是流式处理，或者指定消息是请求还是响应。 有效值包括以下值：<br /><br /> -缓冲：请求消息和响应消息进行缓冲处理。<br />-流式处理：对请求和响应消息进行流式处理。<br />-StreamedRequest：对请求消息进行流式处理，对响应消息进行缓冲处理。<br />-StreamedResponse：对请求消息进行缓冲处理，对响应消息进行流式处理。<br /><br /> 默认值为 Buffered。 此属性的类型为 <xref:System.ServiceModel.TransferMode>。|  
|unsafeConnectionNtlmAuthentication|一个布尔值，指定是否在服务器上启用不安全连接共享。 默认值为 `false`。 如果启用，将对每个 TCP 连接执行一次 NTLM 身份验证。|  
|useDefaultWebProxy|一个布尔值，指定是否使用计算机范围的代理设置，而不使用用户特定的设置。 默认为 `true`。|  
  
### <a name="child-elements"></a>子元素  

 无。  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<binding>](bindings.md)|定义自定义绑定的所有绑定功能。|  
  
## <a name="remarks"></a>备注  

 `httpsTransport` 元素是创建实现 HTTPS 传输协议的自定义绑定的起始点。 HTTPS 是用于安全互操作性用途的主要传输。 Windows Communication Foundation (WCF) 支持 HTTPS，以确保与其他 Web 服务堆栈的互操作性。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Configuration.HttpsTransportElement>
- <xref:System.ServiceModel.Channels.HttpsTransportBindingElement>
- <xref:System.ServiceModel.Channels.TransportBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [传输](../../../wcf/feature-details/transports.md)
- [选择传输方式](../../../wcf/feature-details/choosing-a-transport.md)
- [绑定](../../../wcf/bindings.md)
- [扩展绑定](../../../wcf/extending/extending-bindings.md)
- [自定义绑定](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
