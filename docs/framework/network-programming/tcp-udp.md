---
title: TCP-UDP
description: 了解 TcpClient、TcpListener 和 UdpClient 类如何处理 TCP 和 UDP 服务，这些服务负责处理 .NET Framework 中数据传输的细节。
ms.date: 03/30/2017
helpviewer_keywords:
- protocols, TCP/UDP
- network resources, TCP/UDP
- sending data, TCP/UDP
- TCP/UDP
- TcpClient class, about TcpClient class
- application protocols, TCP/UDP
- receiving data, TCP/UDP
- TcpListener class, about TcpListener class
- Socket class, about Socket class
- UDP
- data requests, TCP/UDP
- requesting data from Internet, TCP/UDP
- Internet, TCP/UDP
ms.assetid: df29b4b0-49e8-4923-82b9-13150dfc40f5
ms.openlocfilehash: b5b8b5cb3ce38fcff9115b2f9acf23fc5a970adf
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96239441"
---
# <a name="tcp-udp"></a>TCP-UDP

应用程序可将传输控制协议 (TCP) 及用户数据报协议 (UDP) 服务用于 <xref:System.Net.Sockets.TcpClient>、<xref:System.Net.Sockets.TcpListener> 以及 <xref:System.Net.Sockets.UdpClient> 类。 这些协议类是在 <xref:System.Net.Sockets.Socket?displayProperty=nameWithType> 类的基础上建立的，并照管数据传输的详细信息。  
  
 协议类使用 Socket 类的同步方法提供简单直接的网络服务访问，没有维护状态信息的开销，也无需了解设定协议特定的套接字的详细信息。 若要使用异步 Socket 方法，可以使用 <xref:System.Net.Sockets.NetworkStream> 类提供的异步法。 若要访问未被协议类公开的 Socket 类功能，必须使用 Socket 类。   
  
 TcpClient 和 TcpListener 代表使用 NetworkStream 类的网络。   使用 <xref:System.Net.Sockets.TcpClient.GetStream%2A> 方法返回网络流，然后调用此流的 <xref:System.Net.Sockets.NetworkStream.Read%2A> 和 <xref:System.Net.Sockets.NetworkStream.Write%2A> 方法。 NetworkStream 不拥有协议类的基础套接字，因此关闭它不会影响套接字。  
  
 UdpClient 类使用字节数组来保存 UDP 数据报。 使用 <xref:System.Net.Sockets.UdpClient.Send%2A> 方法向网络发送数据，并使用 <xref:System.Net.Sockets.UdpClient.Receive%2A> 方法来接收传入的数据报。  
  
## <a name="see-also"></a>请参阅

- [使用 TCP 服务](using-tcp-services.md)
- [使用 UDP 服务](using-udp-services.md)
- [在网络上使用流](using-streams-on-the-network.md)
- [使用异步服务器套接字](using-an-asynchronous-server-socket.md)
- [使用异步客户端套接字](using-an-asynchronous-client-socket.md)
- [使用应用程序协议](using-application-protocols.md)
