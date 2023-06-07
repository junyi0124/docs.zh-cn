---
title: 工作流持久性
description: .NET Framework 4.6.1 包含 SqlWorkflowInstanceStore 类，这允许将工作流数据和元数据保存到 SQL Server 数据库。
ms.date: 03/30/2017
helpviewer_keywords:
- programming [WF], persistence
ms.assetid: 39e69d1f-b771-4c16-9e18-696fa43b65b2
ms.openlocfilehash: 2184a159423a611a8936e900591a480ce7ef6ec8
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96293801"
---
# <a name="workflow-persistence"></a>工作流持久性

工作流持久性是指独立于进程或计算机信息持续捕获工作流实例的状态。 其目的在于：在发生系统故障时为工作流实例提供一个已知恢复点，通过卸载当前未主动执行工作的工作流实例来节省内存，或者将工作流实例状态从服务器场中的一个节点移至另一个节点。  
  
 持久性支持进程灵活性、可扩展性、故障恢复以及更有效地管理内存的功能。 持久性进程包括标识持久点、收集要保存的数据以及将数据实际存储最终委托给持久性提供程序。  
  
 若要为工作流启用持久性，需要将实例存储与 **WorkflowApplication** 或 **WorkflowServiceHost** 相关联，如 [如何：为工作流和工作流服务启用持久性](how-to-enable-persistence-for-workflows-and-workflow-services.md)中所述。 **WorkflowApplication** 和 **WorkflowServiceHost** 使用与之关联的实例存储区来启用将工作流实例保存到持久性存储区中，并基于存储在持久性存储区中的工作流实例数据将工作流实例加载到内存中。  
  
 [!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)]附带了 **SqlWorkflowInstanceStore** 类，这允许将有关工作流实例的数据和元数据保存到 SQL Server 2005 或 SQL Server 2008 数据库。 有关更多详细信息，请参阅 [SQL 工作流实例存储](sql-workflow-instance-store.md) 。  
  
 若要存储和加载应用程序特定数据以及与工作流实例相关的信息，您可以创建用于扩展 <xref:System.Activities.Persistence.PersistenceParticipant> 类的持久性参与者。 持久性参与者参与持久性进程的目的是：将可序列化的自定义数据保存到持久性存储中，将实例存储中的数据加载到内存中，以及在持久性事务下执行任何其他逻辑。 有关详细信息，请参阅 [持久性参与者](persistence-participants.md)。  
  
 Windows Server App Fabric 大大简化了配置持久性的过程。 有关详细信息，请参阅 [Windows Server App Fabric 的持久性概念](/previous-versions/appfabric/ee677272(v=azure.10))  
  
## <a name="implicit-persistence-points"></a>隐式持久点  

 以下列表包含当实例存储与工作流关联时持久化工作流所依据的条件的示例。  
  
- 当 **TransactionScope** 活动完成或 **TransactedReceiveScope** 活动完成时。  
  
- 当工作流实例处于空闲状态，并且在工作流主机上设置 **WorkflowIdleBehavior** 时。 例如，在使用消息传递活动或 **延迟** 活动时，会发生这种情况。  
  
- WorkflowApplication 处于空闲状态，并且应用程序的 **PersistableIdle** 属性设置为 **PersistableIdleAction**。  
  
- 当指示主机应用程序保存或卸载工作流实例时。  
  
- 当中止工作流实例或工作流实例结束时。  
  
- 当执行 **持久** 活动时。  
  
- 当使用 Windows Workflow Foundation 的早期版本开发的工作流实例在可互操作执行过程中遇到持久点时。  
  
## <a name="in-this-section"></a>本节内容  
  
- [SQL 工作流实例存储](sql-workflow-instance-store.md)  
  
- [实例存储区](instance-stores.md)  
  
- [持久性参与者](persistence-participants.md)  
  
- [持久性最佳做法](persistence-best-practices.md)  
  
- [非持久工作流实例](non-persisted-workflow-instances.md)  
  
- [暂停和继续工作流](pausing-and-resuming-a-workflow.md)
