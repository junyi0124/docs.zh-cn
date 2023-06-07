---
title: 在会话中对排队消息进行分组
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- queues [WCF]. grouping messages
ms.assetid: 63b23b36-261f-4c37-99a2-cc323cd72a1a
ms.openlocfilehash: 9ad3bd29535e14231d07b9e491e606f8349ca3ac
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96290057"
---
# <a name="grouping-queued-messages-in-a-session"></a>在会话中对排队消息进行分组

Windows Communication Foundation (WCF) 提供了一个会话，该会话可用于将一组相关消息组合在一起，以便由单个接收应用程序进行处理。 属于一个会话的消息必须属于同一事务。 因为所有消息都属于同一事务，所以如果有一个消息未能得到处理，整个会话都将回滚。 会话对于死信队列和病毒队列具有类似的行为。 在为会话配置的排队绑定上设置的生存时间 (TTL) 属性被应用于整个会话。 如果在 TTL 过期前仅发送了会话中的一部分消息，则会将整个会话都放到死信队列中。 与此类似，如果会话中有消息未能发送到应用程序队列中的应用程序，则会将整个会话都放到病毒队列（如果可用）中。  
  
## <a name="message-grouping-example"></a>消息分组示例  

 例如，在将订单处理应用程序作为 WCF 服务实现时，对消息进行分组很有用。 例如，某个客户端向此应用程序提交一个包含许多项的订单。 对于每个项，该客户端都要调用一次服务，每次调用都会产生一个要发送的消息。 有可能发生服务器 A 接收第一个项，而服务器 B 接收第二个项的情况。 每次添加一个项时，处理该项的服务器都必须找到相应的订单并将该项添加到订单中 — 这样做效率非常低。 如果只使用单个服务器来处理所有请求，仍然会非常低效，因为该服务器必须跟踪当前正在处理的所有订单并确定新项属于哪个订单。 将单个订单的所有请求分为一组可大大简化这样一个应用程序的实现。 客户端应用程序在一个会话中发送单个订单的所有项，因此当服务处理该订单时，它可以一次性处理整个会话。 \  
  
## <a name="procedures"></a>过程  
  
#### <a name="to-set-up-a-service-contract-to-use-sessions"></a>设置服务协定以使用会话  
  
1. 定义一个需要会话的服务协定。 <xref:System.ServiceModel.ServiceContractAttribute>通过指定以下内容，通过属性执行此操作：  
  
    ```csharp
    SessionMode=SessionMode.Required  
    ```  
  
2. 将该协定中的操作标记为单向操作，因为这些方法不返回任何结果。 <xref:System.ServiceModel.OperationContractAttribute>通过指定以下内容来完成此操作：  
  
    ```csharp  
    [OperationContract(IsOneWay = true)]  
    ```  
  
3. 实现该服务协定并指定一个类型为 <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode> 的 <xref:System.ServiceModel.InstanceContextMode.PerSession?displayProperty=nameWithType>。 这样，对于每个会话，仅实例化服务一次。  
  
    ```csharp  
    [ServiceBehavior(InstanceContextMode=InstanceContextMode.PerSession)]  
    ```  
  
4. 每个服务操作都需要一个事务。 可使用 <xref:System.ServiceModel.OperationBehaviorAttribute> 属性予以指定。 负责完成该事务的操作还应将 <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete> 设置为 `true`。  
  
    ```csharp  
    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
    ```  
  
5. 配置一个终结点，该终结点使用系统提供的 `NetMsmqBinding` 绑定。  
  
6. 使用 <xref:System.Messaging> 创建一个事务性队列。 还可以使用消息队列 (MSMQ) 或 MMC 创建该队列。 如果你这样做，请创建一个事务性队列。  
  
7. 使用 <xref:System.ServiceModel.ServiceHost> 为该服务创建一个服务主机。  
  
8. 打开服务主机使服务处于可用状态。  
  
9. 关闭服务主机。  
  
#### <a name="to-set-up-a-client"></a>设置客户端  
  
1. 创建一个要写入事务性队列的事务范围。  
  
2. 使用 [ ( # A0) ](../servicemodel-metadata-utility-tool-svcutil-exe.md) 工具创建 WCF 客户端。  
  
3. 下订单。  
  
4. 关闭 WCF 客户端。  
  
## <a name="example"></a>示例  
  
### <a name="description"></a>描述  

 下面的示例提供 `IProcessOrder` 服务的代码和一个使用此服务的客户端的代码。 其中显示了 WCF 如何使用排队会话来提供分组行为。  
  
### <a name="code-for-the-service"></a>服务代码  

 [!code-csharp[S_Msmq_Session#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_msmq_session/cs/service.cs#1)]
 [!code-vb[S_Msmq_Session#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_msmq_session/vb/service.vb#1)]  

### <a name="code-for-the-client"></a>客户端代码  

 [!code-csharp[S_Msmq_Session#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_msmq_session/cs/client.cs#3)]
 [!code-vb[S_Msmq_Session#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_msmq_session/vb/client.vb#3)]  

## <a name="see-also"></a>另请参阅

- [会话和队列](../samples/sessions-and-queues.md)
- [队列概述](queues-overview.md)
