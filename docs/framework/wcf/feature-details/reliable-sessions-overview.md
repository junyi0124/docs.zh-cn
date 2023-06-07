---
title: 可靠会话概述
ms.date: 03/30/2017
ms.assetid: a7fc4146-ee2c-444c-82d4-ef6faffccc2d
ms.openlocfilehash: a85a34c5e2ec7928c01586e4b01cdf5e90e896a7
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601083"
---
# <a name="reliable-sessions-overview"></a>可靠会话概述

Windows Communication Foundation （WCF） SOAP 可靠消息传送提供 SOAP 终结点之间的端到端消息传输可靠性。 此消息传递通过克服传输失败和 SOAP 消息级别失败，可在不可靠的网络上实现传输可靠性。 具体而言，它为跨 SOAP 或传输中介发送的消息提供了一种基于会话的、单一的和（可选）有序的传送。 基于会话的传递可用于将会话中的消息分组，并对消息进行可选排序。

本主题介绍可靠会话、使用方法和时间，以及如何对其进行保护。

## <a name="wcf-reliable-sessions"></a>WCF 可靠会话

WCF 可靠会话是由 WS-RELIABLEMESSAGING 协议定义的 SOAP 可靠消息的实现。

WCF SOAP 可靠消息传送提供两个终结点之间的端到端可靠会话，而不考虑分隔消息传递终结点的中介的数量或类型。 这包括任何不使用 SOAP 的传输中介（例如 HTTP 代理）或使用 SOAP （例如，基于 SOAP 的路由器或桥接）的中介，消息在终结点之间流动。 可靠会话通道支持*交互式*通信，因此，由此类通道连接的服务可同时运行，并在低延迟的条件下（即，在相对较短的时间间隔内）交换和处理消息。 这种耦合意味着这些组件将进度一起或故障转移，因此它们之间不会提供隔离。

可靠会话可以屏蔽两种失败情况：

- SOAP 消息级失败，其中包括消息丢失或重复的情况以及消息到达的顺序与消息发送的顺序不同的情况。

- 传输失败。

可靠会话实现 WS-ReliableMessaging 协议和内存中传输窗口以屏蔽 SOAP 消息级失败，并在传输失败的情况下重新建立连接。

可靠会话为 SOAP 消息提供 TCP 为 IP 数据包提供的内容。 TCP 套接字连接在节点之间提供单个的 IP 数据包有序传输。 可靠通道提供相同类型的可靠传输，但在以下方面与 TCP 套接字可靠性不同：

- 可靠性位于 SOAP 消息级别，而不是针对一个任意大小的字节数据包。

- 可靠性是不特定于传输的，而不仅仅是通过 TCP 传输。

- 可靠性与特定传输会话（例如，TCP 连接提供的会话）无关，可以在可靠会话的生存期内同时或按顺序使用多个传输会话。

- 可靠会话是发送方和接收方的 SOAP 终结点之间的会话，与二者之间的连接所需的传输连接的数目无关。 简而言之，TCP 可靠性结束后传输连接结束，而可靠会话提供端到端的可靠性。

## <a name="reliable-sessions-and-bindings"></a>可靠会话和绑定

如前文所述，可靠会话是无特定传输的。 此外，还可以通过多种消息交换模式（例如请求-答复或双工）建立可靠会话。 WCF 可靠会话公开为一组绑定的属性。

在使用以下的终结点上使用可靠会话：

- 基于 HTTP 的传输协议标准绑定：

  - `WsHttpBinding` 并公开请求-答复或单向协定。

  - 通过请求-答复或简单单向服务约定使用可靠会话时。

  - `WsDualHttpBinding` 并公开双工、请求-答复或单向协定。

  - `WsFederationHttpBinding` 并公开请求-答复或单向协定。

- 基于 TCP 的传输协议标准绑定：

  - `NetTcpBinding` 并公开双工、请求答复或单向协定。

通过创建自定义绑定（如 HTTPS）（有关问题的详细信息，请参阅<a href="#reliable-sessions-and-security">可靠会话和安全性</a>）或命名管道绑定，在任何其他绑定上使用可靠会话。

可以在不同的基础通道类型上堆叠可靠会话，并且生成的可靠会话通道形状会变化。 在客户端和服务器上，受支持的可靠会话通道的类型取决于所使用的基础通道的类型。 下表列出了在客户端上作为基础通道的一项功能支持的会话通道的类型。

