---
title: 使用单阶段提交和可提升的单阶段通知进行优化
description: 使用单阶段提交和可提升的单阶段通知优化性能。 了解 .NET 中的 System. 事务基础结构。
ms.date: 03/30/2017
ms.assetid: 57beaf1a-fb4d-441a-ab1d-bc0c14ce7899
ms.openlocfilehash: 89ce82e673340c93254983c078f78a2501129383
ms.sourcegitcommit: 6219b1e1feccb16d88656444210fed3297f5611e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/22/2020
ms.locfileid: "85141973"
---
# <a name="optimization-using-single-phase-commit-and-promotable-single-phase-notification"></a>使用单阶段提交和可提升的单阶段通知进行优化

本主题描述 <xref:System.Transactions> 基础结构所提供的用于优化性能的机制。

## <a name="promotable-single-phase-enlistment"></a>可提升的单阶段登记

<xref:System.Transactions> 基础结构在最多包含一个持久资源或包含多个可变资源的单个应用程序域中管理事务。 由于 <xref:System.Transactions> 基础结构仅使用应用程序内的域调用，因此会获得最佳吞吐量和性能。

但如果将事务提供给其他应用程序域（包括跨进程和计算机边界的应用程序域）中的其他对象，或者要登记其他持久资源管理器，则 <xref:System.Transactions> 基础结构会自动将事务升级为由 MSDTC 进行管理。 由 MSDTC 管理的事务不像由 <xref:System.Transactions> 基础结构管理事务那样有利于性能的提高。

为了优化性能，<xref:System.Transactions> 基础结构提供了可提升的单阶段登记 (PSPE)；使用该功能，无需将位于不同应用程序域、进程或计算机中的单个远程持久资源升级为 MSDTC 事务，就可使其参与 <xref:System.Transactions>。 此资源管理器 (RM) 可承载和“拥有”以后可在需要时升级为分布式事务（即 MSDTC 事务）的事务。 这减少了使用 MSDTC 的机会。

此特定的资源管理器通常具有自己的内部非分布式事务，并且需要支持在运行时将这些事务转换为分布式事务。 例如，SQL Server 2005 就是这样的资源管理器。 在这种情况下，<xref:System.Transactions> 基础结构只需监视事务是否需要升级就可发挥积极的管理作用。 若要支持 <xref:System.Transactions> 基础结构与资源管理器之间的交互，后者需要实现 <xref:System.Transactions.IPromotableSinglePhaseNotification> 接口。

<xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A> 方法用于登记以后可升级的单个持久资源。 此方法确保了可根据需要升级登记。 如果该登记成功，则 RM 会创建其内部事务，并将该事务与 <xref:System.Transactions> 事务关联。 如果 PSPE 登记失败，则 RM 应改用 <xref:System.Transactions.Transaction.EnlistDurable%2A> 方法进行登记。 当事务已是分布式事务或者其他 PSPE 已执行 PSPE 登记时，可能就无法用 PSPE 登记。

登记后，分别通过调用 <xref:System.Transactions> 或 <xref:System.Transactions.IPromotableSinglePhaseNotification.SinglePhaseCommit%2A> 方法，将客户端用于提交或中止 <xref:System.Transactions.IPromotableSinglePhaseNotification.Rollback%2A> 事务的调用转换为对资源管理器的调用。

如果 <xref:System.Transactions> 事务从不需要升级，则在提交该事务时，RM 会接收到 <xref:System.Transactions.IPromotableSinglePhaseNotification.SinglePhaseCommit%2A> 通知。 这样，就可以提交最初创建的内部事务。

如果需要升级 <xref:System.Transactions> 事务（例如，要支持多个 RM），则 <xref:System.Transactions> 会通过在 <xref:System.Transactions.ITransactionPromoter.Promote%2A> 接口（从其派生 <xref:System.Transactions.ITransactionPromoter> 接口）上调用 <xref:System.Transactions.IPromotableSinglePhaseNotification> 方法，向资源管理器发出通知。 然后，资源管理器在内部将该事务从本地事务（不要求进行日志记录）转换为能够参与 DTC 事务的事务对象，并将其与已执行的工作关联。 在请求提交该事务时，事务管理器仍会将 <xref:System.Transactions.IPromotableSinglePhaseNotification.SinglePhaseCommit%2A> 通知发送给资源管理器，而后者会提交在升级期间创建的分布式事务。

