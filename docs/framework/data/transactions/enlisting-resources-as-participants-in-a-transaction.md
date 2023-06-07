---
title: 在事务中将资源登记为参与者
description: 在 .NET 事务中将资源登记为参与者。 事务中的每个资源都由资源管理器进行管理，由事务管理器进行协调。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 786a12c2-d530-49f4-9c59-5c973e15a11d
ms.openlocfilehash: c3c777593750b3a4f05ae97ed6e26e19f58e4d72
ms.sourcegitcommit: 6219b1e1feccb16d88656444210fed3297f5611e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2020
ms.locfileid: "85141870"
---
# <a name="enlisting-resources-as-participants-in-a-transaction"></a>在事务中将资源登记为参与者

参与事务的每个资源都由资源管理器进行管理，而后者的操作则由事务管理器进行协调。 这一协调通过通知来执行，这些通知会提供给已通过事务管理器在事务中登记的订户。

本主题介绍如何在事务中登记一个资源（或多个资源）以及不同的登记类型。 [在单阶段和多阶段中提交事务](committing-a-transaction-in-single-phase-and-multi-phase.md)主题介绍了如何在已登记的资源之间协调事务提交。

## <a name="enlisting-resources-in-a-transaction"></a>在事务中登记资源

资源若要参与事务，它必须在事务中进行登记。 <xref:System.Transactions.Transaction>类定义一组方法，这些方法的名称以提供此功能的**征用**开头。 不同的**登记**方法对应于资源管理器可能具有的不同类型的登记。 具体来说，<xref:System.Transactions.Transaction.EnlistVolatile%2A> 方法用于登记可变资源，而 <xref:System.Transactions.Transaction.EnlistDurable%2A> 方法则用于登记持久资源。 资源管理器的持久性（反之为可变性）是指资源管理器是否支持故障恢复。 如果资源管理器支持故障恢复，则它会在第 1 阶段（准备阶段）将数据保存到持久存储区中；这样，一旦资源管理器出现故障，它就可在恢复时在事务中重新登记，并根据从 TM 接收到的通知执行适当的操作。 通常，可变资源管理器管理如内存中的数据结构之类的可变资源（如内存中的事务处理哈希表），而持久资源管理器则管理具有更持久的后备存储区的资源（例如，其后备存储区为磁盘的数据库）。

为了简单起见，在根据资源的持久性支持决定是使用 <xref:System.Transactions.Transaction.EnlistDurable%2A> 还是 <xref:System.Transactions.Transaction.EnlistVolatile%2A> 方法后，应为资源管理器实现 <xref:System.Transactions.IEnlistmentNotification> 接口，从而将资源登记为参与两阶段提交 (2PC)。 有关2PC 的详细信息，请参阅[在单阶段和多阶段提交事务](committing-a-transaction-in-single-phase-and-multi-phase.md)。

单个参与者可以多次调用 <xref:System.Transactions.Transaction.EnlistDurable%2A> 和 <xref:System.Transactions.Transaction.EnlistVolatile%2A> 来登记到其中的某一协议。

### <a name="durable-enlistment"></a>持久登记

<xref:System.Transactions.Transaction.EnlistDurable%2A> 方法用于登记要作为持久资源参与事务的资源管理器。  如果持久资源管理器在事务执行期间关闭，则它在重新联机后，应在它作为参与者且未完成第 2 阶段的所有事务中重新登记（使用 <xref:System.Transactions.TransactionManager.Reenlist%2A> 方法）来执行恢复，并在完成恢复处理后调用 <xref:System.Transactions.TransactionManager.RecoveryComplete%2A>。 有关恢复的详细信息，请参阅[执行恢复](performing-recovery.md)。

<xref:System.Transactions.Transaction.EnlistDurable%2A> 方法都采用 <xref:System.Guid> 对象作为其第一个参数。 事务管理器使用 <xref:System.Guid> 将持久登记与特定的资源管理器关联。 因此，资源管理器在重新启动后必须统一使用同一 <xref:System.Guid> 来标识自身，即使跨不同的资源管理器时也是如此；否则恢复操作就可能失败。

