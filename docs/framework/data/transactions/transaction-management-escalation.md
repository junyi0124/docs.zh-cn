---
title: 事务管理升级
description: 了解 .NET 中的事务管理升级，这是将事务从一个事务管理器组件迁移到另一个组件的过程。
ms.date: 03/30/2017
ms.assetid: 1e96331e-31b6-4272-bbbd-29ed1e110460
ms.openlocfilehash: 83abca2e2c9acf53ce3f72d6bc55a82964b99cb5
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91155433"
---
# <a name="transaction-management-escalation"></a>事务管理升级

Windows 承载一组服务和模块，它们共同构成了一个事务管理器。 事务管理升级描述将事务从一个事务管理器组件迁移到另一个事务管理器组件的过程。  
  
 <xref:System.Transactions> 包含一个事务管理器组件，该组件可协调最多涉及单个持久资源或多个可变资源的事务。 由于事务管理器仅使用应用程序内的域调用，因此会获得最佳性能。 开发人员无需直接与事务管理器交互。 而是由 <xref:System.Transactions> 命名空间提供一个公共基础结构来定义接口、常见行为和帮助器类。  
  
 如果要将事务提供给其他应用程序域中的对象 (包括在同一台计算机上的进程和计算机边界) ，则 <xref:System.Transactions> 基础结构会自动将事务升级为由 Microsoft 分布式事务处理协调器 (MSDTC) 进行管理。 此外，如果登记其他持久资源管理器，也会执行此升级。 在升级过程中，事务在其提升状态下仍被托管，直到其完成升级为止。  
  
 在 <xref:System.Transactions> 事务和 MSDTC 事务之间，存在可通过可提升的单阶段登记 (PSPE) 而提供的中间类型的事务。 PSPE 是 <xref:System.Transactions> 中用于性能优化的另一种重要机制。 通过该机制，无需将位于不同的应用程序域、进程或计算机中的远程持久资源升级为 MSDTC 事务，就可使其参与 <xref:System.Transactions> 事务。 有关 PSPE 的详细信息，请参阅 [在事务中将资源登记为参与者](enlisting-resources-as-participants-in-a-transaction.md)。  
  
## <a name="how-escalation-is-initiated"></a>如何启动升级  

 由于 MSDTC 位于单独的进程中，而将事务升级到 MSDTC 会导致跨进程发送消息，因此升级事务会降低性能。 若要改善性能，应延迟或避免升级到 MSDTC；因此，您需要了解启动升级的方式和时间。  
  
 只要 <xref:System.Transactions> 基础结构处理支持单阶段通知的多个可变资源和最多一个的持久资源，事务就会保留在 <xref:System.Transactions> 基础结构的所有权中。 事务管理器本身仅适用于位于同一应用程序域中的资源，以及不需要日志记录（将事务结果写入磁盘）的资源。 当满足下列任一条件时，<xref:System.Transactions> 基础结构就会执行将事务的所有权传送到 MSDTC 的升级：  
  
- 在事务中至少登记了一个不支持单阶段通知的持久资源。  
  
- 在事务中至少登记了两个支持单阶段通知的持久资源。 例如，使用 SQL Server 2005 登记单一连接不会导致事务提升。 但是，每当你打开与导致数据库登记的 SQL Server 2005 数据库的第二个连接时， <xref:System.Transactions> 基础结构就会检测到它是事务中的第二个持久性资源，并将其升级为 MSDTC 事务。  
  
- 调用请求来将事务“封送”到其他应用程序域或其他进程。 例如，跨应用程序域边界序列化事务对象。 事务对象是按值封送的，这意味着跨应用程序域边界（甚至在同一进程中）传递它的任何尝试都会导致序列化该事务对象。 您可以通过调用采用 <xref:System.Transactions.Transaction> 作为参数的远程方法来传递事务对象，也可以尝试访问远程事务所提供的组件。 这会序列化事务对象并使其升级，比如跨应用程序域序列化事务时。 将分布该事务，并且本地事务管理器将不再胜任。  
  
 下表列出了在升级期间可能会引发的所有可能的异常。  
  
|例外类型|条件|  
|--------------------|---------------|  
|<xref:System.InvalidOperationException>|尝试升级隔离级别等于 <xref:System.Transactions.IsolationLevel.Snapshot> 的事务。|  
|<xref:System.Transactions.TransactionAbortedException>|事务管理器已关闭。|  
|<xref:System.Transactions.TransactionException>|升级失败并且应用程序已中止。|
