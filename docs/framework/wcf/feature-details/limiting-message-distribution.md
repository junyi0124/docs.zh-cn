---
title: 限制消息分布
ms.date: 03/30/2017
ms.assetid: 8b5ec4b8-1ce9-45ef-bb90-2c840456bcc1
ms.openlocfilehash: e736aba60d7d2b39d1b8eb958a8c72e6e8d55e13
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90555011"
---
# <a name="limiting-message-distribution"></a>限制消息分布

对等通道设计为一个广播网格。 其基本洪泛模型实现以下操作：将任意网格成员发送的每条消息均分发给该网格的所有其他成员。 在某个成员生成的每条消息都与所有其他成员相关且对他们有用的情况下（例如聊天室），这是一个理想的模型。 但是，许多应用程序偶尔需要限制消息分发。 例如，如果一个新成员加入网格，希望检索通过网格发送的最后一条消息，则无需将此请求群发给该网格的每个成员。 该请求可能会被限制为附近邻居，或可以筛选出本地生成的消息。还可以将消息发送到网格中的单个节点。 本主题讨论如何使用跃点计数、消息传播筛选器、本地筛选器或直接连接来控制通过网格转发消息的方式，并提供有关方式选择的通用准则。

## <a name="hop-counts"></a>跃点计数

`PeerHopCount` 的概念与 IP 协议中使用的 TTL（生存时间）类似。 `PeerHopCount` 的值绑定到消息实例，该值指定在丢弃消息之前应转发多少次。 对等通道客户端每次收到消息时，都会检查该消息，确定是否指定有 `PeerHopCount`。 如果已指定，则客户端会将跃点计数值减去一，然后再将消息转发给相邻节点。 客户端在收到跃点计数值为零的消息时，将处理该消息，但不转发给邻居。

可以通过将 `PeerHopCount` 作为属性 (attribute) 添加到邮件类实现中适用的属性 (property) 或字段中，来将跃点计数添加到消息中。 在将消息发送到网格之前，可以将该属性 (Property) 或字段设置为特定值。 这样，您可以根据需要使用跃点计数来限制消息在整个网格内的分发，潜在避免不必要的消息复制。 当网格包含大量的冗余数据，或者将消息发送到近邻或相距几个跃点的邻居时，这是很有用的。

- 有关代码段和相关信息，请参阅对等通道博客上的 [PeerHopCount 属性：控制消息分发](/archive/blogs/peerchan/the-peerhopcount-attribute-controlling-message-distribution) 公告。

## <a name="message-propagation-filter"></a>消息传播筛选器

`MessagePropagationFilter` 可以用于消息洪泛的自定义控制，尤其是在消息内容或其他特定方案确定传播时。 筛选器为通过节点的每个消息做出传播决定。 对于您的节点收到的产生自网格中其他位置的消息，以及您的应用程序创建的消息，就是这样做出决定的。 筛选器可以访问消息及其来源，因此可以基于可用的完整信息做出有关转发或丢弃消息的决定。

<xref:System.ServiceModel.PeerMessagePropagationFilter> 是具有单个函数 <xref:System.ServiceModel.PeerMessagePropagationFilter.ShouldMessagePropagate%2A> 的抽象基类。 方法调用的第一个参数传入消息的完整副本。 对消息所做的任何更改都不会影响实际的消息。 方法调用的最后一个自变量标识消息的源（`PeerMessageOrigination.Local` 或 `PeerMessageOrigination.Remote`）。 此方法的具体实现必须从 <xref:System.ServiceModel.PeerMessagePropagation> 枚举返回一个常量，以表示消息是要转发给本地应用程序 (`Local`)、转发给远程客户端 (`Remote`)、同时转发给本地应用程序和远程客户端 (`LocalAndRemote`)，还是均不转发 (`None`)。 可以通过访问对应的 `PeerNode` 对象，并在 `PeerNode.MessagePropagationFilter` 属性中指定所派生传播筛选器类的实例，来应用此筛选器。 请确保在打开对等通道之前附加传播筛选器。

- 有关代码段和相关信息，请参阅对等通道博客上的 [对等通道和 MessagePropagationFilter](/archive/blogs/peerchan/peer-channel-and-messagepropagationfilter) 公告。

## <a name="contacting-an-individual-node-in-the-mesh"></a>与网格中的单个节点联系

通过设置本地筛选器或直接连接，可以与网格中的单个节点联系。

如果网格中的每个节点均具有自己的 ID，则可以在消息的实现中指定一个目标 ID。 可以通过在消息协定中编写一个这样的函数来设置本地筛选器：仅当消息的 ID 与指定的目标 ID 匹配时，才向当前节点显示该消息。 这样网格在传输消息时，无需产生设置新连接的开销。 但是，由于在整个网格中多次发送消息，因此存在效率损耗。 只要消息既不过大也不过于频繁，这很适合将消息发送给网格的各个成员。

对于持续时间很长的高带宽连接，直接连接更为可取。 您可以通过网格发送连接信息，然后在选择发送/接收消息时设置一个直接连接。

## <a name="choosing-an-approach-for-limiting-message-distribution"></a>选择用于限制消息分发的方法

发现需要限制消息分发的情形时，请考虑以下问题：

- **谁** 需要接收消息？ 仅仅是一个邻居节点？ 网格中其他位置的节点？ 还是半个网格？

- 发送此消息的**频率如何**？

- 此消息将使用哪种 **带宽** ？

对这些问题的回答将帮助您确定是使用跃点计数、消息传播筛选器、本地筛选器还是直接连接。 请考虑以下通用准则：

- **人士**

  - *单个节点*：本地筛选器或直接连接。

  - 某个*邻近范围内的邻居*： PeerHopCount。

  - *网格的复杂子集*： MessagePropagationFilter。

- **发送频率**

  - *非常频繁*：直接连接、PeerHopCount、MessagePropagationFilter。

  - *偶尔*：本地筛选器。

- **所用带宽**

  - *高*：直接连接，不建议使用 MessagePropagationFilter 或本地筛选器。

  - *低*：可能不需要直接连接。

## <a name="see-also"></a>请参阅

- [生成对等通道应用程序](building-a-peer-channel-application.md)
