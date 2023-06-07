---
title: 了解 WebRequest 问题和异常
description: WebRequest 和派生类会抛出异常，以指示异常情况。 使用这些可能的解决方案来解决 .NET Framework 中的这些情况。
ms.date: 03/30/2017
ms.assetid: 74a361a5-e912-42d3-8f2e-8e9a96880a2b
ms.openlocfilehash: 27fde2a3cf3e6a3469a47bdd9efe70d31620777d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96236320"
---
# <a name="understanding-webrequest-problems-and-exceptions"></a>了解 WebRequest 问题和异常

<xref:System.Net.WebRequest> 及其派生类（<xref:System.Net.HttpWebRequest>、<xref:System.Net.FtpWebRequest> 和 <xref:System.Net.FileWebRequest>）引发异常以指示异常状态。 有时这些问题的解决方法并不明显。  
  
## <a name="solutions"></a>解决方案  

 检查 <xref:System.Net.WebException> 的 <xref:System.Net.WebException.Status%2A> 属性以确定问题。 下表展示了几种状态值和某些可能的解决方法。  
  
|状态|详细信息|解决方案|  
|------------|-------------|--------------|  
|<xref:System.Net.WebExceptionStatus.SendFailure><br /><br /> \- 或 -<br /><br /> <xref:System.Net.WebExceptionStatus.ReceiveFailure>|基础套接字有问题。 可能已重置连接。|重新连接并重新发送该请求。<br /><br /> 确保安装了最新服务包。<br /><br /> 增大 <xref:System.Net.ServicePointManager.MaxServicePointIdleTime%2A?displayProperty=nameWithType> 属性的值。<br /><br /> 将 <xref:System.Net.HttpWebRequest.KeepAlive%2A?displayProperty=nameWithType> 设置为 `false`。<br /><br /> 增加与 <xref:System.Net.ServicePointManager.DefaultConnectionLimit%2A> 属性的最大连接数。<br /><br /> 检查代理配置。<br /><br /> 如果使用 SSL，请确保服务器进程有权访问证书存储。<br /><br /> 如果发送大量数据，请将 <xref:System.Net.HttpWebRequest.AllowWriteStreamBuffering%2A> 设置为 `false`。|  
|<xref:System.Net.WebExceptionStatus.TrustFailure>|无法验证服务器证书。|尝试使用 Internet Explorer 打开 URI。 解决 IE 显示的任何安全警报。 如果无法解决安全警报，则可创建一个证书策略类，该类执行返回 `true` 的 <xref:System.Net.ICertificatePolicy>，并将其传递给 <xref:System.Net.ServicePointManager.CertificatePolicy%2A>。<br /><br /> 请参阅 <https://support.microsoft.com/?id=823177>。<br /><br /> 确保已将签署服务器证书的证书颁发机构的证书添加到 Internet Explorer 的“受信任的证书颁发机构”列表。<br /><br /> 确保 URL 中的主机名与服务器证书上的公用名称相匹配。|  
|<xref:System.Net.WebExceptionStatus.SecureChannelFailure>|SSL 事务中出现错误，或有证书问题。|.NET Framework 1.1 版仅支持 SSL 3.0 版。 如果服务器仅使用 TLS 1.0 版或 SSL 2.0 版，则会引发异常。 升级到 .NET Framework 2.0 版，并设置 <xref:System.Net.ServicePointManager.SecurityProtocol%2A> 以匹配服务器。<br /><br /> 客户端证书由服务器不信任的证书颁发机构 (CA) 签署。 在服务器上安装 CA 证书。 请参阅 <https://support.microsoft.com/?id=332077>。<br /><br /> 确保已安装最新服务包。|  
|<xref:System.Net.WebExceptionStatus.ConnectFailure>|连接失败。|防火墙或代理正在阻止连接。 修改防火墙或代理以允许连接。<br /><br /> 通过调用 <xref:System.Net.WebProxy> 构造函数在客户端应用程序中显式指定 <xref:System.Net.WebProxy> (`WebServiceProxyClass.Proxy = new WebProxy("http://server:80", true)`)。<br /><br /> 运行 Filemon 或 Regmon 以确保工作进程标识具有访问 WSPWSP.dll、HKLM\System\CurrentControlSet\Services\DnsCache 或 HKLM\System\CurrentControlSet\Services\WinSock2 的必要权限。|  
|<xref:System.Net.WebExceptionStatus.NameResolutionFailure>|域名服务无法解析主机名。|正确配置代理。 请参阅 <https://support.microsoft.com/?id=318140>。<br /><br /> 确保任何已安装的防病毒软件或防火墙未阻止连接。|  
|<xref:System.Net.WebExceptionStatus.RequestCanceled>|已调用 <xref:System.Net.WebRequest.Abort%2A>，或出现错误。|此问题可能是由于客户端或服务器上负载过大引起的。 请减小负载。<br /><br /> 增大 <xref:System.Net.ServicePointManager.DefaultConnectionLimit%2A> 设置。<br /><br /> 请参阅 <https://support.microsoft.com/?id=821268> 以修改 Web 服务性能设置。|  
|<xref:System.Net.WebExceptionStatus.ConnectionClosed>|应用程序尝试写入已关闭的套接字。|客户端或服务器重载。 请减小负载。<br /><br /> 增大 <xref:System.Net.ServicePointManager.DefaultConnectionLimit%2A> 设置。<br /><br /> 请参阅 <https://support.microsoft.com/?id=821268> 以修改 Web 服务性能设置。|  
|<xref:System.Net.WebExceptionStatus.MessageLengthLimitExceeded>|已超出对消息长度设置的限制 (<xref:System.Net.HttpWebRequest.MaximumResponseHeadersLength%2A>)。|增大 <xref:System.Net.HttpWebRequest.MaximumResponseHeadersLength%2A> 属性的值。|  
|<xref:System.Net.WebExceptionStatus.ProxyNameResolutionFailure>|域名服务无法解析代理主机名。|正确配置代理。 请参阅 <https://support.microsoft.com/?id=318140>。<br /><br /> 将 <xref:System.Net.HttpWebRequest.Proxy%2A> 属性设置为 `null`，强制 <xref:System.Net.HttpWebRequest> 不使用任何代理。|  
|<xref:System.Net.WebExceptionStatus.ServerProtocolViolation>|来自服务器的响应不是有效的 HTTP 响应。 .NET Framework 检测到服务器响应不符合 HTTP 1.1 RFC 时，会出现此问题。 如果响应包含错误标头或标头分隔符时，可能会出现此问题。RFC 2616 定义 HTTP 1.1 和来自服务器响应的有效格式。 有关详细信息，请参阅 [Internet 工程任务组 (IETF)](https://www.ietf.org/)网站上的 [RFC 2616 - 超文本传输协议 -- HTTP/1.1](https://tools.ietf.org/html/rfc2616)。|获取事务网络跟踪并检查响应中的标头。<br /><br /> 如果应用程序需要服务器响应，而无需解析（这可能是一个安全问题），请在配置文件中将 `useUnsafeHeaderParsing` 设置为 `true`。 请参阅 [\<httpWebRequest> 元素（网络设置）](../configure-apps/file-schema/network/httpwebrequest-element-network-settings.md)。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.Net.HttpWebRequest>
- <xref:System.Net.HttpWebResponse>
- <xref:System.Net.Dns>
