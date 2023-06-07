---
title: 对等名称发布和解析
ms.date: 03/30/2017
ms.assetid: f0370e08-9fa6-4ee5-ab78-9a58a20a7da2
ms.openlocfilehash: 4a0787972a61f5700d1e8728be96db8ef9ee749e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/15/2020
ms.locfileid: "64623203"
---
# <a name="peer-name-publication-and-resolution"></a>对等名称发布和解析

## <a name="publishing-a-peer-name"></a>发布对等名称  

 若要发布新 PNRP ID，对等机需执行以下操作：  
  
- 将 PNRP 发布消息发送到缓存邻居（在缓存的最低级别中注册了 PNRP ID 的对等机）来播种缓存。  
  
- 在云中选择不是其邻居的随机节点，并为其 P2P ID 发送 PNRP 名称解析请求。 生成的终结点确定进程使用发布对等机的 PNRP ID 对云中随机节点的缓存进行种子化。  
  
若 PNRP 版本 2 节点只解析其他 P2P ID，则不发布 PNRP ID。 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\PeerNet\PNRP\IPV6-Global\SearchOnly=1 注册表值 （REG_DWORD 类型）指定对等机仅使用 PNRP 进行名称解析，绝不进行名称发布。 也可通过组策略配置此注册表值。  
  
## <a name="resolving-a-peer-name"></a>解析对等名称

 在 PNRP 网络或云中查找其他对等是一个包含两个阶段的过程：  
  
1. 终结点确定  
  
2. PNRP ID 解析  
  
 在终结点确定阶段，尝试解析另一台计算机上的服务 PNRP ID 的对等机确定该远程对等机的 IPv6 地址。  远程对等机是发布计算机或服务的 PNRP ID 或与之相关联的对等机。  
  
 确认远程终结点已注册到 PNRP 云中后，PNRP ID 解析阶段中的请求对等机向该对等机终结点发送请求以获取所需服务的 PNRP ID 。 终结点发送确认该服务的 PNRP ID 回复、注释，以及最多 4 KB 的附加信息，请求对等机可将该信息用于将来的通信。 例如，如果所需终结点是游戏服务器，附加对等名称记录数据可能包含有关游戏、游戏级别和当前玩家数量的信息。  
  
 在终结点确定阶段中，PNRP 使用迭代过程来查找发布 PNRP ID 的节点，其中执行解析的节点负责联系更接近目标 PNRP ID 的节点。  
  
 若要在 PNRP 中执行名称解析，对等机需检查其缓存中的条目，查找与目标 PNRP ID 匹配的条目。 找到后，对等机向对等机发送 PNRP 请求消息，并等待响应。 如果未找到该 PNRP ID 的条目，对等机会向具有 PNRP ID（最接近目标 PNRP ID）的条目相对应的对等机发送 PNRP 请求消息。 接收 PNRP 请求消息的节点检查其缓存，并执行以下操作：  
  
- 找到该 PNRP ID 后，所请求的终结点对等机直接回复请求对等机。  
  
- 如果未找到该 PNRP ID 且缓存中的 PNRP ID 更接近目标 PNRP ID，则所请求的对等机向请求对等机发送响应，其中包含表示具有更接近目标 PNRP ID 的 PNRP ID 条目的对等机的 IPv6 地址。 在响应中使用 IP 地址，请求节点向 IPv6 地址发送另一个 PNRP 请求消息，以响应或检查其缓存。  
  
- 如果未找到该 PNRP ID 且缓存中没有更接近目标 PNRP ID 的 PNRP ID，则所请求的对等机向请求对等机发送表明此情况的响应。 然后请求对等机选择下一个最接近的 PNRP ID。  
  
请求对等机通过连续迭代继续此过程，最终查找已注册该 PNRP ID 的节点。  
  
 在 <xref:System.Net.PeerToPeer> 命名空间中，<xref:System.Net.PeerToPeer.PeerName> 记录之间存在多对多的关系，这些关系包含终结点和 PNRP 云或用于通信的对等网格。 有重复或过时条目，或具有相同对等名称的多个节点时，PNRP 节点可以使用 <xref:System.Net.PeerToPeer.PeerNameResolver> 类获取当前信息。 <xref:System.Net.PeerToPeer.PeerNameResolver> 方法使用单一对等名称，将透视简化为一对多对等名称记录和相同的一对多对等机和云的名称记录。 这类似于使用关系表联接执行的查询。 成功完成后，解析程序对象返回指定对等名称的 <xref:System.Net.PeerToPeer.PeerNameRecordCollection>。  例如，集合中所有对等名称记录中将会出现由云排序的对等名称。 这些是对等名称实例，基于 PNRP 的应用程序可能请求这些支持数据。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Net.PeerToPeer>
