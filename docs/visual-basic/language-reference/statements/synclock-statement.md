---
title: SyncLock 语句
ms.date: 07/20/2015
f1_keywords:
- vb.SyncLock
- SyncLock
helpviewer_keywords:
- threading [Visual Basic], locks
- SyncLock statement [Visual Basic]
- locks, threads
ms.assetid: 14501703-298f-4d43-b139-c4b6366af176
ms.openlocfilehash: cc8706b95e0785459e36abe27ce915b5bab8711a
ms.sourcegitcommit: d2db216e46323f73b32ae312c9e4135258e5d68e
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/22/2020
ms.locfileid: "90875198"
---
# <a name="synclock-statement"></a>SyncLock 语句

在执行块之前获取语句块的排他锁。  
  
## <a name="syntax"></a>语法  
  
```vb  
SyncLock lockobject  
    [ block ]  
End SyncLock  
```  
  
## <a name="parts"></a>组成部分  

 `lockobject`  
 必需。 计算结果为对象引用的表达式。  
  
 `block`  
 可选。 获取锁时要执行的语句块。  
  
 `End SyncLock`  
 终止 `SyncLock` 块。  
  
## <a name="remarks"></a>备注  

 `SyncLock`语句可确保多个线程不会同时执行语句块。 `SyncLock` 阻止每个线程进入块，直到没有其他线程执行它。  
  
 最常见的用途 `SyncLock` 是防止多个线程同时更新数据。 如果操作数据的语句必须在不中断的情况下完成，请将它们放在 `SyncLock` 块内。  
  
 受排他锁保护的语句块有时称为 *临界区*。  
  
## <a name="rules"></a>规则  
  
- 产生. 不能 `SyncLock` 从块外分支到块。  
  
- 锁定对象值。 的值 `lockobject` 不能为 `Nothing` 。 在语句中使用该锁对象之前，必须先创建它 `SyncLock` 。  
  
     执行块时无法更改的值 `lockobject` `SyncLock` 。 机制要求锁定对象保持不变。  
  
- 不能在块中使用 [Await](../operators/await-operator.md) 运算符 `SyncLock` 。  
  
## <a name="behavior"></a>行为  
  
- 机制. 当线程到达语句时 `SyncLock` ，它将计算 `lockobject` 表达式并挂起执行，直到它获取表达式返回的对象上的排他锁。 当另一个线程到达 `SyncLock` 语句时，它不会获取锁定，直到第一个线程执行该 `End SyncLock` 语句。  
  
- 受保护的数据。 如果 `lockobject` 是一个 `Shared` 变量，则排他锁会阻止在 `SyncLock` 任何其他线程执行块时，类的任何实例中的线程执行此块。 这会保护在所有实例之间共享的数据。  
  
     如果 `lockobject` 是不)  (实例变量 `Shared` ，则锁定会阻止在当前实例中运行的线程与 `SyncLock` 同一实例中的另一个线程同时执行该程序块。 这将保护由单个实例维护的数据。  
  
- 获取和发布。 `SyncLock`块的行为类似于 `Try...Finally` 构造 `Try` ，其中块在上获取排他锁 `lockobject` ，而 `Finally` 块释放它。 因此，无论 `SyncLock` 退出块的方式如何，块都将保证锁的版本。 即使在发生未经处理的异常时也是如此。  
  
- 框架调用。 `SyncLock`块通过调用 `Enter` `Exit` `Monitor` 命名空间中类的和方法来获取和释放排他锁 <xref:System.Threading> 。  
  
## <a name="programming-practices"></a>编程做法  

 `lockobject`表达式应始终计算为仅属于您的类的对象。 你应声明一个 `Private` 对象变量来保护属于当前实例的数据，或声明一个 `Private Shared` 对象变量来保护所有实例通用的数据。  
  
 不应使用 `Me` 关键字为实例数据提供锁定对象。 如果类的外部代码具有对类的实例的引用，它可以将该引用用作 `SyncLock` 完全不同于你的块的锁对象，从而保护不同的数据。 通过这种方式，你的类和其他类可以相互阻止执行它们的无关 `SyncLock` 块。 类似地锁定字符串可能会产生问题，因为使用相同字符串的进程中的任何其他代码都将共享同一个锁。  
  
 您也不应使用 `Me.GetType` 方法来为共享数据提供锁定对象。 这是因为， `GetType` 对于给定的类名称始终返回相同的 `Type` 对象。 外部代码可以调用 `GetType` 你的类，并获得你正在使用的同一个锁定对象。 这会导致两个类彼此阻止它们 `SyncLock` 。  
  
## <a name="examples"></a>示例  
  
### <a name="description"></a>说明  

 下面的示例演示维护简单消息列表的类。 它将消息保存在一个数组中，并将该数组的最后一个使用元素保存在一个变量中。 此 `addAnotherMessage` 过程将递增最后一个元素并存储新的消息。 这两个操作由 `SyncLock` 和语句保护 `End SyncLock` ，因为在递增最后一个元素之后，必须先存储新消息，然后其他任何线程才可以再次增加最后一个元素。  
  
 如果 `simpleMessageList` 类在所有实例中共享消息列表，则变量 `messagesList` 和将 `messagesLast` 声明为 `Shared` 。 在这种情况下，变量 `messagesLock` 还应为 `Shared` ，因此每个实例都使用单个锁对象。  
  
### <a name="code"></a>代码  

 [!code-vb[VbVbalrThreading#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrThreading/VB/Class1.vb#1)]  
  
### <a name="description"></a>说明  

 下面的示例使用线程和 `SyncLock` 。 只要该 `SyncLock` 语句存在，语句块就会成为关键部分，并且 `balance` 永远不会成为负数。 您可以 `SyncLock` `End SyncLock` 对和语句进行注释，以查看离开关键字的效果 `SyncLock` 。  
  
### <a name="code"></a>代码  

 [!code-vb[VbVbalrThreading#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrThreading/VB/class2.vb#21)]  
  
### <a name="comments"></a>注释  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Threading.Monitor?displayProperty=nameWithType>
- <xref:System.Threading.Interlocked?displayProperty=nameWithType>
- [同步基元概述](../../../standard/threading/overview-of-synchronization-primitives.md)
