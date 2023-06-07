---
title: 如何：查询非持有化实例
ms.date: 03/30/2017
ms.assetid: 294019b1-c1a7-4b81-a14f-b47c106cd723
ms.openlocfilehash: 54a442dab6700dda33cf05df1fb5c60a96bcbd56
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96279995"
---
# <a name="how-to-query-for-non-persisted-instances"></a>如何：查询非持有化实例

创建某个服务的新实例时，如果该服务已定义 SQL 工作流实例存储行为，则服务主机将在实例存储区中为该服务实例创建一个初始项。 随后，当该服务实例第一次持久化时，SQL 工作流实例存储行为会将当前实例状态与执行激活、恢复和控制操作所需的其他数据一起存储。

如果在创建某个实例的初始项后没有持久化该实例，则认为该服务实例处于非持久化状态。  可以查询和控制所有持久化服务实例。 不能查询和控制非持久化服务实例。 如果由于未处理的异常而挂起非持久化实例，则可以对该实例进行查询，但不能对它控制。

尚未持久化的持久服务实例在下列情况中将保持非持久化状态：

- 在第一次持久化实例之前，服务主机出现故障。 实例的初始项保留在实例存储区中。 该实例不可恢复。 如果获得关联的消息，则该实例再一次进入活动状态。

- 该实例第一次持久化之前遇到未经处理的异常。 出现下列情况

  - 如果 **UnhandledExceptionAction** 属性的值设置为 " **放弃**"，则会将服务部署信息写入实例存储区，并从内存中卸载该实例。 在持久性数据库中，该实例保持非持久化状态。

  - 如果 **UnhandledExceptionAction** 属性的值设置为 **AbandonAndSuspend**，则会将服务部署信息写入持久性数据库，并将实例状态设置为 "已 **挂起**"。 无法恢复、取消或终止该实例。 由于该实例尚未持久化，服务主机无法加载该实例，因此该实例的数据库项不完整。

  - 如果 **UnhandledExceptionAction** 属性的值设置为 " **取消** " 或 " **终止**"，则会将服务部署信息写入实例存储区，并将实例状态设置为 " **已完成**"。

下面的部分提供一些示例查询，用于在 SQL 持久性数据库中查找未持久化实例并从数据库删除这些实例。

## <a name="to-find-all-instances-not-persisted-yet"></a>查找所有尚未持久化的实例

下面的 SQL 查询返回所有尚未保存到持久性数据库的实例的 ID 和创建时间。

```sql
select InstanceId, CreationTime from [System.Activities.DurableInstancing].[Instances] where IsInitialized = 0;
```

## <a name="to-find-all-instances-not-persisted-yet-and-also-not-loaded"></a>查找所有尚未持久化并且未加载的实例

 下面的 SQL 查询返回所有未持久化且未加载的实例的 ID 和创建时间。

```sql
select InstanceId, CreationTime from [System.Activities.DurableInstancing].[Instances] where IsInitialized = 0 and CurrentMachine is NULL;
```

## <a name="to-find-all-suspended-instances-not-persisted-yet"></a>查找所有尚未持久化的挂起的实例

下面的 SQL 查询返回所有未持久化且处于挂起状态的实例的 ID、创建时间、挂起原因和挂起异常名称。

```sql
select InstanceId, CreationTime, SuspensionReason, SuspensionExceptionName from [System.Activities.DurableInstancing].[Instances] where IsInitialized = 0 and IsSuspended = 1;
```

## <a name="to-delete-non-persisted-instances-from-the-persistence-database"></a>从持久性数据库中删除非持久化实例

应定期检查实例存储区中的非持久化实例，如果确认该实例将不会接收到关联的消息，则从实例存储区中移除该实例。 如果某个实例在数据库中已经存在了数月，并且您知道工作流通常只有几天的生存期，则可以放心地认为这是一个已经崩溃的未初始化的实例。

通常，删除未挂起或未加载的非持久化实例是安全的。 不应删除 **所有** 非持久化实例，因为此实例集包括刚创建但尚未持久化的实例。 只应删除那些因加载实例的工作流服务主机引发了异常或实例本身引发了异常而留下的非持久化实例。

> [!WARNING]
> 从实例存储区删除非持久化实例可减少存储的大小，并可能提高存储操作的性能。
