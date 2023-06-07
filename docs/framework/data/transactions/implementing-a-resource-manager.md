---
title: 实现资源管理器
description: 在 .NET 中实现资源管理器。 资源管理器管理事务中使用的资源。 事务管理器协调资源管理器操作。
ms.date: 03/30/2017
ms.assetid: d5c153f6-4419-49e3-a5f1-a50ae4c81bf3
ms.openlocfilehash: e6370f6b544255ebdc402f06b7977d4a3a587c32
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91182891"
---
# <a name="implementing-a-resource-manager"></a>实现资源管理器

事务中使用的每个资源都由资源管理器进行管理，而后者的操作则由事务管理器进行协调。 资源管理器与事务管理器协调工作，为应用程序提供了原子性和隔离性的保证。 例如，Microsoft SQL Server、持久消息队列、内存中的哈希表都是资源管理器。  
  
 资源管理器可管理持久或可变数据。 资源管理器的持久性（反之为可变性）是指资源管理器是否支持故障恢复。 如果资源管理器支持故障恢复，则它会在第 1 阶段（准备阶段）将数据保存到持久存储区中；这样，一旦资源管理器出现故障，它就可在恢复时在事务中重新登记，并根据从 TM 接收到的通知执行适当的操作。 通常，可变资源管理器管理如内存中的数据结构之类的可变资源（如内存中的事务处理哈希表），而持久资源管理器则管理具有更持久的后备存储区的资源（例如，其后备存储区为磁盘的数据库）。  
  
 资源若要参与事务，它必须在事务中进行登记。 <xref:System.Transactions.Transaction>类定义一组方法，这些方法的名称以提供此功能的**征用**开头。 不同的 **登记** 方法对应于资源管理器可能具有的不同类型的登记。 具体来说，<xref:System.Transactions.Transaction.EnlistVolatile%2A> 方法用于登记可变资源，而 <xref:System.Transactions.Transaction.EnlistDurable%2A> 方法则用于登记持久资源。 为了简单起见，在根据资源的持久性支持决定是使用 <xref:System.Transactions.Transaction.EnlistDurable%2A> 还是 <xref:System.Transactions.Transaction.EnlistVolatile%2A> 方法后，应为资源管理器实现 <xref:System.Transactions.IEnlistmentNotification> 接口，从而将资源登记为参与两阶段提交 (2PC)。 有关2PC 的详细信息，请参阅 [在单阶段和多阶段提交事务](committing-a-transaction-in-single-phase-and-multi-phase.md)。  
  
 通过登记，资源管理器可确保在事务提交或中止时能够从事务管理器获取回调。 每个登记都有一个 <xref:System.Transactions.IEnlistmentNotification> 实例。 通常情况下，在每个事务中只执行一次登记，但资源管理器可选择在同一事务中执行多次登记。  
  
 登记后，资源管理器会对事务的请求做出响应。 持久资源管理器存储了足够的信息，可允许在它所管理的资源上撤消或重做事务的工作。 可以使用许多方法来实现此目的；最常采用的两种方法是保存数据版本和保存更改日志。  
  
 当应用程序提交事务时，事务管理器会启动两阶段提交协议。 事务管理器先向每个已登记的资源管理器询问它是否已准备好提交事务。 资源管理器必须准备好提交，即它自身准备好提交或中止事务。  
  
 在准备阶段，持久资源管理器会将旧数据和新数据记录到固定存储区中，以便即使系统出现故障，资源管理器也可恢复数据。 如果资源管理器准备就绪，就会向事务管理器通知有关它是提交还是中止事务的投票。 如果有任何资源管理器报告准备失败，则事务管理器会将回滚命令发送给每个资源管理器，并向应用程序指出提交失败。  
  
 在准备就绪后，资源管理器必须等待，直到它在第 2 阶段中从事务管理器获取提交或中止回调为止。 通常，整个准备和提交协议不到一秒即可完成。 如果发生系统或通信故障，则提交或中止通知可能无法在几分钟或几小时内送达。 在此期间，资源管理器将无法确定事务的结果， 即不知道事务是已提交还是已中止。 当资源管理器不确定事务状态时，它会通过使事务处于锁定状态来保留已修改的数据，从而将这些更改与任何其他事务隔离开。  
  
 如果资源管理器发生故障，则除了在发生故障之前已准备或提交的事务之外，其余所有已登记事务都将中止。 当持久资源管理器重新启动时，它会通过检索在准备阶段编写的准备信息重新构建资源的已提交状态，并相应地提交或中止这些事务。  
  
 总之，两阶段提交协议和资源管理器共同使事务具有了原子性和持久性。  
  
 <xref:System.Transactions.Transaction> 类还提供了 <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A> 方法来登记可提升的单阶段登记 (PSPE)。 这使持久资源管理器 (RM) 可承载和“拥有”以后可在需要时升级为由 MSDTC 进行管理的事务。 有关此内容的详细信息，请参阅 [使用单阶段提交和可提升的单阶段通知进行优化](optimization-spc-and-promotable-spn.md)。  
  
## <a name="in-this-section"></a>本节内容  

 下列主题概括了资源管理器通常所遵循的步骤。  
  
 [在事务中将资源登记为参与者](enlisting-resources-as-participants-in-a-transaction.md)  
  
 描述如何在事务中登记持久或可变资源。  
  
 [在单阶段和多阶段中提交事务](committing-a-transaction-in-single-phase-and-multi-phase.md)  
  
 描述资源管理器如何响应提交通知并准备提交。  
  
 [执行恢复](performing-recovery.md)  
  
 描述如何从故障中恢复持久资源管理器。  
  
 [访问资源时的安全信任级别](security-trust-levels-in-accessing-resources.md)  
  
 描述 System.Transactions 的三种信任级别如何限制对 <xref:System.Transactions> 所公开的资源类型的访问。  
  
 [使用单阶段提交和可提升的单阶段通知进行优化](optimization-spc-and-promotable-spn.md)  
  
 描述可用于实现资源管理器的最佳做法。
