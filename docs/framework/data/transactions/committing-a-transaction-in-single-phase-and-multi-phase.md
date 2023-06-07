---
title: 在单阶段和多阶段中提交事务
description: 了解如何在 .NET 中的一个或两个阶段提交事务。 如果事务涉及多个资源，则必须执行两阶段提交 (2PC) 。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 694ea153-e4db-41ae-96ac-9ac66dcb69a9
ms.openlocfilehash: f46c22294da4db017eceb0bfd0b5cb2bb093c0b5
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91147406"
---
# <a name="committing-a-transaction-in-single-phase-and-multi-phase"></a>在单阶段和多阶段中提交事务

事务中所使用的每个资源都由资源管理器 (RM) 进行管理，而资源管理器的操作则由事务管理器 (TM) 进行协调。 在 [事务主题中将资源登记为参与者](enlisting-resources-as-participants-in-a-transaction.md) 讨论了如何在事务中登记资源 (或多个资源) 。 本主题讨论如何在已登记的资源之间协调事务提交。  
  
 当事务结束时，应用程序会请求提交或回滚事务。 事务管理器必须消除类似于以下的风险：有些资源管理器投票决定提交，而有些资源管理器却投票决定回滚事务。  
  
 如果事务涉及到多个资源，则必须执行两阶段提交 (2PC)。 两阶段提交协议（准备阶段和提交阶段）可确保在事务结束时，会完全提交或完全回滚对所有资源的所有更改。 然后，会向所有参与者通知最终结果。 有关两阶段提交协议的详细讨论，请参阅书籍 "*事务处理：数据管理系统中的事务处理：概念和技术 (Morgan Kaufmann 系列) ISBN： 1558601902*" 为 Jim 灰色。  
  
 此外，还可以通过参与单阶段提交协议来优化事务的性能。 有关详细信息，请参阅 [使用单阶段提交和可提升的单阶段通知的优化](optimization-spc-and-promotable-spn.md)。  
  
 如果只希望得到事务结果的通知，而不希望参与投票，则应注册 <xref:System.Transactions.Transaction.TransactionCompleted> 事件。  
  
