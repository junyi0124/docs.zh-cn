---
title: System.Net 类的最佳实践
description: 遵循这些建议，使用 System.Net 中包含的类，在 .NET Framework 编程中发挥最大优势。
ms.date: 03/30/2017
helpviewer_keywords:
- sending data, best practices
- requesting data from Internet, best practices
- WebRequest class, best practices
- data requests, best practices
- WebResponse class, best practices
- best practices, data requests
- receiving data, best practices
ms.assetid: 716decc6-5952-47b7-9c5a-ba6fc5698684
ms.openlocfilehash: 170e8cfe0a7d7c911122dafe18b8a5081ceb3d2d
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96250770"
---
# <a name="best-practices-for-systemnet-classes"></a>System.Net 类的最佳实践

以下建议有助于你充分利用 <xref:System.Net> 中包含的类：  
  
- 对于传输层安全性 (TLS) 最佳做法，请参阅 [.NET Framework 中的传输层安全性 (TLS) 最佳做法](tls.md)。

- 要转换到子代类，请尽可能使用 <xref:System.Net.WebRequest> 和 <xref:System.Net.WebResponse>，而不是使用类。 使用 WebRequest 和 WebResponse 的应用程序，无需大范围修改代码，便可充分利用新的 Internet 协议 。  
  
- 用 System.Net 类编写在服务器上运行的 ASP.NET 应用程序时，从性能的角度来看，使用 <xref:System.Net.WebRequest.GetResponse%2A> 和 <xref:System.Net.WebResponse.GetResponseStream%2A> 异步方法要更好些。  
  
- 对 Internet 资源开放的连接数会对网络性能和吞吐量产生显著的影响。 默认情况下，System.Net 在每个主机的每个应用程序中使用两个连接。 为应用程序在 <xref:System.Net.ServicePoint> 中设置 <xref:System.Net.ServicePoint.ConnectionLimit%2A> 属性，可为特定主机增加此连接数。 设置 <xref:System.Net.ServicePointManager.DefaultPersistentConnectionLimit?displayProperty=nameWithType> 属性可为所有主机提高此默认设置。  
  
- 编写套接字级别协议时，尝试尽可能使用 <xref:System.Net.Sockets.TcpClient> 或 <xref:System.Net.Sockets.UdpClient> 编写，而不是直接写入 <xref:System.Net.Sockets.Socket> 中。 这两个客户端类封装了 TCP 和 UDP 套接字的创建，因此无需处理此连接的详细信息。  
  
- 若要访问需要凭据的站点，请使用 <xref:System.Net.CredentialCache> 类创建凭据缓存，而非为每个站点提供请求。 使用 CredentialCache 类可搜索缓存，查找符合请求的合适凭据，如此，你便无需基于 URL 创建和提供凭据。  
  
## <a name="see-also"></a>请参阅

- [.NET Framework 中的网络编程](index.md)
