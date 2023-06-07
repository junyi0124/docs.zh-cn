---
title: 核心通信： HTTP-HTTPS 传输通道
ms.date: 03/30/2017
ms.assetid: 6c0a23c9-a663-461c-bdab-58b4d3e23642
ms.openlocfilehash: 5d33d153c6c527398b035ad9d027593a0fefd0e8
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96277408"
---
# <a name="core-communications-httphttps-transport-channels"></a>核心通信：HTTP/HTTPS 传输通道

本主题列出了 Windows Communication Foundation (WCF) 传输 HTTP/HTTPS 通道生成的所有异常。  
  
## <a name="exception-list"></a>异常列表  
  
|资源代码|资源字符串|  
|-------------------|---------------------|  
|DigestExplicitCredsImpersonationLevel|指定的模拟级别已指定。 当与显式凭据一起使用时，HTTP Digest 身份验证只支持“模拟”级别。|  
|FramingContentTypeMismatch|指定的服务不支持指定的内容类型。 客户端和服务绑定可能不匹配。|  
|Hosting_SslSettingsMisconfigured|指定服务的安全套接字层设置与 Internet 信息服务中的相应设置不匹配。|  
|HttpAuthSchemeAndClientCert|已将 HTTPS 侦听器工厂配置为需要客户端证书和指定的身份验证方案。 但是，一次只能需要一种形式的客户端身份验证。|  
|HttpReceiveFailure|接收对指定服务终结点的 HTTP 响应时发生错误。 服务终结点绑定可能未使用 HTTP 协议。 另一种可能性是因为关闭了服务，所以服务器终止了 HTTP 请求上下文。 有关详细信息，请参见服务器日志。|  
|HttpRegistrationAccessDenied|HTTP 无法注册指定的 URL。 你的进程不具有访问此命名空间的权限 (参阅 [命名空间保留、注册和路由](/windows/desktop/http/namespace-reservations-registrations-and-routing) ，详细信息) 。|  
|HttpRegistrationAlreadyExists|HTTP 无法注册指定的 URL。 另一应用程序已经向 HTTP.SYS 注册了此 URL。|  
|HttpRegistrationPortInUse|HTTP 无法注册指定的 URL，因为另一应用程序正在使用指定 TCP 端口。|  
|HttpSendFailure|向指定的服务终结点发出 HTTP 请求时发生错误。 请确保原因不是安全绑定不匹配。 还要确保该服务的配置不是针对安全套接字层进行的。|  
|MessageXmlProtocolError|从网络接收到的 XML 存在问题。 有关详细信息，请参见内部异常。|  
|MissingContentType|接收方返回一个错误，该错误指示对指定服务终结点发出的请求缺少内容类型。 有关更多信息，请参见内部异常。|  
|ProxyAuthenticationLevelMismatch|HTTP 代理身份验证凭据所指定的相互身份验证需求比对目标服务器身份验证的需求更严格。|  
|ProxyImpersonationLevelMismatch|HTTP 代理身份验证凭据所指定的模拟级别限制比对目标服务器身份验证的限制更严格。|  
|SecureChannelFailure|无法使用指定的颁发机构为安全套接字层/传输层安全建立安全通道。|  
|TrustFailure|无法使用指定的颁发机构为安全套接字层/传输层安全的安全通道建立信任关系。|  
|UseDefaultWebProxyCantBeUsedWithExplicitProxyAddress|在 HttpTransportBinding 元素中，不能同时指定显式代理地址和 UseDefaultWebProxy=true。|
