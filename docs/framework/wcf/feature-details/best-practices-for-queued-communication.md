---
title: 排队通信的最佳做法
ms.date: 03/30/2017
helpviewer_keywords:
- queues [WCF], best practices
- best practices [WCF], queued communication
ms.assetid: 446a6383-cae3-4338-b193-a33c14a49948
ms.openlocfilehash: 412b30a497fcf4c341f80a64c76fcbbc425e70b2
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96247442"
---
# <a name="best-practices-for-queued-communication"></a>排队通信的最佳做法

本主题提供 Windows Communication Foundation (WCF) 中排队通信的建议做法。 以下各节从方案角度讨论建议的做法。  
  
## <a name="fast-best-effort-queued-messaging"></a>快速、高效的排队消息处理  

 如果方案需要排队消息处理所提供的分离，还需要具有高效保证的快速、高性能消息处理，请使用非事务性队列并将 <xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A> 属性设置为 `false`。  
  
 此外，将 <xref:System.ServiceModel.MsmqBindingBase.Durable%2A> 属性设置为 `false`，还可以选择不产生磁盘写入开销。  
  
 安全性对性能有影响。 有关详细信息，请参阅 [性能注意事项](performance-considerations.md)。  
  
## <a name="reliable-end-to-end-queued-messaging"></a>可靠的端对端排队消息处理  

 以下各节介绍的建议做法适用于要求进行端对端可靠消息处理的方案。  
  
### <a name="basic-reliable-transfer"></a>基本可靠的传输  

 要实现端对端可靠性，请将 <xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A> 属性设置为 `true` 以确保传输安全。 <xref:System.ServiceModel.MsmqBindingBase.Durable%2A> 属性可以设置为 `true` 或 `false`，具体取决于您的需求（默认值为 `true`）。 作为端对端可靠性的一部分，<xref:System.ServiceModel.MsmqBindingBase.Durable%2A> 属性通常设置为 `true`。 代价是性能损失，但可使消息持久，这样，即使队列管理器崩溃，消息也不会丢失。  
  
### <a name="use-of-transactions"></a>事务的使用  

 必须使用事务来确保端对端可靠性。 `ExactlyOnce` 保证只能确保将消息传送到目标队列。 若要确保接收到消息，请使用事务。 如果不使用事务，在服务崩溃的情况下，将丢失虽然正在传递实际上已传送到应用程序的消息。  
  
### <a name="use-of-dead-letter-queues"></a>死信队列的使用  

 死信队列可确保您在消息未能传递到目标队列时收到通知。 您可以使用系统提供的死信队列，也可以使用自定义的死信队列。 通常，最好使用自定义的死信队列，因为这样可以将死信消息从一个应用程序发送到单个死信队列。 否则，系统上运行的所有应用程序产生的所有死信消息都会传递到单个队列。 然后，每个应用程序都必须在该死信队列中搜索，寻找与自己相关的死信消息。 有时，使用自定义死信队列并不可行，例如使用 MSMQ 3.0 时。  
  
 不建议为端对端可靠通信关闭死信队列。  
  
 有关详细信息，请参阅 [使用 Dead-Letter 队列处理消息传输失败](using-dead-letter-queues-to-handle-message-transfer-failures.md)。  
  
### <a name="use-of-poison-message-handling"></a>病毒消息处理的使用  

 通过病毒消息处理，可从消息处理失败的状态下恢复。  
  
 在使用病毒消息处理功能时，请确保将 <xref:System.ServiceModel.MsmqBindingBase.ReceiveErrorHandling%2A> 属性设置为适当的值。 将该属性设置为 <xref:System.ServiceModel.ReceiveErrorHandling.Drop> 表示数据丢失。 另一方面，如果该属性设置为 <xref:System.ServiceModel.ReceiveErrorHandling.Fault>，服务主机一旦检测到病毒消息，则被视为出现了错误。 使用 MSMQ 3.0 时，最好使用 <xref:System.ServiceModel.ReceiveErrorHandling.Fault> 来避免数据丢失并移出病毒消息。 使用 MSMQ 4.0 时，建议使用 <xref:System.ServiceModel.ReceiveErrorHandling.Move>。 <xref:System.ServiceModel.ReceiveErrorHandling.Move> 将病毒消息移出队列，以便服务可以继续处理新消息。 这样，病毒消息服务就可以单独处理病毒消息。  
  
 有关详细信息，请参阅 [病毒消息处理](poison-message-handling.md)。  
  
## <a name="achieving-high-throughput"></a>实现高吞吐量  

 若要在单个终结点上实现高吞吐量，请使用下面的方法：  
  
- 事务处理批处理。 事务处理批处理可确保在单个事务中能够读取多个消息。 这样可优化事务提交，从而提高整体性能。 批处理的代价在于，如果一个批次内某个消息出现错误，则整个批次都会回滚，并且这些消息必须逐个处理，直到可以再次安全地进行批处理为止。 大多数情况下，很少出现病毒消息，因此首选使用批处理来提高系统性能，尤其是具有参与事务的其他资源管理器时。 有关详细信息，请参阅 [在事务中批处理消息](batching-messages-in-a-transaction.md)。  
  
