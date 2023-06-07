---
title: IPv6 路由
ms.date: 03/30/2017
ms.assetid: c98731b4-b542-46a2-9947-1cea63c186b2
ms.openlocfilehash: 646eef4ec178472a99f60de4785fd53381296c3a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96258330"
---
# <a name="ipv6-routing"></a>IPv6 路由

灵活的路由机制是 IPv6 的优势之一。 由于 IPv4 网络 ID 以前和现在的分配方式，需要由 Internet 主干网上的路由器来维护大型路由表。 这些路由器必须了解所有路由，才能转发可能定向到 Internet 任何节点的数据包。 借助聚合地址的功能，IPv6 可以灵活寻址，并且大大减小路由表的大小。 在这种新型寻址体系结构中，中间路由器必须仅跟踪网络中的本地部分，才能恰当地转发消息。  
  
## <a name="neighbor-discovery"></a>邻居发现  

 邻居发现提供的一些功能是：  
  
- 路由器发现。 这允许主机识别本地路由器。  
  
- 地址解析。 这允许节点解析相应的下一跃点地址的链接层地址（地址解析协议 [ARP] 的替换）。  
  
- 地址自动配置。 这允许主机自动配置站点本地和全局地址。  
  
 邻居发现使用 IPv6 的 Internet 控制消息协议 (ICMPv6) 消息，包括：  
  
- 路由器播发。 由路由器在伪周期基础上发送或作为路由器招标的响应发送。 IPv6 路由器使用路由器播发来播发其可用性、地址前缀和其他参数。  
  
- 路由器招标。 由主机发送以请求链接上的路由器立即发送路由器播发。  
  
- 邻居招标。 由节点发送，以进行地址解析、重复地址检测或验证邻居是否仍可以访问。  
  
- 邻居播发。 由节点发送，以响应邻居招标或通知邻居链接层地址的更改。  
  
- 重定向。 由路由器发送，以指示特定目标的下一个更好的跃点地址，用于发送节点。  
  
## <a name="see-also"></a>另请参阅

- [Internet 协议版本 6](internet-protocol-version-6.md)
- [套接字](sockets.md)
