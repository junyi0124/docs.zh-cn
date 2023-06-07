---
title: 持久性最佳做法
ms.date: 03/30/2017
ms.assetid: 6974c5a4-1af8-4732-ab53-7d694608a3a0
ms.openlocfilehash: 950a5d5c742b7882db93d71f3e7f205009f2a863
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96246142"
---
# <a name="persistence-best-practices"></a>持久性最佳做法

本文档介绍了针对与工作流持久性相关的工作流设计和配置的最佳实践。  
  
## <a name="design-and-implementation-of-durable-workflows"></a>持久工作流的设计和实现  

 通常，工作流会在较短的时段内执行工作，这些时段将与工作流因等待事件而处于空闲状态的时段交错。 此事件可以是消息或过期计时器之类的内容。 若要能在工作流的状态变成空闲时卸载工作流，则服务主机必须保留工作流实例。 仅在工作流实例未位于非持久性区域中时（例如，等待事务完成或等待异步回调）可能出现此情况。 若要允许卸载空闲工作流实例，工作流作者应仅对生存期较短的操作使用事务范围和异步活动。 具体而言，作者应尽可能缩短延迟活动在这些非持久性区域中的保留时间。  
  
 只有在可序列化工作流使用的所有数据类型时，才能保留工作流。 此外，持久化工作流中使用的自定义类型必须可通过 <xref:System.Runtime.Serialization.NetDataContractSerializer> 进行序列化，以便 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 能保留它们。  
  
 如果尚未保留某个工作流实例，则在主机或计算机出现故障时，将无法恢复此工作流实例。 通常，建议您在工作流生命周期的早期保留工作流实例。  
  
 如果工作流长时间处于忙碌状态，则建议您在工作流实例的忙碌时段中定期保留工作流实例。 可以通过在一系列使工作流实例处于忙碌状态的活动中添加 <xref:System.Activities.Statements.Persist> 活动来执行此操作。 通过这种方式，应用程序域回收、主机故障或计算机故障便不会导致系统回滚到忙碌时段的开头。 请注意，将 <xref:System.Activities.Statements.Persist> 活动添加到工作流可能会导致性能降低。  
  
 Windows Server App Fabric 大大简化了持久性的配置和使用。 有关详细信息，请参阅 [Windows Server App Fabric 持久性](/previous-versions/appfabric/ee677272(v=azure.10))  
  
## <a name="configuration-of-scalability-parameters"></a>可伸缩性参数的配置  

 可伸缩性和性能要求将决定以下参数的设置：  
  
- <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToPersist%2A>  
  
- <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToUnload%2A>  
  
- <xref:System.ServiceModel.Activities.Description.SqlWorkflowInstanceStoreBehavior.InstanceLockedExceptionAction%2A>  
  
 应根据当前方案设置这些参数，如下所示。  
  
### <a name="scenario-a-small-number-of-workflow-instances-that-require-optimal-response-time"></a>方案：少量工作流实例需要最佳响应时间  

 在此方案中，所有工作流实例应在其状态变成空闲时保持被加载。 将 <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToUnload%2A> 设置为较大的值。 使用此设置可阻止工作流实例在计算机间移动。 仅在满足以下一个或多个条件时使用此设置：  
  
- 工作流实例在其整个生存期内收到一条消息。  
  
- 所有工作流实例都在一台计算机上运行。  
  
- 工作流实例收到的所有消息都通过同一台计算机接收。  
  
 使用 <xref:System.Activities.Statements.Persist> 活动或将 <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToPersist%2A> 设置为 0，可在服务主机或计算机出现故障后恢复工作流实例。  
  
### <a name="scenario-workflow-instances-are-idle-for-long-periods-of-time"></a>方案：工作流实例长时间处于空闲状态  

 在此方案中，将 <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToUnload%2A> 设置为 0 可尽快释放资源。  
  
### <a name="scenario-workflow-instances-receive-multiple-messages-in-a-short-period-of-time"></a>方案：工作流实例在短时间内收到多条消息  

 在此方案中，如果这些消息都通过同一台计算机接收，则将 <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToUnload%2A> 设置为 60 秒。 这将阻止对工作流实例进行一系列快速卸载和加载。 同时也不会在内存中将该实例保留过长时间。  
  
 如果这些消息可通过不同的计算机接收，则将 <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToUnload%2A> 设置为 0，并将 <xref:System.ServiceModel.Activities.Description.SqlWorkflowInstanceStoreBehavior.InstanceLockedExceptionAction%2A> 设置为 BasicRetry 或 AggressiveRetry。 这样做将允许工作流实例由其他计算机加载。  
  
