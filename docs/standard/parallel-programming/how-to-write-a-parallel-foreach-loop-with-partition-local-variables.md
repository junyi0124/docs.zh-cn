---
title: 如何：使用分区本地变量编写 Parallel.ForEach 循环
description: 请参阅如何在 .NET 中编写使用分区局部变量的 Parallel.ForEach 循环的示例。
ms.date: 06/26/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parallel foreach loop, how to use local state
ms.assetid: 24b10041-b30b-45cb-aa65-66cf568ca76d
ms.openlocfilehash: f8cb7f465d4594c5b9405e8152c6c4a3f3e1ff7b
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94825424"
---
# <a name="how-to-write-a-parallelforeach-loop-with-partition-local-variables"></a>如何：使用分区本地变量编写 Parallel.ForEach 循环

下面的示例演示如何编写使用分区本地变量的 <xref:System.Threading.Tasks.Parallel.ForEach%2A> 方法。 当 <xref:System.Threading.Tasks.Parallel.ForEach%2A> 循环执行时，它会将其源集合划分为多个分区。 每个分区都有自己的分区本地变量的副本。 分区本地变量类似于[线程本地变量](xref:System.Threading.ThreadLocal%601)，只是单个线程上可以运行多个分区。

此示例中的代码和参数非常类似于对应的 <xref:System.Threading.Tasks.Parallel.For%2A> 方法。 有关详细信息，请参阅[如何：编写具有线程局部变量的 Parallel.For 循环](how-to-write-a-parallel-for-loop-with-thread-local-variables.md)。

若要在 <xref:System.Threading.Tasks.Parallel.ForEach%2A> 循环中使用分区本地变量，必须调用采用两个类型参数的其中一个方法重载。 第一个类型参数 `TSource` 指定源元素的类型，第二个类型参数 `TLocal` 指定分区本地变量的类型。

## <a name="example"></a>示例

以下示例调用 <xref:System.Threading.Tasks.Parallel.ForEach%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%601%7D%2CSystem.Func%7B%60%600%2CSystem.Threading.Tasks.ParallelLoopState%2C%60%601%2C%60%601%7D%2CSystem.Action%7B%60%601%7D%29?displayProperty=nameWithType> 重载以计算一百万个元素的数组的总和。 此重载具有四个参数：

- `source`，也就是数据源。 它必须实现 <xref:System.Collections.Generic.IEnumerable%601>。 在我们的示例中，数据源是由 `IEnumerable<Int32>` 方法返回的一百万个成员 <xref:System.Linq.Enumerable.Range%2A?displayProperty=nameWithType> 对象。

- `localInit`，或初始化分区本地变量的函数。 为每个在其中执行 <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> 操作的分区调用此函数一次。 我们的示例将分区本地变量初始化为零。

- `body`，由并行循环对循环的每个迭代调用的 <xref:System.Func%604>。 其签名为 `Func\<TSource, ParallelLoopState, TLocal, TLocal>`。 你为委托提供代码，并且循环将传入输入参数，它们是：

  - <xref:System.Collections.Generic.IEnumerable%601> 的当前元素。

  - 你可以在委托的代码中用来检查循环状态的 <xref:System.Threading.Tasks.ParallelLoopState> 变量。

  - 分区本地变量。

  你的委托返回分区本地变量，然后将此变量传递到在该特定分区中执行的循环的下一次迭代。 每个循环分区维护此变量的一个单独实例。

  在该示例中，委托将每个整数的值添加到分区本地变量，该变量维护该分区中整数元素值的不断变化着的总数。

- `localFinally`，当在每个分区中的循环操作完成时，`Action<TLocal>` 调用的 <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> 委托。 <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> 方法将此循环分区的分区本地变量的最终值传递给 `Action<TLocal>` 委托，并且你提供代码，以执行合并来自此分区的结果与其他分区的结果所需的操作。 此委托可以由多个任务并行调用。 因此，该示例使用 <xref:System.Threading.Interlocked.Add%28System.Int32%40%2CSystem.Int32%29?displayProperty=nameWithType> 方法以同步对 `total` 变量的访问。 由于委托类型为 <xref:System.Action%601>，因此不存在返回值。

[!code-csharp[TPL_Parallel#04](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/foreachthreadlocal.cs#04)]
[!code-vb[TPL_Parallel#04](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/foreachthreadlocal.vb#04)]

## <a name="see-also"></a>请参阅

- [数据并行](data-parallelism-task-parallel-library.md)
- [如何：编写具有线程局部变量的 Parallel.For 循环](how-to-write-a-parallel-for-loop-with-thread-local-variables.md)
- [PLINQ 和 TPL 中的 Lambda 表达式](lambda-expressions-in-plinq-and-tpl.md)
