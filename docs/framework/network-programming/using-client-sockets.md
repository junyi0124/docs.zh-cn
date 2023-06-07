---
title: 使用客户端套接字
description: 此示例演示如何使用 .NET Framework 中的套接字创建到远程服务的 TCP/IP 连接。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- application protocols, sockets
- sending data, sockets
- data requests, sockets
- requesting data from Internet, sockets
- receiving data, sockets
- Socket class, client sockets
- protocols, sockets
- Internet, sockets
- sockets, client sockets
- client sockets
ms.assetid: 81de9f59-8177-4d98-b25d-43fc32a98383
ms.openlocfilehash: 6982d09c20cd0d7e9d27fc63880b39e0982c86ef
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96265188"
---
# <a name="using-client-sockets"></a>使用客户端套接字

在通过 <xref:System.Net.Sockets.Socket> 发起对话之前，必须在应用程序和远程设备之间创建数据管道。 尽管存在其他网络地址系列和协议，但本示例说明如何创建与远程服务的 TCP/IP 连接。  
  
 TCP/IP 使用一个网络地址和一个服务端口号来对唯一标识设备。 网络地址标识网络上的特定设备；端口号标识该设备要连接到的特定服务。 网络地址和服务端口的组合称为终结点，它在 .NET Framework 中由 <xref:System.Net.EndPoint> 类表示。 会为每个受支持的地址系列定义 EndPoint 的后代；对于 IP 地址系列，类为 <xref:System.Net.IPEndPoint>。  
  
 <xref:System.Net.Dns> 类向使用 TCP/IP Internet 服务的应用程序提供域名服务。 <xref:System.Net.Dns.Resolve%2A> 方法查询 DNS 服务器以将用户友好的域名（如“host.contoso.com”）映射到数字形式的 Internet 地址（如 192.168.1.1）。 Resolve 返回一个 <xref:System.Net.IPHostEntry>，其包含所请求名称的地址和别名的列表。 在大多数情况下，可以使用 <xref:System.Net.IPHostEntry.AddressList%2A> 数组中返回的第一个地址。 下面的代码获取一个包含服务器 host.contoso.com 的 IP 地址的 <xref:System.Net.IPAddress>。  
  
```vb  
Dim ipHostInfo As IPHostEntry = Dns.Resolve("host.contoso.com")  
Dim ipAddress As IPAddress = ipHostInfo.AddressList(0)  
```  
  
```csharp  
IPHostEntry ipHostInfo = Dns.Resolve("host.contoso.com");  
IPAddress ipAddress = ipHostInfo.AddressList[0];  
```  
  
 Internet 编号分配机构 (IANA) 定义公共服务的端口号；有关详细信息，请参阅[服务名称和传输协议端口号注册表](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)。 其他服务可具有 1,024 到 65,535 范围内的注册端口号。 以下代码将 host.contoso.com 的 IP 地址与端口号组合，为连接创建远程终结点。  
  
```vb  
Dim ipe As New IPEndPoint(ipAddress, 11000)  
```  
  
```csharp  
IPEndPoint ipe = new IPEndPoint(ipAddress,11000);  
```  
  
 确定远程设备的地址并选择要用于连接的端口后，应用程序便可以尝试建立与远程设备的连接。 下面的示例使用现有的 IPEndPoint 连接到远程设备，并捕获引发的任何异常。  
  
```vb  
Try  
    s.Connect(ipe)  
Catch ae As ArgumentNullException  
    Console.WriteLine("ArgumentNullException : {0}", _  
        ae.ToString())  
Catch se As SocketException  
    Console.WriteLine("SocketException : {0}", se.ToString())  
Catch e As Exception  
    Console.WriteLine("Unexpected exception : {0}", e.ToString())  
End Try  
```  
  
```csharp  
try {  
    s.Connect(ipe);  
} catch(ArgumentNullException ae) {  
    Console.WriteLine("ArgumentNullException : {0}", ae.ToString());  
} catch(SocketException se) {  
    Console.WriteLine("SocketException : {0}", se.ToString());  
} catch(Exception e) {  
    Console.WriteLine("Unexpected exception : {0}", e.ToString());  
}  
```  
  
## <a name="see-also"></a>请参阅

- [使用同步客户端套接字](using-a-synchronous-client-socket.md)
- [使用异步客户端套接字](using-an-asynchronous-client-socket.md)
- [如何：创建套接字](how-to-create-a-socket.md)
- [套接字](sockets.md)
