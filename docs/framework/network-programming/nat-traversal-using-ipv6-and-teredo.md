---
title: 使用 IPv6 和 Teredo 的 NAT 遍历
ms.date: 03/30/2017
ms.assetid: 568cd245-3300-49ef-a995-d81bf845d961
ms.openlocfilehash: a6448ddf117e1f454338869820751ae5d9e0070e
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96258616"
---
# <a name="nat-traversal-using-ipv6-and-teredo"></a>使用 IPv6 和 Teredo 的 NAT 遍历

增强了功能：为网络地址转换 (NAT) 遍历提供支持。 这些更改旨在用于 IPv6 和 Teredo，但也同样适用于其他 IP 隧道技术。 这些增强功能会影响 <xref:System.Net> 和相关命名空间中的类。  
  
 这些更改会影响客户端和计划使用 IP 隧道技术的服务器应用程序。  
  
 支持 NAT 遍历的更改仅可用于使用 .NET Framework 版本 4 的应用程序。 这些功能不可用于 .NET Framework 的早期版本。  
  
## <a name="overview"></a>概述  

 Internet 协议版本 4 (IPv4) 将 IPv4 地址的长度定义为 32 位。 因此，IPv4 支持大约 40 亿个唯一 IP 地址（2^32）。 20 世纪 90 年代，随着 Internet 上计算机和网络设备数量的增加，IPv4 地址空间的限制变得明显起来。  
  
 有几种技术可延长 IPv4 的生存期，其中一种是通过部署 NAT，使单个唯一公共 IP 地址可以表示大量专用 IP 地址（专用 Intranet）。 NAT 设备后的多个专用 IP 地址共享一个公共 IPv4 地址。 NAT 设备可能是一个专用的硬件设备（例如，成本较低的无线接入点和路由器），或者是运行设备以便提供 NAT 的计算机。 这种公共 IP 地址的设备或服务转换公共 Internet 和专用 Intranet 之间的 IP 网络数据包。  
  
 此方案非常适用于在专用 Intranet 上运行的客户端应用程序，该应用程序将请求发送到 Internet 上的其他 IP 地址（通常为服务器）。 NAT 设备或服务器可以保留客户端请求的映射，因此响应返回后，它知道要将响应发送到什么位置。 但是，对于在 NAT 设备后的专用 Intranet 上运行且需要提供服务、侦听数据包和做出响应的应用程序，这一方案也带来了一些问题。 对于对等应用程序而言，尤为如此。  
  
 IPv6 协议将 IPv4 地址的长度定义为 128 位。 因此，IPv6 支持具有 3.2 x 10^38 个唯一地址（2^128）的超大 IP 地址空间。 凭借这一大小的地址空间，便能为每台连接到 Internet 的设备提供一个唯一的地址。 但是还存在一些问题。 世界上许多地方仍然只使用 IPv4。 特别是一些小型公司、组织和家庭使用的现有路由器和无线接入点，它们不支持 IPv6。 此外，一些为客户提供服务的 Internet 服务提供商也不支持 IPv6 或尚未配置对 IPv6 的支持。  
  
 已开发多个 IPv6 转换技术，用于在 IPv4 数据包中挖掘 IPv6 隧道地址。 这些技术包括 6to4、ISATAP 和 Teredo 隧道，在 IPv6 主机必须遍历 IP4 网络来连接其他 IPv6 网络时，为单播 IPv6 流量提供地址分配和主机到主机的自动隧道。 IPv6 数据包将隧道作为 IPv4 数据包发送。 正在使用多个隧道技术通过 NAT 设备实现 IPv6 地址的 NAT 遍历。  
  
 Teredo 是 IPv6 转换技术之一，使 IPv6 连接到 IPv4 网络。 Internet 工程任务组发布的 RFC 4380 中记录了这一技术。 Windows XP SP2 及更高版本为能够在 2001:0::/32 范围内提供公共 IPv6 地址的虚拟 Teredo 适配器提供支持。 此 IPv6 地址可用于侦听从 Internet 传入的连接，并且可以提供给支持 IPv6、需要连接到侦听服务的客户端。 应用程序可以只使用计算机的 IPv6 Teredo 地址连接计算机，因此，无需再担心应用程序如何为 NAT 设备下的计算机寻址。  
  
