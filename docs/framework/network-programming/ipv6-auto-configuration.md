---
title: IPv6 自动配置
description: 了解 IPv6 如何支持节点即插即用，其中节点加入 IPv6 网络并在无人工干预的情况下进行配置。
ms.date: 03/30/2017
ms.assetid: 581c1d21-1013-43a3-bf3e-2d9ead62b79c
ms.openlocfilehash: db6156433e21411ff1e88634a359bbde7528fd91
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96258369"
---
# <a name="ipv6-auto-configuration"></a>IPv6 自动配置

IPv6 的一个重要目标是支持节点即插即用。 也就是说，可以将节点插入 IPv6 网络，让其自动进行配置，无需任何人为干预。  
  
## <a name="type-of-auto-configuration"></a>自动配置类型  

 IPv6 支持以下自动配置类型：  
  
- 有状态自动配置。 这种类型的配置需要一定程度的人为干预，因其需要 IPv6 动态主机配置协议 (DHCPv6) 服务器来安装和管理节点。 DHCPv6 服务器保存向其提供配置信息的节点列表。 它还维护状态信息，这样服务器知道每个地址的使用时间，以及何时可用于重新分配。  
  
- 无状态自动配置。 这种配置类型适用于小型组织和个人。 这种情况下，每个主机通过接收的路由器播发的内容确定其地址。 使用 IEEE EUI-64 标准来定义地址的网络 ID 部分，可以合理地假定链接上主机地址的唯一性。  
  
 无论如何确定地址，节点必须验证可能的地址对于本地链接是否唯一。 通过向可能的地址发送邻居请求消息可完成此操作。 如果节点收到任何响应，就知道该地址已在使用，并且必须确定其他地址。  
  
## <a name="ipv6-mobility"></a>IPv6 移动性  

 移动设备的广泛应用催生了新的要求：设备必须能够随意改变 IPv6 Internet 上的位置，但是仍然保持现有连接。 为实现此功能，向移动节点分配一个始终可以到达的主地址。 当移动节点在其中时，它连接到主链接并使用其主地址。 当移动节点离开时，主代理（通常是路由器）在移动节点和其正与之通信的节点之间中继消息。  
  
## <a name="see-also"></a>请参阅

- [Internet 协议版本 6](internet-protocol-version-6.md)
- [套接字](sockets.md)
