---
title: 可插入协议简介
description: 了解可插入协议，该协议支持开发使用 Internet 资源的应用程序（不考虑资源使用的协议详细信息）。
ms.date: 03/30/2017
helpviewer_keywords:
- data requests, pluggable protocols
- WebRequest class, pluggable protocols
- response to Internet request, pluggable protocols
- URI
- Windows Sockets
- request/response model
- sending data, pluggable protocols
- pluggable protocols
- WebClient class, about WebClient class
- pluggable protocols, about pluggable protocols
- Internet, pluggable protocols
- path identifiers
- Uniform Resource Identifier
- application development [.NET Framework], pluggable protocols
- requesting data from Internet, pluggable protocols
- receiving data, pluggable protocols
- protocols, pluggable
- server identifiers
- scheme identifiers
ms.assetid: 4b48e22d-e4e5-48f0-be80-d549bda97415
ms.openlocfilehash: 5b61497963e785ffe8e4c04db5b54dfb2500eed8
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96279631"
---
# <a name="introducing-pluggable-protocols"></a>可插入协议简介

Microsoft .NET Framework 为 Internet 服务提供了一种分层、可扩展且托管的实现，可以快速、轻松地将其集成到应用程序中。 <xref:System.Net> 和 <xref:System.Net.Sockets> 命名空间的 Internet 访问类可用于实现基于 Web 和基于 Internet 的应用程序。  
  
## <a name="internet-applications"></a>Internet 应用程序  

 Internet 应用程序大致分为两类：请求信息的客户端应用程序和响应客户端的信息请求的服务器应用程序。 经典的 Internet 客户端服务器应用程序是万维网，人们通过万维网，使用浏览器访问文档和存储于全球 Web 服务器中的其他数据。  
  
 应用程序不仅仅局限于其中一种角色；例如，熟悉的中间层应用程序服务器通过从其他服务器请求数据响应客户端请求，在此情况下，它同时充当服务器和客户端。  
  
 客户端应用程序通过标识请求的 Internet 资源和用于请求和响应的通信协议发出请求。 如有必要，客户端还提供完成请求所需的任何其他数据，例如代理位置或身份验证信息（用户名、密码等）。 形成请求后，可将请求发送到服务器。  
  
## <a name="identifying-resources"></a>标识资源  

 .NET Framework 使用统一资源标识符 (URI) 标识请求的 Internet 资源和通信协议。 URI 由至少三个或者四个片段组成：方案标识符，用于标识请求和响应的通信协议；服务器标识符，其由唯一标识 Internet 上服务器的域名系统 (DNS) 主机名或 TCP 地址组成；路径标识符，其在服务器上查找请求的信息；可选的查询字符串，其将信息从客户端传递到服务器。 例如，URI `http://www.contoso.com/whatsnew.aspx?date=today` 由方案标识符 `http`、服务器标识符 `www.contoso.com`、路径 `/whatsnew.aspx` 和查询字符串 `?date=today` 组成。  
  
 服务器收到请求并处理响应后，会将这些响应返回到客户端应用程序。 响应包括补充信息，如内容的类型（例如，原始文本或 XML 数据）。  
  
