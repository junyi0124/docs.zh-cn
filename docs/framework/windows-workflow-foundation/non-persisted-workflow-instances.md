---
title: 非持久工作流实例
ms.date: 03/30/2017
ms.assetid: 5e01af77-6b14-4964-91a5-7dfd143449c0
ms.openlocfilehash: 315d791585ace6ce4adf281abbba0a4c8c72d75a
ms.sourcegitcommit: d6e27023aeaffc4b5a3cb4b88685018d6284ada4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/09/2019
ms.locfileid: "67663045"
---
# <a name="non-persisted-workflow-instances"></a>非持久工作流实例

在创建工作流的新实例（该实例在 <xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 中保留其状态）时，服务主机将在实例存储中为该服务实例创建一个项。 随后，当第一次持久化工作流实例时，<xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 将存储当前实例状态。 如果工作流承载于 Windows 进程激活服务中，则服务部署数据也将在第一次持久化实例时写入实例存储中。

只要工作流实例未保持不变，是在**非持久化**状态。 当处于此状态时，将无法在发生应用程序域回收、主机故障或计算机故障后恢复工作流实例。

## <a name="the-non-persisted-state"></a>非持久状化态

尚未持久化的持久工作流实例在下列情况下将保持非持久化状态：

- 在第一次持久化工作流实例之前，服务主机发生崩溃。 工作流实例将保留在实例存储中且未恢复。 如果获得关联的消息，则该工作流实例会再一次进入活动状态。

- 该工作流实例第一次持久化之前遇到异常。 将发生以下情况，具体取决于返回的 <xref:System.Activities.UnhandledExceptionAction>：

  - <xref:System.Activities.UnhandledExceptionAction> 设置为<xref:System.Activities.UnhandledExceptionAction.Abort>:异常发生时，将服务部署信息写入实例存储区，和是从内存中卸载工作流实例。 工作流实例将保持非持久化状态且无法重新加载。

  - <xref:System.Activities.UnhandledExceptionAction> 设置为<xref:System.Activities.UnhandledExceptionAction.Cancel>或<xref:System.Activities.UnhandledExceptionAction.Terminate>:当发生异常，将服务部署信息写入实例存储区，且活动实例状态设置为<xref:System.Activities.ActivityInstanceState.Closed>。

若要最大程度地降低遇到已卸载非持久化工作流实例的风险，建议您在工作流生命周期的早期持久化工作流。

## <a name="detection-and-removal-of-non-persisted-instances"></a>非持久化实例的检测和移除

<xref:System.Activities.DurableInstancing.SqlWorkflowInstanceStore> 将不移除实例存储中的任何非持久化工作流实例。 它也不会移除任何具有关联的非持久化工作流实例的已过期锁定所有者。

建议管理员定期检查实例存储中的非持久化实例。 只要管理员知道此工作流将不接收关联消息，就可以从实例存储中移除这些实例。 例如，如果某个实例在数据库中已经存在了数月，并且知道工作流通常只有几天的生存期，则可以放心地认为这是一个已经崩溃的已初始化的实例。

若要在 SQL 工作流实例存储中查找非持久化实例，可使用以下 SQL 查询：

- 此查询将查找所有未持久化的实例，并返回这些实例的 ID 和创建时间（用 UTC 时间格式存储）。

  ```sql
  select InstanceId, CreationTime
      from [System.Activities.DurableInstancing].[Instances]
      where IsInitialized = 0
  ```

- 此查询将查找所有未持久化且未加载的实例，并返回这些实例的 ID 和创建时间（用 UTC 时间格式存储）。

  ```sql
  select InstanceId, CreationTime
      from [System.Activities.DurableInstancing].[Instances]
      where IsInitialized = 0
          and CurrentMachine is NULL
  ```

- 此查询将查找所有未持久化的且挂起的实例，并返回这些实例的 ID、创建时间（用 UTC 时间格式存储）、挂起原因和异常名称。

  ```sql
  select InstanceId, CreationTime, SuspensionReason, SuspensionExceptionName
      from [System.Activities.DurableInstancing].[Instances]
      where IsInitialized = 0
          and IsSuspended = 1
  ```

删除非持久化实例时务必小心。 通常，移除由 <xref:System.ServiceModel.Activities.WorkflowServiceHost> 创建的已挂起或未加载的非持久化实例是安全的。 可通过使用以下 SQL 命令（替换正确的实例 ID）将这些特定实例从 `[System.Activities.DurableInstancing].[Instances]` 视图中删除，从而从存储中删除这些实例。

```sql
delete [System.Activities.DurableInstancing].[Instances]
    where InstanceId=’078a9bc4-ada5-4f9e-8cce-b0eb0009995f’
```

> [!WARNING]
> 建议不要移除所有非持久化实例，因为其中包括那些刚刚创建且尚未持久化的实例。
