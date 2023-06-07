---
title: 如何：使用 WorkflowServiceHost 配置永久性
ms.date: 03/30/2017
ms.assetid: e31cd4df-13a3-4a9a-9be8-5243e0055356
ms.openlocfilehash: 3d8b7183b11c138b8da1f04d9084f8b94b7dcaa6
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96257335"
---
# <a name="how-to-configure-persistence-with-workflowservicehost"></a>如何：使用 WorkflowServiceHost 配置永久性

本主题介绍如何使用配置文件配置 SQL 工作流实例存储功能，以便对 <xref:System.ServiceModel.Activities.WorkflowServiceHost> 中承载的工作流启用永久性。 使用 SQL 工作流实例存储功能之前，必须创建用于保存工作流实例的 SQL 数据库。 有关详细信息，请参阅 [如何：为工作流和工作流服务启用 SQL 持久性](../../windows-workflow-foundation/how-to-enable-sql-persistence-for-workflows-and-workflow-services.md)。  
  
### <a name="to-configure-the-sql-workflow-instance-store-in-configuration"></a>在配置中配置 SQL 工作流实例存储  
  
1. 可以通过 <xref:System.ServiceModel.Activities.Description.SqlWorkflowInstanceStoreBehavior>（一个用于通过 XML 配置更改设置的服务行为）配置 SQL 工作流实例存储的属性。 下面的配置示例演示如何使用 `sqlWorkflowInstanceStore` 配置文件中的 <> 行为元素来配置 SQL 工作流实例存储区。  
  
    ```xml  
    <serviceBehaviors>  
        <behavior name="">  
            <sqlWorkflowInstanceStore
                 connectionString="provider=System.Data.SqlClient;Data Source=(local);Initial Catalog=DefaultPersistenceProviderDb;Integrated Security=True;Async=true"  
                 instanceEncodingOption="GZip | None"  
                 instanceCompletionAction="DeleteAll | DeleteNothing"  
                 instanceLockedExceptionAction="NoRetry | SimpleRetry | AggressiveRetry"  
                 hostLockRenewalPeriod="00:00:30"
                 runnableInstancesDetectionPeriod="00:00:05">  
            </sqlWorkflowInstanceStore>  
        </behavior>  
    </serviceBehaviors>  
    ```  
  
     有关如何配置 SQL 工作流实例存储的详细信息，请参阅 [如何：为工作流和工作流服务启用 SQL 持久性](../../windows-workflow-foundation/how-to-enable-sql-persistence-for-workflows-and-workflow-services.md)。 有关 <> 行为元素的各个设置的详细信息 `sqlWorkflowInstanceStore` ，请参阅 [SQL 工作流实例存储](../../windows-workflow-foundation/sql-workflow-instance-store.md)。 Windows Server App Fabric 提供其自己的永久性存储。 有关详细信息，请参阅 [Windows Server App Fabric 暂留](/previous-versions/appfabric/ee677272(v=azure.10))。  
  
    > [!NOTE]
    > 上面的配置示例使用的是简化配置。 有关详细信息，请参阅 [简化配置](../simplified-configuration.md)  
  
### <a name="to-configure-the-sql-workflow-instance-store-in-code"></a>在代码中配置 SQL 工作流实例存储  
  
1. 可以通过 <xref:System.ServiceModel.Activities.Description.SqlWorkflowInstanceStoreBehavior>（一个用于通过代码更改设置的服务行为）配置 SQL 工作流实例存储的属性。 下面的示例演示如何在代码中使用 <xref:System.ServiceModel.Activities.Description.SqlWorkflowInstanceStoreBehavior> 行为元素来配置 SQL 工作流实例存储。  
  
    ```csharp  
    host.Description.Behaviors.Add(new SqlWorkflowInstanceStoreBehavior  
    {  
       ConnectionString = "provider=System.Data.SqlClient;Data Source=(local);Initial Catalog=DefaultPersistenceProviderDb;Integrated Security=True;Async=true",  
       InstanceEncodingOption = "GZip | None",  
       InstanceCompletionAction = "DeleteAll | DeleteNothing",  
       InstanceLockedExceptionAction = "NoRetry | SimpleRetry | AggressiveRetry",  
       HostLockRenewalPeriod = new TimeSpan(00, 00, 30),  
       RunnableInstancesDetectionPeriod = new TimeSpan(00, 00, 05)  
    });  
    ```  
  
     有关如何配置 SQL 工作流实例存储的详细信息，请参阅 [如何：为工作流和工作流服务启用 SQL 持久性](../../windows-workflow-foundation/how-to-enable-sql-persistence-for-workflows-and-workflow-services.md)。 有关行为元素的各个设置的详细信息 <xref:System.ServiceModel.Activities.Description.SqlWorkflowInstanceStoreBehavior> ，请参阅 [SQL 工作流实例存储](../../windows-workflow-foundation/sql-workflow-instance-store.md)。 Windows Server App Fabric 提供其自己的永久性存储。 有关详细信息，请参阅 [Windows Server App Fabric 暂留](/previous-versions/appfabric/ee677272(v=azure.10))。  
  
    > [!NOTE]
    > 上面的配置示例使用的是简化配置。 有关详细信息，请参阅 [简化配置](../simplified-configuration.md)  
  
     有关如何以编程方式配置持久性的示例，请参阅 [如何：为工作流和工作流服务启用持久性](../../windows-workflow-foundation/how-to-enable-persistence-for-workflows-and-workflow-services.md)。  
  
## <a name="see-also"></a>另请参阅

- [工作流服务](workflow-services.md)
- [工作流持久性](../../windows-workflow-foundation/workflow-persistence.md)
- [Windows Server App Fabric 持久性](/previous-versions/appfabric/ee677272(v=azure.10))
