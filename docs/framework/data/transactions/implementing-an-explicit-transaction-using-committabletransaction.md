---
title: 使用 CommittableTransaction 执行显式事务
description: 使用 .NET 中的 CommittableTransaction 类实现显式事务。 此类为应用程序提供了使用事务的显式方法。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 29efe5e5-897b-46c2-a35f-e599a273acc8
ms.openlocfilehash: 7e1d78b581fcb3c4b2265f1d04cf2aba83faa28a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91182878"
---
# <a name="implementing-an-explicit-transaction-using-committabletransaction"></a>使用 CommittableTransaction 执行显式事务

<xref:System.Transactions.CommittableTransaction> 类为应用程序使用事务提供了一种显式方法，而不是隐式地使用 <xref:System.Transactions.TransactionScope> 类。 对于要跨多个函数调用或多个线程调用使用同一事务的应用程序，前一种类十分有用。 与 <xref:System.Transactions.TransactionScope> 类不同，应用程序编写器需要明确调用 <xref:System.Transactions.CommittableTransaction.Commit%2A> 和 <xref:System.Transactions.Transaction.Rollback%2A> 方法以提交或中止事务。  
  
## <a name="overview-of-the-committabletransaction-class"></a>CommittableTransaction 类概述  

 <xref:System.Transactions.CommittableTransaction> 类是从 <xref:System.Transactions.Transaction> 类派生的，因此它提供了后者所具有的所有功能。 <xref:System.Transactions.Transaction.Rollback%2A> 类上的 <xref:System.Transactions.Transaction> 方法尤其有用，该方法还可用于回滚 <xref:System.Transactions.CommittableTransaction> 对象。  
  
 <xref:System.Transactions.Transaction> 类与 <xref:System.Transactions.CommittableTransaction> 类似，但前者不提供 `Commit` 方法。 这使您可以将事务对象（或其克隆）传递给其他方法（可能位于其他线程上），同时仍可控制事务的提交时间。 被调用的代码可以在事务中登记并为其投票，但只有 <xref:System.Transactions.CommittableTransaction> 对象的创建者才能提交事务。  
  
 此外，在使用 <xref:System.Transactions.CommittableTransaction> 类时，还应注意以下几点。  
  
- 创建 <xref:System.Transactions.CommittableTransaction> 事务并不会设置环境事务。 您需要明确设置和重置环境事务，才能确保资源管理器在需要时在正确的事务上下文中操作。 设置当前环境事务的方式是在全局 <xref:System.Transactions.Transaction.Current%2A> 对象上设置静态 <xref:System.Transactions.Transaction> 属性。  
  
- <xref:System.Transactions.CommittableTransaction> 对象不能被重用。 <xref:System.Transactions.CommittableTransaction> 对象一旦提交或回滚后，就不能再在事务中使用。 也就是说，它不能设置为当前环境事务上下文。  
  
## <a name="creating-a-committabletransaction"></a>创建可提交的事务  

 下面的示例创建一个新的 <xref:System.Transactions.CommittableTransaction> 并提交它。  
  
 [!code-csharp[Tx_CommittableTx#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/tx_committabletx/cs/committabletxwithsql.cs#1)]
 [!code-vb[Tx_CommittableTx#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/tx_committabletx/vb/committabletxwithsql.vb#1)]  
  
 创建 <xref:System.Transactions.CommittableTransaction> 的实例并不会自动设置环境事务上下文。 因此，资源管理器上的任何操作都不属于该事务。 全局 <xref:System.Transactions.Transaction.Current%2A> 对象上的静态 <xref:System.Transactions.Transaction> 属性用于设置或检索环境事务，应用程序必须手动设置该属性，才能确保资源管理器可参与事务。 此外，还有一个很好的做法，就是保存旧环境事务并在完成时使用 <xref:System.Transactions.CommittableTransaction> 对象还原它。  
  
 若要提交事务，需要显式调用 <xref:System.Transactions.CommittableTransaction.Commit%2A> 方法。 若要回滚事务，应调用 <xref:System.Transactions.Transaction.Rollback%2A> 方法。 请注意，还有一点很重要，就是在提交或回滚 <xref:System.Transactions.CommittableTransaction> 之前，该事务中所涉及的所有资源仍处于锁定状态。  
  
 可跨函数调用和线程使用 <xref:System.Transactions.CommittableTransaction> 对象。 但是，在发生故障时，应由应用程序开发人员负责处理异常并明确调用 <xref:System.Transactions.Transaction.Rollback%28System.Exception%29> 方法。  
  
## <a name="asynchronous-commit"></a>异步提交  

 <xref:System.Transactions.CommittableTransaction> 类还提供了一种以异步方式提交事务的机制。 事务提交可能会耗费大量时间，因为它可能涉及多个数据库访问和可能的网络延迟。 如果要在高吞吐量应用程序中避免死锁，可以使用异步提交尽快地完成事务工作，并将提交操作作为后台任务运行。 此操作可以通过 <xref:System.Transactions.CommittableTransaction.BeginCommit%2A> 类的 <xref:System.Transactions.CommittableTransaction.EndCommit%2A> 和 <xref:System.Transactions.CommittableTransaction> 方法执行。  
  
 调用 <xref:System.Transactions.CommittableTransaction.BeginCommit%2A> 可将提交延迟调度到线程池中的某一线程。 此外，还可以调用 <xref:System.Transactions.CommittableTransaction.EndCommit%2A> 来确定是否确实已提交事务。 如果无法提交事务（无论出于什么原因），<xref:System.Transactions.CommittableTransaction.EndCommit%2A> 就会引发事务异常。 如果在调用 <xref:System.Transactions.CommittableTransaction.EndCommit%2A> 时仍未提交事务，则调用方就会处于锁定状态，直到提交或中止事务为止。  
  
 执行异步提交的最简单方法就是提供要在完成提交时调用的回调方法。 但是，必须在用于执行该调用的原始 <xref:System.Transactions.CommittableTransaction.EndCommit%2A> 对象上调用 <xref:System.Transactions.CommittableTransaction> 方法。 若要获取该对象，可以向下转换回调方法的 *IAsyncResult* 参数，因为 <xref:System.Transactions.CommittableTransaction> 类实现 <xref:System.IAsyncResult> 类。  
  
 下面的示例演示如何执行异步提交。  
  
```csharp  
public void DoTransactionalWork()  
{  
     Transaction oldAmbient = Transaction.Current;  
     CommittableTransaction committableTransaction = new CommittableTransaction();  
     Transaction.Current = committableTransaction;  
  
     try  
     {  
          /* Perform transactional work here */  
          // No errors - commit transaction asynchronously  
          committableTransaction.BeginCommit(OnCommitted,null);  
     }  
     finally  
     {  
          //Restore the ambient transaction
          Transaction.Current = oldAmbient;  
     }  
}  
void OnCommitted(IAsyncResult asyncResult)  
{  
     CommittableTransaction committableTransaction;  
     committableTransaction = asyncResult as CommittableTransaction;
     Debug.Assert(committableTransaction != null);  
     try  
     {  
          using(committableTransaction)  
          {  
               committableTransaction.EndCommit(asyncResult);  
          }  
     }  
     catch(TransactionException e)  
     {  
          //Handle the failure to commit  
     }  
}  
```  
  
## <a name="see-also"></a>请参阅

- [使用事务范围实现隐式事务](implementing-an-implicit-transaction-using-transaction-scope.md)
- [事务处理](index.md)
