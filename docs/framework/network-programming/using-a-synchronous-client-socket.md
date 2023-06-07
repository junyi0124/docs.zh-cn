---
title: 使用同步客户端套接字
description: 此示例演示 .NET Framework 中的同步客户端套接字，这将在网络操作完成时挂起应用程序。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- application protocols, sockets
- sending data, sockets
- data requests, sockets
- requesting data from Internet, sockets
- synchronous client sockets
- Socket class, synchronous client sockets
- receiving data, sockets
- sockets, synchronous client sockets
- protocols, sockets
- Internet, sockets
- client sockets
ms.assetid: 945d00c6-7202-466c-9df9-140b84156d43
ms.openlocfilehash: f198f283f2acfdcfbafed25baecb02a64e9d1e26
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96236307"
---
# <a name="using-a-synchronous-client-socket"></a>使用同步客户端套接字

网络操作完成过程中，同步客户端套接字会挂起应用程序。 同步套接字不适用于在操作中大量使用网络的应用程序，但它们可以为其他应用程序启用对网络服务的简单访问。  
  
 若要发送数据，请将字节数组传递到 <xref:System.Net.Sockets.Socket> 类的数据发送方法之一（<xref:System.Net.Sockets.Socket.Send%2A> 和 <xref:System.Net.Sockets.Socket.SendTo%2A>）。 下面的示例使用 <xref:System.Text.Encoding.ASCII%2A?displayProperty=nameWithType> 属性将字符串编码到字节数组缓冲区，然后使用 Send 方法将该缓冲区传输到网络设备。 Send 方法返回发送到网络设备的字节数。  
  
```vb  
Dim msg As Byte() = _  
    System.Text.Encoding.ASCII.GetBytes("This is a test.")  
Dim bytesSent As Integer = s.Send(msg)  
```  
  
```csharp  
byte[] msg = System.Text.Encoding.ASCII.GetBytes("This is a test");  
int bytesSent = s.Send(msg);  
```  
  
 Send 方法从缓冲区移除字节，并用网络接口将这些字节排队以便发送到网络设备。 网络接口可能不会立即发送数据，但它最终将发送，只要使用 <xref:System.Net.Sockets.Socket.Shutdown%2A> 方法正常关闭连接。  
  
 若要从网络设备接收数据，请将缓冲区传递到 Socket 类的数据接收方法之一（<xref:System.Net.Sockets.Socket.Receive%2A> 和 <xref:System.Net.Sockets.Socket.ReceiveFrom%2A>）。  同步套接字将挂起应用程序，直到从网络收到字节或者套接字关闭。 下面的示例接收来自网络的数据，然后将其显示在控制台上。 该示例假定来自网络的数据是用 ASCII 编码的文本。 Receive 方法返回从网络接收的字节数。  
  
```vb  
Dim bytes(1024) As Byte  
Dim bytesRec = s.Receive(bytes)  
Console.WriteLine("Echoed text = {0}", _  
    System.Text.Encoding.ASCII.GetString(bytes, 0, bytesRec))  
```  
  
```csharp  
byte[] bytes = new byte[1024];  
int bytesRec = s.Receive(bytes);  
Console.WriteLine("Echoed text = {0}",  
    System.Text.Encoding.ASCII.GetString(bytes, 0, bytesRec));  
```  
  
 不再需要套接字时需将其释放，方法是调用 <xref:System.Net.Sockets.Socket.Shutdown%2A> 方法，再调用 Close 方法。 下面的示例释放套接字。 <xref:System.Net.Sockets.SocketShutdown> 枚举定义常数以指示应该关闭套接字用于发送，还是用于接收，或者用于两者。  
  
```vb  
s.Shutdown(SocketShutdown.Both)  
s.Close()  
```  
  
```csharp  
s.Shutdown(SocketShutdown.Both);  
s.Close();  
```  
  
## <a name="see-also"></a>请参阅

- [使用异步客户端套接字](using-an-asynchronous-client-socket.md)
- [使用套接字侦听](listening-with-sockets.md)
- [同步客户端套接字示例](synchronous-client-socket-example.md)
