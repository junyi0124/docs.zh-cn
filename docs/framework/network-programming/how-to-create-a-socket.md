---
title: 如何：创建套接字
description: 了解如何初始化套接字以与远程设备通信。 使用 Socket 类指定地址系列、套接字类型和协议类型。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- application protocols, sockets
- Networking
- sending data, sockets
- data requests, sockets
- requesting data from Internet, sockets
- Socket class, creating sockets
- Network Resources
- receiving data, sockets
- protocols, sockets
- Internet, sockets
- sockets, creating
ms.assetid: c64a049c-5981-43bc-a2dc-1851473589c7
ms.openlocfilehash: 9746b814188a4dc92463399542a6044501d0da12
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96287392"
---
# <a name="how-to-create-a-socket"></a>如何：创建套接字

必须先使用协议和网络地址信息初始化套接字，然后才能使用套接字与远程设备进行通信。 <xref:System.Net.Sockets.Socket> 类的构造函数包含的参数可以指定地址系列、套接字类型，以及套接字用于建立连接的协议类型。  
  
## <a name="example"></a>示例  

 下例创建的套接字可用于与基于 TCP/IP 的网络（例如 Internet）进行通信。  
  
```csharp  
Socket s = new Socket(AddressFamily.InterNetwork,
   SocketType.Stream, ProtocolType.Tcp);  
```  
  
```vb  
Dim s as New Socket(AddressFamily.InterNetwork, _  
   SocketType.Stream, ProtocolType.Tcp)  
```  
  
 使用 UDP 而不是 TCP更改协议类型，如以下示例所示：  
  
```csharp  
Socket s = new Socket(AddressFamily.InterNetwork,
   SocketType.Dgram, ProtocolType.Udp);  
```  
  
```vb  
Dim s as New Socket(AddressFamily.InterNetwork, _  
   SocketType.Dgram, ProtocolType.Udp)  
```  
  
 <xref:System.Net.Sockets.AddressFamily> 枚举指定 Socket 类用其解析网络地址的标准地址系列（例如，AddressFamily.InterNetwork 成员指定 IP 版本 4 地址系列） 。  
  
 <xref:System.Net.Sockets.SocketType> 枚举指定套接字的类型（例如，SocketType.Stream 成员指定用流控制来发送和接收数据的标准套接字）。  
  
 <xref:System.Net.Sockets.ProtocolType> 枚举指定通信时套接字使用的网络协议（例如：ProtocolType.Tcp 表示套接字使用 TCP；ProtocolType.Udp 表示套接字使用 UDP）  。  
  
 套接字创建完成后，可启动与远程终结点的连接或接收来自远程设备的连接。  
  
## <a name="see-also"></a>请参阅

- [使用客户端套接字](using-client-sockets.md)
- [使用套接字侦听](listening-with-sockets.md)