| 支持的可靠会话通道类型&#8224; | `IRequestChannel` | `IRequestSessionChannel` | `IDuplexChannel` | `IDuplexSessionChannel` |
| ----------------------------------------------- | :---------------: | :----------------------: | :--------------: | :---------------------: |
| `IOutputSessionChannel`                         | 是               | 是                      | 是              | 是                     |
| `IRequestSessionChannel`                        | 是               | 是                      | No               | 否                      |
| `IDuplexSessionChannel`                         | 否                | 否                       | 是              | 是                     |

&#8224;支持的通道类型是 `TChannel` 传递给方法的泛型参数值可用的值 <xref:System.ServiceModel.Channels.ReliableSessionBindingElement.BuildChannelFactory%60%601%28System.ServiceModel.Channels.BindingContext%29> 。

下表列出了在服务器上作为基础通道的一项功能支持的会话通道的类型。

| 支持的可靠会话通道类型&#8225; | `IReplyChannel` | `IReplySessionChannel` | `IDuplexChannel` | `IDuplexSessionChannel` |
| ----------------------------------------------- | :-------------: | :--------------------: | :--------------: | :---------------------: |
| `IInputSessionChannel`                          | 是             | 是                    | 是              | 是                     |
| `IReplySessionChannel`                          | 是             | 是                    | No               | 否                      |
| `IDuplexSessionChannel`                         | 否              | 否                     | 是              | 是                     |

&#8225;支持的通道类型是 `TChannel` 传递给方法的泛型参数值可用的值 <xref:System.ServiceModel.Channels.ReliableSessionBindingElement.BuildChannelListener%60%601%28System.ServiceModel.Channels.BindingContext%29> 。

## <a name="reliable-sessions-and-security"></a>可靠会话和安全

保护可靠会话对于确保通信方（服务和客户端）进行身份验证，并且会话中交换的消息不会被篡改，这一点非常重要。 而且，确保每个可靠会话的完整性是很重要的。 通过将可靠会话绑定到由安全会话通道表示和管理的安全上下文，可对其进行保护。 安全通道提供安全会话。 然后，使用在会话建立的过程中交换的安全令牌保护可靠会话中的消息。

当可靠会话通过 TCP 时，TCP 会话与可靠会话相关联。 因此，传输安全机制可确保安全会话与可靠会话相关联。 在此情况下，将关闭重新建立连接的操作。

唯一的例外是在使用 HTTPS 时。 安全套接字层（SSL）会话未绑定到可靠会话。 这会导致威胁，因为共享安全上下文（SSL 会话）的会话不会相互保护;这可能会也可能不是真正的威胁，具体取决于应用程序。

## <a name="using-reliable-sessions"></a>使用可靠会话

若要使用 WCF 可靠会话，请使用支持可靠会话的绑定创建一个终结点。 使用启用了可靠会话的 WCF 提供的系统提供的绑定之一，或者创建自己的自定义绑定来执行此项。

默认情况下支持并启用可靠会话的系统定义的绑定包括：

- <xref:System.ServiceModel.WSDualHttpBinding>

默认情况下，系统提供的绑定支持可靠会话，但默认情况下不启用一个的绑定包括：

- <xref:System.ServiceModel.WSHttpBinding>

- <xref:System.ServiceModel.WSFederationHttpBinding>

- <xref:System.ServiceModel.NetTcpBinding>

有关如何创建自定义绑定的示例，请参阅[如何：使用 HTTPS 创建自定义可靠会话绑定](how-to-create-a-custom-reliable-session-binding-with-https.md)。

有关支持可靠会话的 WCF 绑定的讨论，请参阅[系统提供的绑定](../system-provided-bindings.md)。

## <a name="when-to-use-reliable-sessions"></a>何时使用可靠会话

了解何时在应用程序中使用可靠会话是很重要的。 WCF 支持同时处于活动状态和活动状态的终结点之间的可靠会话。 如果你的应用程序要求其中一个终结点不可用，则使用队列来实现可靠性。

如果方案要求通过 TCP 连接两个终结点，则 TCP 可能足以提供可靠的消息交换。 尽管不需要使用可靠会话，但由于 TCP 可以确保数据包按顺序到达，而且只会出现一次。

如果你的方案具有以下任何特征，则必须认真考虑使用可靠会话。

- SOAP 中介，如 SOAP 路由器

- 代理中介或传输桥

- 间歇性连接

- HTTP 会话

## <a name="see-also"></a>另请参阅

- [使用绑定配置服务和客户端](../using-bindings-to-configure-services-and-clients.md)
- [WS 可靠会话](../samples/ws-reliable-session.md)
