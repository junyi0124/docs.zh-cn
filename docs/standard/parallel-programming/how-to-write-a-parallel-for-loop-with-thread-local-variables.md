---
title: 如何：编写具有线程局部变量的 Parallel.For 循环
description: 查看示例，了解如何在 .NET 中编写使用线程局部变量的 Parallel.For 循环，这些变量可在循环中的每个独立任务中存储和检索状态。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parallel for loops, how to use local state
ms.assetid: 68384064-7ee7-41e2-90e3-71f00bde01bb
ms.openlocfilehash: f3adcfa98f4004f283b24bcd31dc243c18c2644c
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95729364"
---
# <a name="how-to-write-a-parallelfor-loop-with-thread-local-variables"></a>如何：编写具有线程局部变量的 Parallel.For 循环

此示例演示如何使用线程本地变量来存储和检索由 <xref:System.Threading.Tasks.Parallel.For%2A> 循环创建的每个单独任务中的状态。 通过使用线程本地数据，你可以避免将大量的访问同步为共享状态的开销。 在任务的所有迭代完成之前，你将计算和存储值，而不是写入每个迭代上的共享资源。 然后，你可以将最终结果一次性写入共享资源，或将其传递到另一个方法。  
  
## <a name="example"></a>示例  

 以下示例调用 <xref:System.Threading.Tasks.Parallel.For%60%601%28System.Int32%2CSystem.Int32%2CSystem.Func%7B%60%600%7D%2CSystem.Func%7BSystem.Int32%2CSystem.Threading.Tasks.ParallelLoopState%2C%60%600%2C%60%600%7D%2CSystem.Action%7B%60%600%7D%29> 方法以计算在包含一百万个元素的数组中值的总和。 每个元素的值等于其索引。  
  
 [!code-csharp[TPL_Parallel#05](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/forandforeach_simple.cs#05)]
 [!code-vb[TPL_Parallel#05](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/forwiththreadlocal.vb#05)]  
  
 每个 <xref:System.Threading.Tasks.Parallel.For%2A> 方法的前两个参数都指定起始迭代值和结束迭代值。 在方法的此重载中，第三个参数是在其中初始化本地状态的参数。 在此上下文中，“本地状态”是指其生存期恰好从当前线程上循环的第一个迭代之前延伸至最后一个迭代之后的变量。  
  
 第三个参数的类型为 <xref:System.Func%601>，其中 `TResult` 是将存储线程本地状态的变量的类型。 其类型由在调用泛型 <xref:System.Threading.Tasks.Parallel.For%60%601%28System.Int32%2CSystem.Int32%2CSystem.Func%7B%60%600%7D%2CSystem.Func%7BSystem.Int32%2CSystem.Threading.Tasks.ParallelLoopState%2C%60%600%2C%60%600%7D%2CSystem.Action%7B%60%600%7D%29> 方法时提供的泛型类型参数定义，在此情况下为 <xref:System.Int64>。 该类型参数告诉编译器将用于存储线程本地状态的临时变量的类型。 在此示例中，表达式 `() => 0`（在 Visual Basic 中为 `Function() 0`）将线程本地变量初始化为零。 如果泛型类型参数是引用类型或用户定义的值类型，表达式将如下所示：  
  
```csharp  
() => new MyClass()  
```  
  
```vb  
Function() new MyClass()  
```  
  
 第四个参数定义循环逻辑。 它必须是一个委托或 lambda 表达式，其签名在 C# 中为 `Func<int, ParallelLoopState, long, long>`，在 Visual Basic 中为 `Func(Of Integer, ParallelLoopState, Long, Long)`。 第一个参数是针对循环的该次迭代的循环计数器的值。 第二个参数是可用于中断循环的 <xref:System.Threading.Tasks.ParallelLoopState> 对象；此对象由 <xref:System.Threading.Tasks.Parallel> 类提供给循环的每个匹配项。 第三个参数是线程本地变量。 最后一个参数是返回类型。 在此情况下，该类型为 <xref:System.Int64>，因为那是我们在 <xref:System.Threading.Tasks.Parallel.For%2A> 类型参数中指定的类型。 该变量名为 `subtotal` 并且由 lambda 表达式返回。 返回值用于在循环的每个后续迭代上初始化 `subtotal`。 你也可以将最后一个参数看作传递到每个迭代，然后在最后一个迭代完成时传递到 `localFinally` 委托的值。  
  
 第五个参数定义在特定线程上的所有迭代都完成后，将调用一次的方法。 输入参数的类型同样也对应于 <xref:System.Threading.Tasks.Parallel.For%60%601%28System.Int32%2CSystem.Int32%2CSystem.Func%7B%60%600%7D%2CSystem.Func%7BSystem.Int32%2CSystem.Threading.Tasks.ParallelLoopState%2C%60%600%2C%60%600%7D%2CSystem.Action%7B%60%600%7D%29> 方法的类型参数，以及主体 lambda 表达式返回的类型。 在此示例中，通过调用 <xref:System.Threading.Interlocked.Add%2A?displayProperty=nameWithType> 方法，采用线程安全的方式在类范围将值添加到变量。 通过使用线程本地变量，我们避免了在循环的每个迭代上写入此类变量。  
  
 若要详细了解如何使用 Lambda 表达式，请参阅 [PLINQ 和 TPL 中的 Lambda 表达式](lambda-expressions-in-plinq-and-tpl.md)。  
  
## <a name="see-also"></a>请参阅

- [数据并行](data-parallelism-task-parallel-library.md)
- [并行编程](index.md)
- [任务并行库 (TPL)](task-parallel-library-tpl.md)
- [PLINQ 和 TPL 中的 Lambda 表达式](lambda-expressions-in-plinq-and-tpl.md)
