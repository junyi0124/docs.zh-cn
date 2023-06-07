---
title: 可靠会话的最佳做法
ms.date: 03/30/2017
ms.assetid: b94f6e01-8070-40b6-aac7-a2cb7b4cb4f2
ms.openlocfilehash: 1d9671e7e3124d535b66de8cd8468f76dcb32b10
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "61857982"
---
# <a name="best-practices-for-reliable-sessions"></a>可靠会话的最佳做法

本主题讨论可靠会话的最佳实践。

## <a name="setting-maxtransferwindowsize"></a>设置 MaxTransferWindowSize

Windows Communication Foundation (WCF) 中的可靠会话使用传输窗口保存客户端和服务上的消息。 可配置属性 <xref:System.ServiceModel.Channels.ReliableSessionBindingElement.MaxTransferWindowSize%2A> 指示传输窗口可以保存多少条消息。

在发件人，这指示等待确认; 时传输窗口可以保存多少条消息在接收方，它指示要为服务缓冲的消息数。

选择合适的大小会影响网络的效率和服务的最佳容量。 以下各节将详细介绍选择此属性和值的影响的值时要考虑。

默认传输窗口大小为 8 条消息。

### <a name="efficient-use-of-the-network"></a>有效地利用网络

在此上下文中，术语*网络*对应于所有内容用作客户端 （发送方） 和服务 （接收方） 之间的通信的基础。 这包括传输连接和任何中间站点或在之间架起桥梁包括 SOAP 路由器或 HTTP 代理/防火墙。

有效使用网络可确保充分利用网络容量。 可以每秒通过网络传输的数据量 (*数据速率*) 并将数据从发送方到接收方传输所花费的时间 (*延迟*) 如何有效地影响网络则可利用。

在发送方，属性 <xref:System.ServiceModel.Channels.ReliableSessionBindingElement.MaxTransferWindowSize%2A> 指示在等待确认消息时，其传输窗口可以保存多少条消息。 如果网络延迟较高，并且为了确保及时响应发送者和网络有效利用，应增加传输窗口大小。

例如即使发件人不断变化的数据速率，延迟时间会很高如果发送者和接收者之间存在多个中介或者数据都必须通过率有损中介或网络。 因此，发件人必须接受新消息在网络上发送之前，等待其传输窗口中消息的确认信息。 具有高延迟、 较少的有效的小缓冲区网络利用率。 另一方面，过高的传输窗口大小可能会影响服务，因为该服务可能需要几乎与客户端发送的数据的速率很高。

### <a name="running-the-service-to-capacity"></a>与容量运行服务

尽可能有效地使用网络，理想情况下还想要在最佳容量运行的服务。 接收方上的传输窗口大小属性指示接收方可以缓冲多少条消息。 此消息缓冲不仅帮助网络流控制，而且还可让服务满负荷运行。 例如，如果缓冲区是一条消息，消息到达的速度超过了该服务可以处理它们，然后在网络可能会丢弃一些消息，并可能浪费或闲置容量。

使用缓冲区可提高服务的可用性，因为它同时接收并处理以前接收的消息时缓冲消息。

我们建议你使用相同`MaxTransferWindowSize`发送方和接收方。

### <a name="enabling-flow-control"></a>启用流控制

*流控制*是一种机制，可确保发送方和接收方跟上节奏彼此，消息是使用和处理，即快速它们正在生成。 客户端和服务上的传输窗口大小可确保发送方和接收方在一个合理的同步窗口中。

我们强烈建议将属性设置<xref:System.ServiceModel.Channels.ReliableSessionBindingElement.FlowControlEnabled%2A>到`true`当使用 WCF 客户端和 WCF 服务之间可靠会话。

## <a name="setting-maxpendingchannels"></a>设置 MaxPendingChannels

在编写一个服务，使来自不同客户端的可靠会话通信，就可以有许多客户端创建在同一时间对服务的可靠会话。 在这些情况下，服务的响应取决于 `MaxPendingChannels` 属性。

当发送方创建到接收方的可靠会话通道时，发送方和接收方之间的握手将建立可靠会话。 建立可靠会话之后，该通道会放入到挂起的通道队列中以供服务接受。 此 `MaxPendingChannels` 属性指示有多少个通道可以处于此状态。

很可能要在其中它不能接受更多通道状态的服务。 如果队列已满时，在尝试建立可靠会话被拒绝，并在客户端必须重试。

还有可能更长的时间在队列中挂起的通道保留在队列中。 在此期间，可能出现可靠会话的非活动超时，从而导致通道转换到出错状态。

在编写同时服务多个客户端的服务时，应设置一个值，适用于你的需求。 设置过高的值`MaxPendingChannels`属性会影响您的工作集。

默认值为<xref:System.ServiceModel.Channels.ReliableSessionBindingElement.MaxPendingChannels%2A>是四个通道。

## <a name="reliable-sessions-and-hosting"></a>可靠会话和宿主

Web 承载服务使用可靠会话时，您应记住以下重要注意事项：

- 可靠会话是有状态的，而状态在 AppDomain 中进行维护。 这意味着，属于可靠会话的一部分的所有消息必须在同一个 AppDomain 中进行处理。 场或园的大小大于一个节点的 web 场和网络园无法保证此约束。

- 可靠会话使用双工 HTTP 通道 (例如，使用`WsDualHttpBinding`) 会要求多于默认值为两个 HTTP 连接每个客户端。 这意味着，双工可靠会话可能需要最多两个连接每一种方式，因为并发应用程序和协议消息可能会传输每一种方式在任意给定时间。 某些情况下根据该服务的消息交换模式，这意味着就可能发生死锁 web 承载的服务使用双工 HTTP 和可靠会话。 若要增加每个客户端允许的 HTTP 连接数，将以下代码添加到相关的配置文件 (例如， *web.config*相关服务造成的):

  ```xml
  <configuration>
    <system.net>
      <connectionManagement>
        <add name="*" maxconnection="4" />
      </connectionManagement>
    </system.net>
  </configuration>
  ```

  值`maxconnection`属性是所需的连接数。 在这种情况下，最小值应为四个连接。