## <a name="requests-and-responses-in-the-net-framework"></a>.NET Framework 的请求和响应  

 .NET Framework 使用特定的类来提供通过请求/响应模型访问 Internet 资源所需的三个信息：<xref:System.Uri> 类，其中包含你要寻找的 Internet 资源的 URI；<xref:System.Net.WebRequest> 类，其中包含对资源的请求；<xref:System.Net.WebResponse> 类，它为传入的响应提供一个容器。  
  
 客户端应用程序通过将网络资源的 URI 传递到 <xref:System.Net.WebRequest.Create%2A> 方法创建 `WebRequest` 实例。 此静态方法为 HTTP 等特定协议创建 `WebRequest`。 返回的 `WebRequest` 提供对属性的访问权限，这些属性可同时控制对服务器的请求和对发出请求时发送的数据流的访问。 `WebRequest` 的 <xref:System.Net.WebRequest.GetResponse%2A> 方法将请求从客户端应用程序发送到 URI 中标识的服务器。 在响应可能会延迟的情况下，可使用“WebRequest”的 <xref:System.Net.WebRequest.BeginGetResponse%2A> 方法进行异步请求，并且可使用 <xref:System.Net.WebRequest.EndGetResponse%2A> 方法稍后返回响应。  
  
 “GetResponse”和“EndGetResponse”方法返回“WebResponse”提供对由服务器返回的数据的访问  。 因为此数据提供给请求的应用程序的流的形式 <xref:System.Net.WebResponse.GetResponseStream%2A> 方法，可在任何位置使用数据流的应用程序。  
  
 “WebRequest”和“WebResponse”类是可插入协议的基础，实施网络服务可帮助你开发使用 Internet 资源的应用程序，无需担心每个资源使用的协议的具体细节 。 “WebRequest”的子类向“WebRequest”类注册，以管理连接 Internet 资源的细节 。  
  
 例如，<xref:System.Net.HttpWebRequest> 类管理使用 HTTP 连接到 Internet 资源的细节。 默认情况下，当 WebRequest.Create 方法遇到以“http:”或“https:”（用于 HTTP 和安全 HTTP 的协议标识符）开头的 URI 时，返回的 WebRequest 可以按原样使用，也可以类型转换为 HttpWebRequest 以访问特定于协议的属性  。 在大多数情况下，WebRequest 提供发出请求所需的所有信息。  
  
 任何可以表示为请求/响应事务的协议都可用于 WebRequest 。 可以从 WebRequest 和 WebResponse 派生特定于协议的类，然后通过静态 <xref:System.Net.WebRequest.RegisterPrefix%2A?displayProperty=nameWithType> 方法注册它们以供应用程序使用 。  
  
 当需要 Internet 请求的客户端授权时，WebRequest 的 <xref:System.Net.WebRequest.Credentials%2A> 属性提供必需的凭据。 这些凭据可以是用于基本 HTTP 或摘要身份验证的简单名称/密码对，也可以是为 NTLM 或 Kerberos 身份验证设置的名称/密码/域。 凭据集可以存储在 <xref:System.Net.NetworkCredential> 实例中，或者可以在 <xref:System.Net.CredentialCache> 实例中同时存储多个集。 CredentialCache 使用请求的 URI 和服务器支持的身份验证方案来确定要发送到服务器的凭据。  
  
## <a name="simple-requests-with-webclient"></a>WebClient 的简单请求  

 对于需要对 Internet 资源发出简单请求的应用程序，<xref:System.Net.WebClient> 类提供用于将数据上传到 Internet 服务器或从 Internet 服务器下载数据的常用方法。 WebClient 依赖于 WebRequest 类为 Internet 资源提供访问权限；因此，WebClient 类可使用任何已注册的可插入协议  。  
  
 对于无法使用请求/响应模型的应用程序，或者需要侦听网络以及发送请求的应用程序，System.Net.Sockets 命名空间提供 <xref:System.Net.Sockets.TcpClient>、<xref:System.Net.Sockets.TcpListener> 和 <xref:System.Net.Sockets.UdpClient> 类。 这些类处理使用不同传输协议建立连接的细节，并将应用程序的网络连接公开为流。  
  
 熟悉 Windows Sockets 接口的开发人员或需要通过套接字级编程提供的控件的开发人员将发现 System.Net.Sockets 类能够满足他们的需求。 System.Net.Sockets 类是从托管到 System.Net 类中的本机代码的转换点 。 大多数情况下，System.Net.Sockets 类将数据封送到 Windows 32 位对应项，并处理任何必要的安全检查。  
  
## <a name="see-also"></a>请参阅

- [对可插入协议进行编程](programming-pluggable-protocols.md)
- [.NET Framework 中的网络编程](index.md)
- [网络编程示例](network-programming-samples.md)
