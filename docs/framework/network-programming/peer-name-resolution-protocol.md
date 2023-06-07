---
title: 对等名称解析协议
description: 了解对等名称解析协议 (PNRP)，一种安全、可缩放的动态名称注册和名称解析协议。
ms.date: 03/30/2017
ms.assetid: 11940511-c124-4d91-ae31-d4ed6e81ee58
ms.openlocfilehash: d50514569d066d04391ce65522df789ed421dbed
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96239388"
---
# <a name="peer-name-resolution-protocol"></a>对等名称解析协议

在对等环境中，对等机使用特定的名称解析系统从名称或其他类型的标识符解析彼此的网络位置（地址、协议和端口）。 过去，由于本质上的短暂性连接以及域名系统 (DNS) 内的其他缺陷，造成对等名称解析十分复杂。  
  
 Microsoft® Windows® 对等网络平台使用对等名称解析协议 (PNRP) 解决了这个问题，该协议是一个安全、可缩放的动态名称注册和名称解析协议，最早开发用于 Windows XP，然后在 Windows Vista™ 中得到升级。 PNRP 与传统的名称解析系统相比有很大不同，为应用程序开发者带来了振奋人心的全新可能。  
  
 通过 PNRP，对等名称可以应用于计算机或者计算机上单独的应用程序和服务。 对等名称解析包括地址、端口，以及可能的扩展有效负载。 此系统的优势在于容错度、无瓶颈以及绝不返回过时地址的名称解析；这使得该协议成为定位移动用户的卓越解决方案。  
  
 在安全性方面，对等名称可以发布为安全的（受保护的）或不安全的（不受保护的）。 PNRP 使用公钥加密保护安全对等名称免遭欺骗；计算机和服务都可使用 PNRP 命名。  
  
对等名称解析协议具备以下属性：  
  
- 分布式的，且几乎完全无服务器的。 只有在启动进程时才需要服务器。  
  
- 安全的名称发布，无需第三方介入。 和 DNS 名称发布不同，PNRP 名称发布是即时的，且没有财务成本。  
  
- PNRP 实时更新，以便防止解析过时地址。  
  
- 通过 PNRP 解析名称远不止适用于计算机，它还允许对服务进行名称解析。  
  
## <a name="the-systemnetpeertopeer-namespace"></a>System.Net.PeerToPeer 命名空间  
  
- PNRP 功能由 .NET Framework 版本 3.5 内的 <xref:System.Net.PeerToPeer> 命名空间定义。 它提供的类型集可以用于注册可用的 PNRP 服务并解析其对等名称。  
  
- （可以使用 <xref:System.ServiceModel.PeerResolvers> 命名空间中提供的类型创建并实例化 PNRP 和自定义对等解析程序。）  
  
- 有如下基本类型可用于注册可用的 PNRP 服务并解析其名称：  
  
- <xref:System.Net.PeerToPeer.Cloud>：定义描述可用 PNRP 云的信息，包括其范围。  
  
- <xref:System.Net.PeerToPeer.PeerName>：定义可用于在云中注册并随后解析对等机的对等名称。  
  
- <xref:System.Net.PeerToPeer.PeerNameRecord>：定义 PNRP 云中包含对等机注册信息在内的记录，其中包括可联系到该对等机的网络终结点。  
  
- <xref:System.Net.PeerToPeer.PeerNameRegistration>：定义对等名称的注册进程，包括开始和结束对等名称解析的方法。  
  
- <xref:System.Net.PeerToPeer.PeerNameResolver>：定义将对等名称解析到其网络终结点的进程，包括解析的同步和异步方法。  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.PeerResolvers>
- <xref:System.Net.PeerToPeer>
- [网络编程示例](network-programming-samples.md)

<!-- to-do: review sample links
- [PeerToPeer Technology Sample](https://go.microsoft.com/fwlink/?LinkID=179571)
-->
