---
title: 对等网格
ms.date: 03/30/2017
ms.assetid: d93e312e-ac04-40f8-baea-5da1cacb546e
ms.openlocfilehash: 62feb237dd4a8a471175e32363887376f7d86212
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96272093"
---
# <a name="peer-meshes"></a>对等网格

*网格* 是一种名为的集合， (对等节点之间的互连图形) ，这些节点可以彼此通信，并且由唯一的网格 ID 标识。 每个节点都与其他多个节点相连接。 在连接良好的网格中，任何两个节点之间都存在一个路径，网格最远边缘的节点之间的跃点相对较少，并且即使某些节点或连接已断开，网格也将保持连接状态。网格中的活动节点使用相应的网格 ID 发布其终结点信息，以便其他对等节点可以找到它们。  
  
## <a name="characteristics-of-a-mesh-created-using-peer-channel"></a>使用对等通道创建的网格的特征  
  
#### <a name="uniquely-identified"></a>标识唯一性  
  
- 每个网格由唯一的 ID 标识。 网格名称（或网格 ID）与域名系统 (DNS) 主机名的格式相同。 因此，在所使用的解析程序的作用范围内，该网格 ID 对应用程序的目标客户端而言必须是唯一的。 像“MyFamilysPeers”或“KevinsPokerTable”这样的常见名称很容易与其他用户名发生冲突，因此可能返回意外的对等终结点信息，从而导致发生隐私问题或增加连接延迟。 避免这些问题的一种方法是给网格昵称添加一个唯一的 ID 作为后缀（例如“KevinsPokerTable90210”）。  
  
#### <a name="message-flooding"></a>消息洪泛  
  
- 网格允许消息从一个或多个发送方传播到同一网格中的其他所有对等节点。 由对等节点发送的消息洪流使用 `http://schemas.microsoft.com/net/2006/05/peer` 上的命名空间中指定的标头。  
  
#### <a name="optimized-connections"></a>优化的连接  
  
- 当节点加入和离开网格时，对等通道网格会自动进行调整，以确保所有节点都具有良好的连接性，并尽可能降低产生分区（相互隔离的节点组）的可能性。 网格中的连接还根据当前流量模式进行动态优化，以便尽量减小从发送方到接收方的消息延迟。  
  
#### <a name="popular-network-features-that-peer-channel-does-not-provide"></a>对等通道不提供的常用网络功能  

 了解对等通道没有提供的一些常用网络功能非常重要。 这些功能可能全部构建在对等通道之上，其中包括：  
  
- **消息顺序：** 源自单个源的消息可能不会以相同顺序或按发送源的顺序到达所有其他方。 要求按一定顺序传递消息的应用程序必须将此功能内置到应用程序中（例如，通过在所有消息中包含一个单调递增的 ID）。  
  
- **可靠消息传送：** 对等通道不包括确保所有对等方接收消息的机制。 若要保证消息得到传递，必须在对等通道之上编写一个可靠层。
