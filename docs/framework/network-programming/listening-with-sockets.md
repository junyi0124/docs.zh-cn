---
title: 使用套接字侦听
description: 了解如何创建远程服务，其中服务器套接字会打开网络上的一个端口并等待客户端连接到该端口。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- application protocols, sockets
- sending data, sockets
- sockets, listening with sockets
- data requests, sockets
- requesting data from Internet, sockets
- receiving data, sockets
- protocols, sockets
- listening with sockets
- Internet, sockets
ms.assetid: 40e426cc-13db-4371-95eb-f7388bd23ebf
ms.openlocfilehash: 4249948579384ec0159ba61072126944596c8f56
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96242209"
---
# <a name="listening-with-sockets"></a>使用套接字侦听

侦听器或服务器套接字打开网络上的端口，然后等待客户端连接到该端口。 虽然存在其他网络地址系列和协议，但本示例演示如何创建 TCP/IP 网络的远程服务。  
  
 通过将主机的 IP 地址与服务的端口号组合来定义 TCP/IP 服务的唯一地址，以创建该服务的终结点。 <xref:System.Net.Dns> 类提供了返回有关本地网络设备支持的网络地址信息的方法。 如果本地网络设备有多个网络地址或本地系统支持多个网络设备，该 Dns 类将返回所有网络地址信息，并且应用程序必须为此服务选择正确的地址。 Internet 编号分配机构 (IANA) 定义公共服务的端口号；有关详细信息，请参阅 [服务名称和传输协议端口号注册表](https://www.iana.org/assignments/port-numbers)。 其他服务可具有 1,024 到 65,535 范围内的注册端口号。  
  
 通过将主机计算机的 Dns 返回的第一个 IP 地址与从已注册端口号范围内选择的端口号组合，以下示例为服务器创建 <xref:System.Net.IPEndPoint>。  
  
```vb  
Dim ipHostInfo As IPHostEntry = Dns.GetHostEntry(Dns.GetHostName())  
Dim ipAddress As IPAddress = ipHostInfo.AddressList(0)  
Dim localEndPoint As New IPEndPoint(ipAddress, 11000)  
```  
  
```csharp  
IPHostEntry ipHostInfo = Dns.GetHostEntry(Dns.GetHostName());  
IPAddress ipAddress = ipHostInfo.AddressList[0];  
IPEndPoint localEndPoint = new IPEndPoint(ipAddress, 11000);  
```  
  
 确定本地终结点后，<xref:System.Net.Sockets.Socket> 必须与使用 <xref:System.Net.Sockets.Socket.Bind%2A> 方法的终结点相关联，并且设置为在使用 <xref:System.Net.Sockets.Socket.Listen%2A> 方法的终结点上侦听。 如果特定的地址和端口组合已在使用中，绑定将引发异常。 以下示例显示了将套接字与 IPEndPoint 相关联。  
  
```vb  
Dim listener As New Socket(ipAddress.AddressFamily, _  
    SocketType.Stream, ProtocolType.Tcp)
listener.Bind(localEndPoint)  
listener.Listen(100)  
```  
  
```csharp  
Socket listener = new Socket(ipAddress.AddressFamily,
    SocketType.Stream, ProtocolType.Tcp);
listener.Bind(localEndPoint);  
listener.Listen(100);  
```  
  
 Listen 方法使用一个参数，该参数指定在服务器忙错误返回到连接客户端之前，允许的挂起套接字连接的数目。 在这种情况下，在服务器忙响应返回到客户端编号 101 前，最多 100 个客户端置于连接队列中。  
  
## <a name="see-also"></a>请参阅

- [使用同步服务器套接字](using-a-synchronous-server-socket.md)
- [使用异步服务器套接字](using-an-asynchronous-server-socket.md)
- [使用客户端套接字](using-client-sockets.md)
- [如何：创建套接字](how-to-create-a-socket.md)
- [套接字](sockets.md)