## <a name="enhancements-to-support-nat-traversal-and-teredo"></a>支持 NAT 遍历和 Teredo 的增强功能  

 向 <xref:System.Net>、<xref:System.Net.NetworkInformation>和 <xref:System.Net.Sockets> 命名空间添加了增强功能，这些增强功能支持使用 IPv6 和 Teredo 进行 NAT 遍历。  
  
 向 <xref:System.Net.NetworkInformation.IPGlobalProperties?displayProperty=nameWithType> 类添加了几种方法，用于获取主机上的单播 IP 地址列表。 <xref:System.Net.NetworkInformation.IPGlobalProperties.BeginGetUnicastAddresses%2A> 方法发起一个异步请求，该请求用于检索本地计算机上稳定的单播 IP 地址表。 <xref:System.Net.NetworkInformation.IPGlobalProperties.EndGetUnicastAddresses%2A> 方法结束挂起的异步请求，该请求用于检索本地计算机上稳定的单播 IP 地址表。 <xref:System.Net.NetworkInformation.IPGlobalProperties.GetUnicastAddresses%2A> 方法是一来检索本地计算机上稳定的单播 IP 地址的异步请求，它根据需要等待，直到地址表稳定。  
  
 <xref:System.Net.IPAddress.IsIPv6Teredo%2A?displayProperty=nameWithType> 属性可以用于确定 <xref:System.Net.IPAddress> 是否为 IPv6 Teredo 地址。  
  
 联合使用这些新 <xref:System.Net.NetworkInformation.IPGlobalProperties> 类方法和 <xref:System.Net.IPAddress.IsIPv6Teredo%2A> 属性可让应用程序轻松查找 Teredo 地址。 如果应用程序将本地 Teredo 地址传达给远程应用程序，则它只需知道该信息即可。 例如，一个对等应用程序可能会将其所有 IPv6 地址发送给匹配服务器，该服务器再将这些地址转发给其他对等机，从而实现直接通信。  
  
 应用程序通常应将其侦听服务设置为在 <xref:System.Net.IPAddress.IPv6Any?displayProperty=nameWithType> 上侦听，而不是在本地 Teredo 地址上侦听。 因此，如果远程客户端或对等机具有侦听服务主机的直接 IPv6 路由，该客户端或对等机可以使用 IPv6（而无需使用 Teredo）直接连接到隧道数据包。  
  
 对于 TCP 应用程序，<xref:System.Net.Sockets.TcpListener?displayProperty=nameWithType> 类具有 <xref:System.Net.Sockets.TcpListener.AllowNatTraversal%2A> 方法，该方法可启用 NAT 遍历。 对于 UDP 应用程序，<xref:System.Net.Sockets.UdpClient?displayProperty=nameWithType> 类具有 <xref:System.Net.Sockets.UdpClient.AllowNatTraversal%2A> 方法，该方法可启用 NAT 遍历。  
  
 对于使用 <xref:System.Net.Sockets.Socket?displayProperty=nameWithType> 和相关类的应用程序，可将 <xref:System.Net.Sockets.Socket.GetSocketOption%2A> 和 <xref:System.Net.Sockets.Socket.SetSocketOption%2A> 方法与 <xref:System.Net.Sockets.SocketOptionName.IPProtectionLevel?displayProperty=nameWithType> 套接字选项一起使用，用于查询、启用或禁用 NAT 遍历。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Net.IPAddress.IsIPv6Teredo%2A?displayProperty=nameWithType>
- <xref:System.Net.NetworkInformation.IPGlobalProperties.BeginGetUnicastAddresses%2A?displayProperty=nameWithType>
- <xref:System.Net.NetworkInformation.IPGlobalProperties.EndGetUnicastAddresses%2A?displayProperty=nameWithType>
- <xref:System.Net.NetworkInformation.IPGlobalProperties.GetUnicastAddresses%2A?displayProperty=nameWithType>
- <xref:System.Net.Sockets.IPProtectionLevel?displayProperty=nameWithType>
- <xref:System.Net.Sockets.Socket.SetIPProtectionLevel%2A?displayProperty=nameWithType>
- <xref:System.Net.Sockets.TcpListener.AllowNatTraversal%2A?displayProperty=nameWithType>
- <xref:System.Net.Sockets.UdpClient.AllowNatTraversal%2A?displayProperty=nameWithType>
