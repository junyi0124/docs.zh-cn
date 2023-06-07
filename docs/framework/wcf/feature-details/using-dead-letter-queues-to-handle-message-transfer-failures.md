---
title: 使用死信队列处理消息传输故障
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 9e891c6a-d960-45ea-904f-1a00e202d61a
ms.openlocfilehash: d3087021c717d6ee055a4a1f5332d9d259f06381
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96289589"
---
# <a name="using-dead-letter-queues-to-handle-message-transfer-failures"></a>使用死信队列处理消息传输故障

排队消息可能传送失败。 这些失败的消息将记录在死信队列中。 传送失败可能是由于网络故障、队列已删除、队列已满、身份验证失败或未能准时传送等原因而引起的。  
  
 如果接收应用程序未从队列中及时读取排队消息，则这些消息可能会在队列中长时间保留。 这种行为对于时效性较强的消息可能不合适。 时效较强的消息在排队绑定中设置了一个生存时间 (TTL) 属性，该属性指示在这些消息必须过期之前可以在队列中保留多长时间。 过期消息将发送到一个称为死信队列的特殊队列中。 也可以出于其他原因将这些消息放入死信队列，如超出队列配额或由于身份验证失败。  
  
 通常，应用程序会编写补偿逻辑从死信队列中读取这些消息及失败的原因。 补偿逻辑取决于失败的原因。 例如，当身份验证失败时，您可以更正附有消息的证书，然后重新发送该消息。 如果传送失败的原因在于已达到目标队列的配额，则您可以在认为配额问题已得到解决时重新尝试发送。  
  
 大多数队列系统都有系统范围的死信队列，其中存储来自该系统的所有失败消息。 消息队列 (MSMQ) 提供两个系统范围的死信队列：事务性系统范围死信队列，用于存储未能传送到事务性队列的消息；非事务性系统范围死信队列，用于存储未能传送到非事务性队列的消息。 如果两个客户端将消息发送到两个不同的服务，因此，WCF 中的不同队列共享相同的 MSMQ 服务以发送，则可能会在系统死信队列中混合使用消息。 这并不总是最佳的。 在某些情况下（如安全），您可能不希望一个客户端从死信队列中读取另一个客户端的消息。 共享死信队列还要求客户端浏览该队列来查找这些客户端所发送的消息，但根据死信队列中的消息数量，这样做的开销可能会极其大。 因此，在 WCF 中 `NetMsmqBinding` ， `MsmqIntegrationBinding,` Windows Vista 上的 MSMQ 提供自定义死信队列， (有时称为应用程序特定的死信队列) 。  
  
 自定义死信队列在共享同一个 MSMQ 服务来发送消息的各客户端之间提供隔离。  
  
 在 Windows Server 2003 和 Windows XP 上，Windows Communication Foundation (WCF) 为所有排队的客户端应用程序提供系统级死信队列。 在 Windows Vista 上，WCF 提供每个排队客户端应用程序的死信队列。  
  
## <a name="specifying-use-of-the-dead-letter-queue"></a>指定死信队列的用途  

 死信队列位于发送应用程序的队列管理器中。 该队列中存储已过期或者传输/传送失败的消息。  
  
 该绑定具有下列死信队列属性：  
  
- <xref:System.ServiceModel.MsmqBindingBase.DeadLetterQueue%2A>  
  
- <xref:System.ServiceModel.MsmqBindingBase.CustomDeadLetterQueue%2A>  
  
## <a name="reading-messages-from-the-dead-letter-queue"></a>从死信队列中读取消息  

 从死信队列中读取消息的应用程序类似于从应用程序队列读取消息的 WCF 服务，但存在以下细微差异：  
  
- 若要从系统的事务性死信队列中读取消息，统一资源标识符 (URI) 必须为以下形式：net.msmq://localhost/system$;DeadXact。  
  
- 若要从系统的非事务性死信队列中读取消息，URI 必须为以下形式：net.msmq://localhost/system$;DeadLetter。  
  
- 若要从自定义死信队列中读取消息，URI 必须采用以下格式： net.tcp：//localhost/private/， \<*custom-dlq-name*> 其中 *dlq* 是自定义死信队列的名称。  
  
 有关如何对队列进行寻址的详细信息，请参阅 [服务终结点和队列寻址](service-endpoints-and-queue-addressing.md)。  
  
 接收方上的 WCF 堆栈会将服务正在侦听的地址与消息上的地址相匹配。 如果地址匹配，则调度该消息；否则，不调度该消息。 在从死信队列中进行读取时，这种操作可能会引发问题，原因是死信队列中的消息通常将寻址到该服务而非死信队列服务。 因此，从死信队列中进行读取的服务必须安装一个地址筛选器 `ServiceBehavior`，指示堆栈独立于被寻地址来与队列中的所有消息匹配。 具体而言，您必须将一个带有 `ServiceBehavior` 参数的 <xref:System.ServiceModel.AddressFilterMode.Any> 添加到从死信队列中读取消息的服务中。  
  
## <a name="poison-message-handling-from-the-dead-letter-queue"></a>死信队列中的病毒消息处理  

 病毒消息处理在死信队列中可用，但有一些条件。 因为在从系统的死信队列中进行读取时，您无法从系统队列创建子队列，所以不能将 `ReceiveErrorHandling` 设置为 `Move`。 请注意，如果您正在从自定义死信队列中读取，则可以拥有子队列，因此 `Move` 对于病毒消息为有效处理。  
  
 在将 `ReceiveErrorHandling` 设置为 `Reject` 以及从自定义死信队列中进行读取时，病毒消息将放入系统的死信队列。 如果从系统的死信队列中进行读取，则将丢弃（清除）该消息。 在 MSMQ 的系统死信队列中执行拒绝将丢弃（清除）该消息。  
  
## <a name="example"></a>示例  

 下面的示例演示如何创建死信队列以及如何使用该队列来处理过期消息。 该示例基于 [如何：与 WCF 终结点交换排队消息](how-to-exchange-queued-messages-with-wcf-endpoints.md)中的示例。 下面的示例演示如何将客户端代码写入将死信队列用于每个应用程序的订单处理服务。 该示例还显示如何处理死信队列中的消息。  
  
 下面是为每个应用程序指定死信队列的客户端的代码。  
  
 [!code-csharp[S_DeadLetter#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_deadletter/cs/client.cs#1)]
 [!code-vb[S_DeadLetter#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_deadletter/vb/client.vb#1)]  
  
 下面是客户端配置文件的代码。  

 下面是死信队列中的服务处理消息的代码。  
  
 [!code-csharp[S_DeadLetter#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_deadletter/cs/dlservice.cs#3)]
 [!code-vb[S_DeadLetter#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_deadletter/vb/dlservice.vb#3)]  
  
 下面是死信队列服务配置文件的代码。  

## <a name="see-also"></a>另请参阅

- [队列概述](queues-overview.md)
- [如何：使用 WCF 终结点交换排队消息](how-to-exchange-queued-messages-with-wcf-endpoints.md)
- [病毒消息处理](poison-message-handling.md)