## <a name="two-phase-commit-2pc"></a>两阶段提交 (2PC)  

 在第一个事务阶段，事务管理器将查询每个资源以确定是应提交还是回滚事务。 在第二个事务阶段，事务管理器会向每个资源通知它的查询结果，以便它可以执行任何必要的清理。  
  
 若要参与这种事务，资源管理器必须实现 <xref:System.Transactions.IEnlistmentNotification> 接口，该接口提供在 2PC 期间由 TM 作为通知调用的方法。  下面的示例演示了一个这种实现的示例。  
  
 [!code-csharp[Tx_Enlist#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/tx_enlist/cs/enlist.cs#2)]
 [!code-vb[Tx_Enlist#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/tx_enlist/vb/enlist.vb#2)]  
  
### <a name="prepare-phase-phase-1"></a>准备阶段（第 1 阶段）  

 在从应用程序接收到 <xref:System.Transactions.CommittableTransaction.Commit%2A> 请求时，事务管理器会对每个已登记的资源调用 <xref:System.Transactions.IEnlistmentNotification.Prepare%2A> 方法来开始所有已登记参与者的准备阶段，以便获取事务上每个资源的投票。  
  
 实现 <xref:System.Transactions.IEnlistmentNotification> 接口的资源管理器应先实现 <xref:System.Transactions.IEnlistmentNotification.Prepare%28System.Transactions.PreparingEnlistment%29> 方法，如下面的简单示例所示。  
  
```csharp
public void Prepare(PreparingEnlistment preparingEnlistment)  
{  
     Console.WriteLine("Prepare notification received");  
     //Perform work  
  
     Console.Write("reply with prepared? [Y|N] ");  
     c = Console.ReadKey();  
     Console.WriteLine();  
  
     //If work finished correctly, reply with prepared  
     if ((c.KeyChar == 'Y') || (c.KeyChar == 'y'))  
     {  
          preparingEnlistment.Prepared();  
          break;  
     }  
  
     // otherwise, do a ForceRollback  
     else if ((c.KeyChar == 'N') || (c.KeyChar == 'n'))  
     {  
          preparingEnlistment.ForceRollback();  
          break;  
     }  
}  
```  
  
 当持久资源管理器接收到此调用时，它应记录事务的恢复信息（可通过检索 <xref:System.Transactions.PreparingEnlistment.RecoveryInformation%2A> 属性获取）以及在提交时完成事务所需的任何信息。 无需在 <xref:System.Transactions.IEnlistmentNotification.Prepare%2A> 方法中执行这一操作，因为 RM 可在辅助线程上执行此操作。  
  
 当 RM 完成其准备工作后，应调用 <xref:System.Transactions.PreparingEnlistment.Prepared%2A> 或 <xref:System.Transactions.PreparingEnlistment.ForceRollback%2A> 方法来投票决定是提交还是回滚。 请注意，<xref:System.Transactions.PreparingEnlistment> 类从 <xref:System.Transactions.Enlistment.Done%2A> 类继承 <xref:System.Transactions.Enlistment> 方法。 如果在准备阶段在 <xref:System.Transactions.PreparingEnlistment> 回调时调用此方法，则此方法会向 TM 通知这是只读登记（也就是说，资源管理器可读取但不能更新受事务保护的数据），并且 RM 不会再从事务管理器接收到有关第 2 阶段中事务结果的任何通知。  
  
 在所有资源管理器都对 <xref:System.Transactions.PreparingEnlistment.Prepared%2A> 投票后，会向应用程序通知事务已成功提交。  
  
### <a name="commit-phase-phase-2"></a>提交阶段（第 2 阶段）  

 在事务的第二阶段，如果事务管理器从所有资源管理器均获悉准备成功（所有资源管理器都已在第 1 阶段结束时调用了 <xref:System.Transactions.PreparingEnlistment.Prepared%2A>），则会为每个资源管理器调用 <xref:System.Transactions.IEnlistmentNotification.Commit%2A> 方法。 这样，资源管理器就可使更改永久固定下来并完成提交。  
  
 如果有任何资源管理器报告在第 1 阶段的准备失败，则事务管理器应为每个资源管理器调用 <xref:System.Transactions.IEnlistmentNotification.Rollback%2A> 方法，并向应用程序指出提交失败。  
  
 因此，资源管理器应实现下列方法。  
  
```csharp
public void Commit (Enlistment enlistment)  
{  
     // Do any work necessary when commit notification is received  
  
     // Declare done on the enlistment  
     enlistment.Done();  
}  
  
public void Rollback (Enlistment enlistment)  
{  
     // Do any work necessary when rollback notification is received  
  
     // Declare done on the enlistment
     enlistment.Done();
}  
```  
  
 RM 应根据通知类型执行完成事务所需的所有工作，并在 <xref:System.Transactions.Enlistment.Done%2A> 参数上调用 <xref:System.Transactions.Enlistment> 方法来向 TM 通知它已完成。 可在辅助线程上完成此工作。 请注意，第 2 阶段的通知可能在第 1 阶段调用了 <xref:System.Transactions.PreparingEnlistment.Prepared%2A> 方法的同一线程上发生内联。 因此，在调用 <xref:System.Transactions.PreparingEnlistment.Prepared%2A> 后，您不应执行任何预计在收到第 2 阶段通知前就可完成的操作（如释放锁定）。  
  
### <a name="implementing-indoubt"></a>实现 InDoubt  

 最后，应为可变资源管理器实现 <xref:System.Transactions.IEnlistmentNotification.InDoubt%2A> 方法。 如果事务管理器与一个或多个参与者失去联系，以致这些参与者的状态是未知的，则会调用此方法。 如果发生这种情况，则应将相关事宜记录下来，以便可在以后调查是否有任何事务参与者事后处于不一致状态。  
  
```csharp
public void InDoubt (Enlistment enlistment)  
{  
     // log this  
     enlistment.Done();  
}  
```  
  
## <a name="single-phase-commit-optimization"></a>单阶段提交优化  

 单阶段提交协议在运行时更有效，因为使用它，无需进行任何显式协调就可执行所有更新。 有关此协议的详细信息，请参阅 [使用单阶段提交和可提升的单阶段通知进行优化](optimization-spc-and-promotable-spn.md)。  
  
## <a name="see-also"></a>请参阅

- [使用单阶段提交和可提升的单阶段通知进行优化](optimization-spc-and-promotable-spn.md)
- [在事务中将资源登记为参与者](enlisting-resources-as-participants-in-a-transaction.md)