> [!NOTE]
> **TransactionCommitted**跟踪（在升级的事务上调用 Commit 时生成）包含 DTC 事务的活动 ID。

有关管理升级的详细信息，请参阅[事务管理升级](transaction-management-escalation.md)。

## <a name="transaction-management-escalation-scenario"></a>事务管理升级方案

下面的方案演示如何将一个事务升级为分布式事务，该方案将 <xref:System.Data> 命名空间用作资源管理器的“代理”。 该方案假定该事务中已包含一个至数据库的 <xref:System.Data> 连接 CN1，并且应用程序希望包含另一个 <xref:System.Data> 连接 CN2。 该事务必须作为完全分布式的两阶段提交事务升级到 DTC。

在此方案中

1. CN1 调用 <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A> 方法以在该事务中登记。 这样，该事务仍是本地事务，并且该事务上没有其他可提升的登记，因此 <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A> 调用会成功执行。

2. 当第二个连接 CN2 调用 <xref:System.Transactions.Transaction.EnlistPromotableSinglePhase%2A> 时，该调用会因包含另一个可提升的登记而失败。 因此，CN2 必须获取 DTC 事务才能将它传递给 SQL。 若要实现此目的，CN2 将使用 <xref:System.Transactions.TransactionInterop> 类所提供的方法之一生成一种可提供给 SQL 的事务格式。

3. <xref:System.Transactions> 在由 CN1 实现的 <xref:System.Transactions.ITransactionPromoter.Promote%2A> 接口上调用 <xref:System.Transactions.ITransactionPromoter> 方法。

4. 此时，CN1 使用 SQL 2005 和 <xref:System.Data> 特定的某一机制升级该事务。

5. <xref:System.Transactions.ITransactionPromoter.Promote%2A> 方法中的返回值是一个包含事务的传播标记的字节数组。  <xref:System.Transactions>使用此传播令牌创建可合并到本地事务中的 DTC 事务。

6. 此时，CN2 可使用通过调用 <xref:System.Transactions.TransactionInterop> 方法之一而接收到的数据，来将事务传入 SQL 中。

7. 现在，这两个连接都登记到了 DTC 分布式事务中。

## <a name="single-phase-commit-optimization"></a>单阶段提交优化

单阶段提交协议在运行时更有效，因为使用它时，无需进行任何显式协调即可执行所有更新。 若要利用此优化，应对资源使用 <xref:System.Transactions.ISinglePhaseNotification> 接口来实现资源管理器，并使用 <xref:System.Transactions.Transaction.EnlistDurable%2A> 或 <xref:System.Transactions.Transaction.EnlistVolatile%2A> 方法在事务中登记。 具体而言， *EnlistmentOptions*参数应该等于， <xref:System.Transactions.EnlistmentOptions.None> 以确保执行单阶段提交。

由于 <xref:System.Transactions.ISinglePhaseNotification> 接口是从 <xref:System.Transactions.IEnlistmentNotification> 接口派生的，因此如果 RM 不适合执行单阶段提交，它仍可接收两阶段提交通知。 如果 RM 从 TM 接收到 <xref:System.Transactions.ISinglePhaseNotification.SinglePhaseCommit%2A> 通知，它应尝试完成提交所需的工作，并在 <xref:System.Transactions.SinglePhaseEnlistment.Committed%2A> 参数上调用 <xref:System.Transactions.SinglePhaseEnlistment.Aborted%2A>、<xref:System.Transactions.SinglePhaseEnlistment.InDoubt%2A> 或 <xref:System.Transactions.SinglePhaseEnlistment> 方法，来相应地通知事务管理器是提交还是回滚事务。 在此阶段中，登记时的 <xref:System.Transactions.Enlistment.Done%2A> 响应表示 ReadOnly 语义。 因此，不应答复 <xref:System.Transactions.Enlistment.Done%2A> 以及任何其他方法。

如果只有一个可变登记并且没有持久登记，则可变登记会收到 SPC 通知。 如果存在任何易失性登记并且只有一个持久登记，则可变登记会接收2PC。 完成提交后，持久登记会接收到 SPC 通知。

## <a name="see-also"></a>另请参阅

- [在事务中将资源登记为参与者](enlisting-resources-as-participants-in-a-transaction.md)
- [在单阶段和多阶段中提交事务](committing-a-transaction-in-single-phase-and-multi-phase.md)