### <a name="scenario-workflow-uses-delay-activities-with-short-durations"></a>方案：工作流在较短时间内使用延迟活动  

 在此方案中，<xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 将定期轮询因过期的 <xref:System.Activities.Statements.Delay> 活动而应加载的实例的持久性数据库。 如果 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 找到一个将在下一个轮询时间间隔中过期的计时器，则 SQL 工作流实例存储将缩短轮询时间间隔。 之后，下一次轮询将在计时器过期后发生。 这样一来，SQL 工作流实例存储将实现运行时间长于轮询时间间隔（由 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.RunnableInstancesDetectionPeriod%2A> 设置）的计时器的高精度。 若要启用较短延迟的及时处理，工作流实例必须至少在内存中保留一个轮询时间间隔。  
  
 将 <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToPersist%2A> 设置为 0 可将过期时间写入持久性数据库。  
  
 将 <xref:System.ServiceModel.Activities.Description.WorkflowIdleBehavior.TimeToUnload%2A> 设置为大于或等于 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.RunnableInstancesDetectionPeriod%2A> 的值可在内存中将实例保留至少一个轮询时间间隔。  
  
 建议不要减少 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.RunnableInstancesDetectionPeriod%2A>，因为这将会增加持久性数据库的负载。 每台使用 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 的服务主机在每个检测周期轮询数据库一次。 如果服务主机的数目过多，则将 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.RunnableInstancesDetectionPeriod%2A> 设置为过短的时间间隔可能会导致系统性能下降。  
  
## <a name="configuring-the-sql-workflow-instance-store"></a>配置 SQL 工作流实例存储  

 SQL 工作流实例存储具有以下配置参数：  
  
 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.InstanceEncodingOption%2A>  
 此参数指示 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 压缩工作流实例状态。 压缩可减少持久性数据库中存储的数据的数量，并可在持久性数据库位于专用数据库服务器上时减少网络流量。 如果使用压缩，则需要计算资源来压缩和提取实例状态。 在大多数情况下，压缩可提高性能。  
  
 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.InstanceCompletionAction%2A>  
 此参数指示 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 保留或删除已完成的实例。 保留已完成的实例将提高持久性数据库存储需求并导致生成大型表，从而增加表查找时间。 除非已完成的实例是进行调试或审核所必需的，否则最好是指示 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 删除已完成的实例。 应仅在用户创建用于最终移除已删除的实例的过程时保留这些实例。 请注意，只要已完成的工作流示例位于实例存储中，就无法重用相关键。  
  
 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.RunnableInstancesDetectionPeriod%2A>  
 此参数定义了一个最大时间间隔，<xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 将按照此时间间隔轮询应在 <xref:System.Activities.Statements.Delay> 活动过期时加载的实例的持久性数据库。 如果 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 找到一个将在下一个轮询时间间隔中过期的计时器，则它将缩短轮询时间间隔，以使下一次轮询在该计时器过期后发生。 这样一来，SQL 工作流实例存储将实现运行时间长于 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.RunnableInstancesDetectionPeriod%2A> 的计时器的高精度。  
  
 建议不要减少 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.RunnableInstancesDetectionPeriod%2A>，因为这将会增加持久性数据库的负载。 每台使用 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 的服务主机在每个检测周期轮询数据库一次。 如果服务主机的数量过大，则将 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.RunnableInstancesDetectionPeriod%2A> 设置为过短的时间间隔可能会导致系统性能下降。  
  
 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.HostLockRenewalPeriod%2A>  
 此参数定义主机续订其持久性数据库中的锁定的间隔。 通过缩短此间隔，将使工作流实例能够在主机或计算机出现故障时更快地恢复。 另一方面，较短的锁定续订期将增加持久性数据库的负载。 每台使用 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 的服务主机将在每个续订期更新其数据库中的锁定一次。 如果一台计算机运行多个服务主机，请确保锁定续订所产生的负载不会降低系统的性能。 如果出现此情况，请考虑增加 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.HostLockRenewalPeriod%2A>。  
  
 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.InstanceLockedExceptionAction%2A>  
 如果启用此参数，则 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 将在下个 30 秒内重试加载锁定实例。 如果工作流在短时间内收到多条消息，并且这些消息通过不同的计算机接收，则将 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.InstanceLockedExceptionAction%2A> 设置为 BasicRetry 或 AggressiveRetry。  
  
 由于只要未尝试加载重试，加载重试机制就不会产生任何性能开销，因此应始终启用 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore.InstanceLockedExceptionAction%2A>。