- 并发。 并发可增加吞吐量，但并发也会影响对共享资源的争用。 有关详细信息，请参阅 [并发](../samples/concurrency.md)。  
  
- 遏制。 要实现最佳性能，需要遏制调度程序管线中的消息数。 有关如何执行此操作的示例，请参阅 [限制](../samples/throttling.md)。  
  
 使用批处理时，应注意并发和遏制转换为并发批处理。  
  
 若要实现更高的吞吐量和可用性，请使用从队列中读取的 WCF 服务场。 这要求所有这些服务都在相同的终结点上公开相同的协定。 场方法最适用于具有高消息产生率的应用程序，因为它使大量服务都从同一队列中进行读取操作。  
  
 使用场时，应注意 MSMQ 3.0 不支持远程事务处理读取。 MSMQ 4.0 支持远程事务处理读取。  
  
 有关详细信息，请参阅 [在事务中批处理消息](batching-messages-in-a-transaction.md) 和 [Windows Vista、windows Server 2003 和 windows XP 中的队列功能](diff-in-queue-in-vista-server-2003-windows-xp.md)之间的差异。  
  
## <a name="queuing-with-unit-of-work-semantics"></a>以工作语义为单元排队  

 某些情况下，队列中的一组消息可能具有相关性，因此这些消息的顺序很重要。 在这些情况下，将一组相关消息作为单个单元进行处理：要么成功处理所有消息，要么所有消息的处理都不成功。 若要实现这样的行为，请将会话用于队列。  
  
 有关详细信息，请参阅 [将排队消息分组到会话中](grouping-queued-messages-in-a-session.md)。  
  
## <a name="correlating-request-reply-messages"></a>关联请求-回复消息  

 虽然队列一般是单向的，但在某些情况下，可能希望将接收到的回复关联到先前发送的请求。 如果需要此类关联，建议应用自己的 SOAP 消息头，它包含消息的关联信息。 通常，发送方将此标头附加到消息，接收方处理消息并在回复队列中用新消息回复时，会附加发送方的消息头，消息头包含关联信息，这样发送方就能通过请求消息识别出回复消息。  
  
## <a name="integrating-with-non-wcf-applications"></a>集成非 WCF 应用程序  

 `MsmqIntegrationBinding`在将 wcf 服务或客户端与非 WCF 服务或客户端集成时使用。 非 WCF 应用程序可以是使用 System.web、COM +、Visual Basic 或 c + + 编写的 MSMQ 应用程序。  
  
 使用 `MsmqIntegrationBinding` 时，应注意以下几点：  
  
- WCF 消息正文与 MSMQ 消息正文不同。 当使用排队绑定发送 WCF 消息时，将 WCF 消息正文放置在 MSMQ 消息的内部。 MSMQ 基础结构并不在意这一额外信息，它只注意 MSMQ 消息。  
  
- `MsmqIntegrationBinding` 支持常见的序列化类型。 根据序列化类型和一般消息的正文类型，<xref:System.ServiceModel.MsmqIntegration.MsmqMessage%601> 采用不同的类型参数。 例如，<xref:System.ServiceModel.MsmqIntegration.MsmqMessageSerializationFormat.ByteArray> 需要 `MsmqMessage\<byte[]>` 而 <xref:System.ServiceModel.MsmqIntegration.MsmqMessageSerializationFormat.Stream> 需要 `MsmqMessage<Stream>`。  
  
- 使用 XML 序列化时，可以使用元素上的属性指定已知类型， `KnownTypes` 然后使用该属性来 [\<behavior>](../../configure-apps/file-schema/wcf/behavior-of-servicebehaviors.md) 确定如何对 XML 消息进行反序列化。  
  
## <a name="see-also"></a>另请参阅

- [在 WCF 中排队](queuing-in-wcf.md)
- [如何：使用 WCF 终结点交换排队消息](how-to-exchange-queued-messages-with-wcf-endpoints.md)
- [如何：与 WCF 终结点和消息队列应用程序交换消息](how-to-exchange-messages-with-wcf-endpoints-and-message-queuing-applications.md)
- [在会话中对排队消息进行分组](grouping-queued-messages-in-a-session.md)
- [在事务中对消息进行批处理](batching-messages-in-a-transaction.md)
- [使用死信队列处理消息传输故障](using-dead-letter-queues-to-handle-message-transfer-failures.md)
- [病毒消息处理](poison-message-handling.md)
- [Windows Vista、Windows Server 2003 和 Windows XP 在排队功能方面的差异](diff-in-queue-in-vista-server-2003-windows-xp.md)
- [使用传输安全保护消息](securing-messages-using-transport-security.md)
- [使用消息安全保护消息](securing-messages-using-message-security.md)
- [排队消息处理疑难解答](troubleshooting-queued-messaging.md)
