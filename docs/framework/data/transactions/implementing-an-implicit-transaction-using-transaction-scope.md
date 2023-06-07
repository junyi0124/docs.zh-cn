---
title: 使用事务范围实现隐式事务
description: 在 .NET 中使用 TransactionScope 类实现隐式事务。 此类提供一种将代码块标记为参与事务的方式。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 49d1706a-1e0c-4c85-9704-75c908372eb9
ms.openlocfilehash: ff2fe64156d5d72773549d78b2e29631905cbb10
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91186856"
---
# <a name="implementing-an-implicit-transaction-using-transaction-scope"></a>使用事务范围实现隐式事务

<xref:System.Transactions.TransactionScope> 类提供了一种简单方法，使您无需与事务自身进行交互，就可以在参与事务时对代码块进行标记。 事务范围可以自动选择和管理环境事务。 由于 <xref:System.Transactions.TransactionScope> 具有简单易用性和高效性，因此建议您在开发事务应用程序时使用该类。  
  
 此外，还无需将资源显式登记到事务。 任何 <xref:System.Transactions> 资源管理器（如 SQL Server 2005）都可以检测范围所创建的环境事务是否存在，并自动对其进行登记。  
  
## <a name="creating-a-transaction-scope"></a>创建事务范围  

 下面的示例演示 <xref:System.Transactions.TransactionScope> 类的简单用法。  
  
 [!code-csharp[TransactionScope#1](../../../../samples/snippets/csharp/VS_Snippets_Remoting/TransactionScope/cs/ScopeWithSQL.cs#1)]
 [!code-vb[TransactionScope#1](../../../../samples/snippets/visualbasic/VS_Snippets_Remoting/TransactionScope/vb/ScopeWithSQL.vb#1)]  
  
 创建新的对象后，将启动事务范围 <xref:System.Transactions.TransactionScope> 。  如代码示例中所示，建议使用语句创建作用域 `using` 。 `using`语句既可在 c # 中使用，也可在 Visual Basic 中使用，它的工作方式类似于 `try` ... `finally` 块，以确保正确释放范围。  
  
 在实例化 <xref:System.Transactions.TransactionScope> 时，事务管理器确定哪些事务参与进来。 一旦确定，该范围将始终参与该事务。 此决策基于两个因素：是否存在环境事务以及构造函数中 `TransactionScopeOption` 参数的值。 环境事务是指在其中执行代码的事务。 可通过调用 <xref:System.Transactions.Transaction.Current%2A?displayProperty=nameWithType> 类的静态 <xref:System.Transactions.Transaction> 属性，获取对环境事务的引用。 有关如何使用此参数的详细信息，请参阅本主题的 [使用 TransactionScopeOption 管理事务流](#ManageTxFlow) 部分。  
  
## <a name="completing-a-transaction-scope"></a>完成事务范围  

 当应用程序完成它要在一个事务中执行的所有工作以后，您应当只调用一次 <xref:System.Transactions.TransactionScope.Complete%2A?displayProperty=nameWithType> 方法，来通知事务管理器它可以提交事务。 将调用 <xref:System.Transactions.TransactionScope.Complete%2A> 作为块中的最后一个语句，这是一个很好的做法 `using` 。  
  
 未能调用此方法会中止事务，因为事务管理器会将此解释为系统故障，或等效于事务范围内引发的异常。 但是，调用此方法并不保证会提交事务。 它只是一种将状态通知给事务管理器的方式。 调用 <xref:System.Transactions.TransactionScope.Complete%2A> 方法之后，就不能再通过 <xref:System.Transactions.Transaction.Current%2A> 属性访问环境事务，尝试这样做将会导致引发异常。  
  
 如果 <xref:System.Transactions.TransactionScope> 对象最初创建了事务，则事务管理器提交事务的实际工作发生在块中的最后一个代码行之后 `using` 。 如果该对象未创建事务，则每当 <xref:System.Transactions.CommittableTransaction.Commit%2A> 对象的所有者调用 <xref:System.Transactions.CommittableTransaction> 时都会执行提交。 此时，事务管理器将调用资源管理器，并根据是否 <xref:System.Transactions.TransactionScope.Complete%2A> 对该对象调用了方法，通知它们提交或回滚 <xref:System.Transactions.TransactionScope> 。  
  
 `using`语句可确保 <xref:System.Transactions.TransactionScope.Dispose%2A> <xref:System.Transactions.TransactionScope> 即使发生异常，也会调用该对象的方法。 <xref:System.Transactions.TransactionScope.Dispose%2A> 方法标志着事务范围的结束。 在调用此方法之后所发生的异常不会影响事务。 此方法还将环境事务还原到其前一状态。  
  
 如果范围创建事务，则会引发 <xref:System.Transactions.TransactionAbortedException>，从而中止事务。 如果事务管理器无法做出提交决定，则会引发 <xref:System.Transactions.TransactionInDoubtException>。 如果已提交事务，则不会引发异常。  
  
## <a name="rolling-back-a-transaction"></a>回滚事务  

 如果要回滚事务，则不应在事务范围中调用 <xref:System.Transactions.TransactionScope.Complete%2A> 方法。 例如，可以在该范围中引发异常。 这样，就会回滚该范围所参与的事务。  
  
## <a name="managing-transaction-flow-using-transactionscopeoption"></a><a name="ManageTxFlow"></a> 使用 TransactionScopeOption 管理事务流  

 可通过调用一个方法来嵌套事务范围，该方法在使用其自己范围的方法中使用 <xref:System.Transactions.TransactionScope>，下面示例中的 `RootMethod` 方法就是前者这样的方法。  
  
```csharp  
void RootMethod()
{
    using(TransactionScope scope = new TransactionScope())
    {
        /* Perform transactional work here */
        SomeMethod();
        scope.Complete();
    }
}

void SomeMethod()
{
    using(TransactionScope scope = new TransactionScope())
    {
        /* Perform transactional work here */
        scope.Complete();
    }
}
```  
  
 最顶层事务范围称为根范围。  
  
 <xref:System.Transactions.TransactionScope> 类提供了多个重载构造函数，它们接受 <xref:System.Transactions.TransactionScopeOption> 类型的枚举，而该枚举定义范围的事务行为。  
  
 <xref:System.Transactions.TransactionScope> 对象有以下三个选项：  
  
- 联接环境事务，或者在环境事务不存在的情况下创建新的环境事务。  
  
- 成为新的根范围，也就是说，启动一个新事务并使该事务成为其自己范围中的新环境事务。  
  
- 根本不参与事务。 因此没有环境事务。  
  
 如果用 <xref:System.Transactions.TransactionScopeOption.Required> 实例化范围并且存在环境事务，则该范围会联接该事务。 相反，如果不存在环境事务，该范围就会创建新的事务并成为根范围。 这是默认值。 在使用 <xref:System.Transactions.TransactionScopeOption.Required> 时，无论范围是根范围还是仅联接环境事务，该范围中的代码都不需要有不同的行为。 该代码在这两种情况下的行为应相同。  
  
 如果用 <xref:System.Transactions.TransactionScopeOption.RequiresNew> 实例化范围，则它始终为根范围。 它会启动一个新事务，并且其事务成为该范围中的新环境事务。  
  
 如果用 <xref:System.Transactions.TransactionScopeOption.Suppress> 实例化范围，则无论是否存在环境事务，范围都从不参与事务。 使用此值实例化的范围始终具有 `null` 作为其环境事务。  
  
 下表概括了上述这些选项。  
  
|TransactionScopeOption|参与环境事务|范围参与|  
|----------------------------|-------------------------|-----------------------------|  
|必需|否|参与新事务（将成为根范围）|  
|Requires New|否|参与新事务（将成为根范围）|  
|取消|否|不参与任何事务|  
|必需|是|参与环境事务|  
|Requires New|是|参与新事务（将成为根范围）|  
|取消|是|不参与任何事务|  
  
 在 <xref:System.Transactions.TransactionScope> 对象联接现有环境事务时，除非范围中止该事务，否则释放范围对象的操作可能并不会结束事务。 如果环境事务是由根范围创建的，则仅当释放根范围时，才会对事务调用 <xref:System.Transactions.CommittableTransaction.Commit%2A>。 如果事务是手动创建的，则它将在中止或由其创建者提交时结束。  
  
 下面的示例演示一个 <xref:System.Transactions.TransactionScope> 对象，该对象创建了三个嵌套的范围对象，并用不同的 <xref:System.Transactions.TransactionScopeOption> 值对其中每个对象进行了实例化。  
  
```csharp  
using(TransactionScope scope1 = new TransactionScope())
//Default is Required
{
    using(TransactionScope scope2 = new TransactionScope(TransactionScopeOption.Required))
    {
        //...
    }

    using(TransactionScope scope3 = new TransactionScope(TransactionScopeOption.RequiresNew))
    {
        //...  
    }
  
    using(TransactionScope scope4 = new TransactionScope(TransactionScopeOption.Suppress))
    {
        //...  
    }
}
```  
  
 下面的示例演示一个不包含任何环境事务的代码块，它使用 `scope1` 创建了一个新范围 (<xref:System.Transactions.TransactionScopeOption.Required>)。 范围 `scope1` 是根范围，因为它创建了一个新事务（事务 A），并使事务 A 成为环境事务。 `Scope1` 然后再创建三个对象，每个对象具有不同的 <xref:System.Transactions.TransactionScopeOption> 值。 例如，`scope2` 是用 <xref:System.Transactions.TransactionScopeOption.Required> 创建的；由于存在环境事务，因此该范围联接 `scope1` 所创建的第一个事务。 请注意，`scope3` 是新事务的根范围，而 `scope4` 则没有环境事务。  
  
 虽然 <xref:System.Transactions.TransactionScopeOption> 的默认值和最常用的值是 <xref:System.Transactions.TransactionScopeOption.Required>，但其他各值都有其独有的用途。  

### <a name="non-transactional-code-inside-a-transaction-scope"></a>事务范围内的非事务性代码

 <xref:System.Transactions.TransactionScopeOption.Suppress> 如果要保留代码部分执行的操作，并且不希望在操作失败的情况下中止环境事务，则会很有用。 例如，在要执行日志记录或审核操作时，或者在无论环境事务提交还是中止都要将事件发布给订户时。 使用此值，可以在事务范围中包含非事务代码段，如下面的示例所示。  
  
```csharp  
using(TransactionScope scope1 = new TransactionScope())
{
    try
    {
        //Start of non-transactional section
        using(TransactionScope scope2 = new
            TransactionScope(TransactionScopeOption.Suppress))  
        {  
            //Do non-transactional work here  
        }  
        //Restores ambient transaction here
   }
   catch {}  
   //Rest of scope1
}
```  
  
### <a name="voting-inside-a-nested-scope"></a>在嵌套范围中投票  

 虽然嵌套范围可联接根范围的环境事务，但在嵌套范围中调用 <xref:System.Transactions.TransactionScope.Complete%2A> 对根范围没有影响。 仅当从根范围到最后一个嵌套范围的所有范围都投票决定提交事务时，才会提交该事务。 由于环境事务将立即中止，因此嵌套范围中的 <xref:System.Transactions.TransactionScope.Complete%2A> 调用并不全都会影响根范围。  
  
## <a name="setting-the-transactionscope-timeout"></a>设置 TransactionScope 超时  

 <xref:System.Transactions.TransactionScope> 的有些重载构造函数接受 <xref:System.TimeSpan> 类型的值，该值用于控制事务的超时。 超时设置为零时表示超时无限长。 无限长的超时主要对调试有用，调试过程中可能要经由逐句通过代码来隔离业务逻辑中的问题，并且在尝试确定问题期间不希望所调试的事务超时。 在所有其他情况下使用无限长的超时时一定要格外小心，因为它会覆盖防止事务死锁的保护。  
  
 <xref:System.Transactions.TransactionScope> 超时值通常设置为以下两种情况下的默认值之外的值。 第一种情况是在开发期间要测试应用程序处理中止事务的方式时。 将超时值设置为较小值（如 1 毫秒）时，会导致事务失败，因而会看到错误处理代码。 将超时值设置为小于默认超时值的第二种情况是：认为在导致死锁的资源争用中涉及到范围时。 在这种情况下，需要尽快地中止事务，而不能等到达到默认超时值。  
  
 在范围联接环境事务但所指定的超时值小于为该环境事务所设置的超时值时，会在 <xref:System.Transactions.TransactionScope> 对象上强制实施较小的超时值，并且该范围必须在指定的嵌套时间内结束，否则会自动中止事务。 如果嵌套范围的超时值大于环境事务的超时值，则前者无效。  
  
## <a name="setting-the-transactionscope-isolation-level"></a>设置 TransactionScope 隔离级别  

 除超时值之外，<xref:System.Transactions.TransactionScope> 的有些重载构造函数还接受 <xref:System.Transactions.TransactionOptions> 类型的结构，用于指定隔离级别。 默认情况下，事务在隔离级别设置为 <xref:System.Transactions.IsolationLevel.Serializable> 的情况下执行。 通常对频繁执行读取的系统选择 <xref:System.Transactions.IsolationLevel.Serializable> 之外的隔离级别。 这需要全面地了解事务处理理论、事务本身的语义、所涉及的并发问题以及系统一致性的结果。  
  
 此外，并不是所有的资源管理器都支持所有的隔离级别，并且它们可选择参与高于所配置级别的事务。  
  
 除 <xref:System.Transactions.IsolationLevel.Serializable> 之外的所有级别都容易因其他事务访问相同的信息而产生不一致问题。 不同隔离级别之间的差异在于它们使用读取和写入锁定的方式。 可在事务访问资源管理器中的数据时保持锁定，也可在提交或中止事务之前保持锁定。 从吞吐量的角度来说，前者比较适合；而从一致性角度来说，后者比较适合。 这两种锁定和两种操作（读取/写入）提供了四种基本隔离级别。 有关详细信息，请参阅 <xref:System.Transactions.IsolationLevel> 。  
  
 在使用嵌套的 <xref:System.Transactions.TransactionScope> 对象时，如果要联接环境事务，则必须将所有嵌套范围配置为使用完全相同的隔离级别。 如果嵌套的 <xref:System.Transactions.TransactionScope> 对象尝试联接环境事务但却指定了不同的隔离级别，则会引发 <xref:System.ArgumentException>。  
  
## <a name="interop-with-com"></a>与 COM+ 交互  

 在创建新的 <xref:System.Transactions.TransactionScope> 实例时，可以在某一构造函数中使用 <xref:System.Transactions.EnterpriseServicesInteropOption> 枚举来指定与 COM+ 交互的方式。 有关此操作的详细信息，请参阅 [与企业服务和 COM + 事务的互操作性](interoperability-with-enterprise-services-and-com-transactions.md)。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Transactions.Transaction.Clone%2A>
- <xref:System.Transactions.TransactionScope>
