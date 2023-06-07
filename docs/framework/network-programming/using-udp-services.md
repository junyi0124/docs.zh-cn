---
title: 使用 UDP 服务
description: UdpClient 类摘录详细信息，以在 .NET Framework 中创建套接字来使用 UDP 请求和接收数据。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- protocols, UDP
- network resources, UDP
- requesting data from Internet, UDP
- UDP
- receiving data, UDP
- Internet, UDP
- broadcasting messages to multiple addresses
- data requests, UDP
- UdpClient class, about UdpClient class
- sending data, UDP
- application protocols, UDP
ms.assetid: d5c3477a-e798-454c-a890-738ba14c5707
ms.openlocfilehash: 4c2e88492f737800151d30097719d7d011054a5e
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/08/2020
ms.locfileid: "84501945"
---
# <a name="use-udp-services"></a>使用 UDP 服务

<xref:System.Net.Sockets.UdpClient> 类使用 UDP与网络服务通信。 <xref:System.Net.Sockets.UdpClient> 类的属性和方法概要说明了使用 UDP 创建 <xref:System.Net.Sockets.Socket> 以请求和接收数据的详情。

用户数据报协议 (UDP) 是一种简单协议，非常适合用于将数据传递到远程主机。 但由于 UDP 协议是一种无连接协议，因此发送到远程终结点的 UDP 数据报不一定可到达，也无法保证其能以发送的相同顺序到达。 使用 UDP 的应用程序必须准备好处理丢失的、重复的和乱序的数据报。

要使用 UDP 发送数据报，必须知道承载所需服务的网络设备的网络地址以及该服务用来通信的 UDP 端口号。 Internet 编号分配机构 (IANA) 定义公共服务的端口号（请参阅[服务名称和传输协议端口号注册表](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)）。 不在 IANA 列表上的服务可使用 1,024 到 65,535 范围内的端口号。

特殊网络地址用于支持基于 IP 的网络上的 UDP 广播消息。 下面的讨论以 Internet 上使用的 IP 版本 4 地址系列作为示例。

IP 版本 4 地址使用 32 位指定网络地址。 对于使用 255.255.255.0 网络掩码的 C 类地址，这些数位被分为四个八进制数。 当以十进制数表示时，这四个八进制数构成我们熟悉的以点分隔的四部分表示法，如 192.168.100.2。 前两个八进制数（此示例中的 192.168）构成网络号码，第三个八进制数 (100) 定义子网，最后一个八进制数 (2) 则是主机标识符。

将 IP 地址的所有数位均设置为同一个（即 255.255.255.255），可构成有限的广播地址。 将 UDP 数据报发送到此地址会将消息传递到本地网络段上的任何主机。 由于路由器不会转发发送到此地址的消息，因此只有网络段上的主机会接收到广播消息。

通过设置主机标识符的所有数位，可以将广播定向到网络的特定部分。 例如，若要将广播发送到以 192.168.1 开头的 IP 地址标识的网络上的所有主机，请使用地址 192.168.1.255。

下面的示例代码使用 <xref:System.Net.Sockets.UdpClient> 侦听端口 11,000 上的 UDP 数据报。 客户端将接收消息字符串并将消息写入控制台。

```vb
Imports System.Net
Imports System.Net.Sockets
Imports System.Text

Public Class UDPListener
   Private Const listenPort As Integer = 11000

   Private Shared Sub StartListener()
      Dim listener As New UdpClient(listenPort)
      Dim groupEP As New IPEndPoint(IPAddress.Any, listenPort)
      Try
         While True
            Console.WriteLine("Waiting for broadcast")
            Dim bytes As Byte() = listener.Receive(groupEP)
            Console.WriteLine("Received broadcast from {0} :", groupEP)
            Console.WriteLine(Encoding.ASCII.GetString(bytes, 0, bytes.Length))
         End While
      Catch e As SocketException
         Console.WriteLine(e)
      Finally
         listener.Close()
      End Try
   End Sub 'StartListener

   Public Shared Sub Main()
      StartListener()
   End Sub 'Main
End Class 'UDPListener
```

```csharp
using System;
using System.Net;
using System.Net.Sockets;
using System.Text;

public class UDPListener
{
    private const int listenPort = 11000;

    private static void StartListener()
    {
        UdpClient listener = new UdpClient(listenPort);
        IPEndPoint groupEP = new IPEndPoint(IPAddress.Any, listenPort);

        try
        {
            while (true)
            {
                Console.WriteLine("Waiting for broadcast");
                byte[] bytes = listener.Receive(ref groupEP);

                Console.WriteLine($"Received broadcast from {groupEP} :");
                Console.WriteLine($" {Encoding.ASCII.GetString(bytes, 0, bytes.Length)}");
            }
        }
        catch (SocketException e)
        {
            Console.WriteLine(e);
        }
        finally
        {
            listener.Close();
        }
    }

    public static void Main()
    {
        StartListener();
    }
}
```

下面的代码示例使用 <xref:System.Net.Sockets.Socket> 将 UDP 数据报用端口 11,000 发送到定向广播地址 192.168.1.255。 客户端将发送在命令行上指定的消息字符串。

```vb
Imports System.Net
Imports System.Net.Sockets
Imports System.Text

Public Class Program
    Public Shared Sub Main(args() As [String])
      Dim s As New Socket(AddressFamily.InterNetwork, SocketType.Dgram, ProtocolType.Udp)
      Dim broadcast As IPAddress = IPAddress.Parse("192.168.1.255")
      Dim sendbuf As Byte() = Encoding.ASCII.GetBytes(args(0))
      Dim ep As New IPEndPoint(broadcast, 11000)
      s.SendTo(sendbuf, ep)
      Console.WriteLine("Message sent to the broadcast address")
   End Sub 'Main
End Class
```

```csharp
using System;
using System.Net;
using System.Net.Sockets;
using System.Text;

class Program
{
    static void Main(string[] args)
    {
        Socket s = new Socket(AddressFamily.InterNetwork, SocketType.Dgram, ProtocolType.Udp);

        IPAddress broadcast = IPAddress.Parse("192.168.1.255");

        byte[] sendbuf = Encoding.ASCII.GetBytes(args[0]);
        IPEndPoint ep = new IPEndPoint(broadcast, 11000);

        s.SendTo(sendbuf, ep);

        Console.WriteLine("Message sent to the broadcast address");
    }
}
```

## <a name="see-also"></a>请参阅

- <xref:System.Net.Sockets.UdpClient>
- <xref:System.Net.IPAddress>
