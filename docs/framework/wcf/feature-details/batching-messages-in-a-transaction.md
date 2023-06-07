---
title: 在事务中对消息进行批处理
ms.date: 03/30/2017
helpviewer_keywords:
- batching messages [WCF]
ms.assetid: 53305392-e82e-4e89-aedc-3efb6ebcd28c
ms.openlocfilehash: c18d5a36f4263a93589b75129517d66df80ce463
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96247455"
---
# <a name="batching-messages-in-a-transaction"></a>在事务中对消息进行批处理

排队的应用程序使用事务来确保消息的正确性以及传递的可靠性。 但是，事务是一种成本较高的操作，可能会大幅度降低消息吞吐量。 提高消息吞吐量的一个方法是让一个应用程序在单个事务内读取和处理多个消息。 这需要在性能和恢复之间进行权衡；随着批处理中消息数量的增加，事务回滚时所需完成的恢复工作的工作量也会增加。 需要注意的是，在事务中对消息进行批处理与会话是不同的。 *会话* 是由单个应用程序处理并作为单个单元提交的相关消息的分组。 当必须将一组相关消息一起进行处理的时候，通常使用会话。 这方面的一个例子是在线购物网站。 *批处理* 用于以增加消息吞吐量的方式处理多个不相关的消息。 有关会话的详细信息，请参阅 [将排队消息分组到会话中](grouping-queued-messages-in-a-session.md)。 批处理中的消息也是由单个应用程序处理，并且作为一个单元提交，但批处理中的消息之间可能没有任何关系。 在事务中对消息进行批处理是一种优化方法，它不会改变应用程序的运行方式。  
  
## <a name="entering-batching-mode"></a>进入批处理模式  

 <xref:System.ServiceModel.Description.TransactedBatchingBehavior> 终结点行为对批处理进行控制。 将此终结点行为添加到服务终结点会告诉 Windows Communication Foundation (WCF) 在事务中对消息进行批处理。 并非所有消息都需要事务，因此，只会将需要事务的消息放入批中，而只会将使用和标记的操作发送的消息 `TransactionScopeRequired`  =  `true` `TransactionAutoComplete`  =  `true` 视为批处理。 如果对服务协定的所有操作都用和进行标记 `TransactionScopeRequired`  =  `false` `TransactionAutoComplete`  =  `false` ，则永远不会进入批处理模式。  
  
## <a name="committing-a-transaction"></a>提交事务  

 根据以下因素提交批处理事务：  
  
- `MaxBatchSize`. <xref:System.ServiceModel.Description.TransactedBatchingBehavior> 行为的一个属性。 此属性确定放入批处理中的消息的最大数量。 一旦达到此数量，批处理便会被提交。 该值并不是一个严格的限制，在接收到这一数量的消息之前也可以提交批处理。  
  
- `Transaction Timeout`. 当事务的超时时间已经过去 80% 之后，系统会提交批处理，并且创建一个新的批处理。 这表示如果为事务完成所分配的时间只剩下 20% 或更少，系统便会提交批处理。  
  
- `TransactionScopeRequired`. 当处理一批消息时，如果 WCF 找到了一批消息， `TransactionScopeRequired`  =  `false` 则它将提交该批，并使用和在收到第一条消息时重新打开新批 `TransactionScopeRequired`  =  `true` `TransactionAutoComplete`  =  `true` 。  
  
- 如果队列中不再有其他消息，则即使尚未达到 `MaxBatchSize` 或事务的超时时间尚未过去 80%，系统也会提交当前的批处理。  
  
## <a name="leaving-batching-mode"></a>离开批处理模式  

 如果批处理中的消息导致事务中止，则会执行以下步骤：  
  
1. 回滚整个消息批处理。  
  
2. 一次只读取一个消息，直到所读取的消息数量超过最大批次大小的两倍为止。  
  
3. 重新进入批处理模式。  
  
## <a name="choosing-the-batch-size"></a>选择批次大小  

 批次大小依应用程序而定。 经验性方法是达到应用程序的最佳批次大小的最好方式。 请一定牢记，在选择批次大小时，要根据应用程序的实际部署模型来选择大小。 例如，在部署应用程序时，如果需要位于远程计算机上的 SQL Server 以及一个横跨队列和该 SQL Server 的事务，则最好通过运行此确切配置来确定批次大小。  
  
## <a name="concurrency-and-batching"></a>并发和批处理  

 若要增加吞吐量，还可以同时运行多个批处理。 通过设置 `ConcurrencyMode.Multiple` 中的 `ServiceBehaviorAttribute` 可以启用并发批处理。  
  
 *服务限制* 是一种服务行为，用于指示可对服务进行的最大并发调用数。 在将其与批处理结合使用时，可以将其解释为可以运行的并发批处理的数量。 如果未设置服务限制，WCF 会将最大并发调用数默认为16。 因此，如果在默认情况下添加批处理行为，则最多会有 16 个批处理同时处于活动状态。 最好根据系统处理能力来调整服务遏制和批处理。 例如，如果队列中有 100 个消息并且将批次大小设置为 20 是比较理想的，那么将最大并发调用数设置为 16 是没有用处的，因为根据吞吐量，可能有 16 个事务处于活动状态，这与未启用批处理的结果类似。 因此，在对这些设置进行微调以提高性能时，要么干脆不使用并发批处理，否则就要使并发批处理具有正确的服务遏制大小。  
  
## <a name="batching-and-multiple-endpoints"></a>批处理和多个终结点  

 终结点由地址和协定组成。 可能有多个终结点共享同一个绑定。 两个终结点可以共享同一个绑定，并侦听统一资源标识符 (URI)，即队列地址。 如果两个终结点从同一个队列中进行读取，并且两个终结点都添加了事务处理批处理行为，则指定的批次大小可能会发生冲突。 使用在两个事务处理批处理行为之间指定的最小批次大小来实现批处理，可以解决该冲突。 在此方案中，如果其中一个终结点未指定事务处理批处理，则两个终结点都不会使用批处理。  
  
## <a name="example"></a>示例  

 下面的示例演示如何在配置文件中指定 `TransactedBatchingBehavior`。  
  
```xml  
<behaviors>
  <endpointBehaviors>
    <behavior name="TransactedBatchingBehavior"
              maxBatchSize="100" />
  </endpointBehaviors>
</behaviors>
```  
  
 下面的示例演示如何在代码中指定 <xref:System.ServiceModel.Description.TransactedBatchingBehavior>。  
  
```csharp
using (ServiceHost serviceHost = new ServiceHost(typeof(OrderProcessorService)))
{
     ServiceEndpoint sep = ServiceHost.AddServiceEndpoint(typeof(IOrderProcessor), new NetMsmqBinding(), "net.msmq://localhost/private/ServiceModelSamplesTransacted");
     sep.Behaviors.Add(new TransactedBatchingBehavior(100));

     // Open the ServiceHost to create listeners and start listening for messages.
    serviceHost.Open();
  
    // The service can now be accessed.
    Console.WriteLine("The service is ready.");
    Console.WriteLine("Press <ENTER> to terminate service.");
    Console.WriteLine();
    Console.ReadLine();
  
    // Close the ServiceHostB to shut down the service.
    serviceHost.Close();
}  
```  
  
## <a name="see-also"></a>另请参阅

- [队列概述](queues-overview.md)
- [在 WCF 中排队](queuing-in-wcf.md)