<xref:System.Transactions.Transaction.EnlistDurable%2A> 方法的第二个参数是对资源管理器所实现的用于接收事务通知的对象的引用。 您所使用的重载会向事务管理器通知资源管理器是否支持单阶段提交 (SPC) 优化。 大多数情况下，应实现 <xref:System.Transactions.IEnlistmentNotification> 接口来参与两阶段提交 (2PC)。 但如果要优化提交过程，可考虑实现 <xref:System.Transactions.ISinglePhaseNotification> 接口来参与 SPC。 有关 SPC 的详细信息，请参阅[使用单阶段提交和可提升的单阶段通知](optimization-spc-and-promotable-spn.md)[在单阶段和多阶段和优化中提交事务](committing-a-transaction-in-single-phase-and-multi-phase.md)。

第三个参数是 <xref:System.Transactions.EnlistmentOptions> 枚举，该枚举的值可以为 <xref:System.Transactions.EnlistmentOptions.None> 或 <xref:System.Transactions.EnlistmentOptions.EnlistDuringPrepareRequired>。 如果将该值设置为 <xref:System.Transactions.EnlistmentOptions.EnlistDuringPrepareRequired>，则这种登记类型可在从事务管理器接收到准备通知时登记附加资源管理器。 但是，您应清楚这种登记类型不适合执行单阶段提交优化。

### <a name="volatile-enlistment"></a>可变登记

管理可变资源（如缓存）的参与者应使用 <xref:System.Transactions.Transaction.EnlistVolatile%2A> 方法进行登记。 此类对象可能无法获取事务的结果，或者在系统出现故障后可能无法恢复它们所参与的任何事务的状态。

如前面所述，资源管理器在管理内存中的可变资源时应进行可变登记。 使用 <xref:System.Transactions.Transaction.EnlistVolatile%2A> 的优点之一就是不会强制执行不必要的事务升级。 有关事务升级的详细信息，请参阅[事务管理升级](transaction-management-escalation.md)主题。 登记波动性意味着事务管理器处理登记的方式和事务管理器预期的资源管理器的预期差别。 这是因为可变资源管理器不执行恢复。 <xref:System.Transactions.Transaction.EnlistVolatile%2A> 方法不采用 <xref:System.Guid> 参数，因为可变资源管理器不执行恢复，并且不会调用需要 <xref:System.Transactions.TransactionManager.Reenlist%2A> 的 <xref:System.Guid> 方法。

与持久登记一样，无论使用哪种重载方法进行登记，都会向事务管理器指示资源管理器是否支持单阶段提交优化。 由于可变资源管理器不能执行恢复，因此在准备阶段不会为可变登记写入任何恢复信息。 因此，调用 <xref:System.Transactions.PreparingEnlistment.RecoveryInformation%2A> 方法会导致 <xref:System.InvalidOperationException>。

下面的示例演示如何使用 <xref:System.Transactions.Transaction.EnlistVolatile%2A> 方法在事务中将此类对象登记为参与者。

[!code-csharp[Tx_Enlist#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/tx_enlist/cs/enlist.cs#1)]
[!code-vb[Tx_Enlist#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/tx_enlist/vb/enlist.vb#1)]

### <a name="optimizing-performance"></a>优化性能

<xref:System.Transactions.Transaction> 类还提供了 <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A> 方法来登记可提升的单阶段登记 (PSPE)。 这使持久资源管理器 (RM) 可承载和“拥有”以后可在需要时升级为由 MSDTC 进行管理的事务。 有关此内容的详细信息，请参阅[使用单阶段提交和可提升的单阶段通知进行优化](optimization-spc-and-promotable-spn.md)。

## <a name="see-also"></a>另请参阅

- [使用单阶段提交和可提升的单阶段通知进行优化](optimization-spc-and-promotable-spn.md)
- [在单阶段和多阶段中提交事务](committing-a-transaction-in-single-phase-and-multi-phase.md)
