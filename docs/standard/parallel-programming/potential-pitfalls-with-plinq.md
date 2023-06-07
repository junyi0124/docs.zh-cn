---
title: PLINQ 的潜在缺陷
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, pitfalls
ms.assetid: 75a38b55-4bc4-488a-87d5-89dbdbdc76a2
ms.openlocfilehash: 1e1763132cd0ee8f1135e562c36ce4adf603f5af
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94822238"
---
# <a name="potential-pitfalls-with-plinq"></a>PLINQ 的潜在缺陷

在许多情况下，与顺序 LINQ to Objects 查询相比，PLINQ 可以显著提升性能。 不过，并行执行查询增加了工作复杂性，可能会导致在顺序代码中不常见或根本不会遇到的问题。 本主题列出了一些在编写 PLINQ 查询时要避免的做法。

## <a name="dont-assume-that-parallel-is-always-faster"></a>不要假定并行速度总是更快

并行执行有时会导致 PLINQ 查询比相当的 LINQ to Objects 慢。 基本原则是，源元素很少且用户委托速度快的查询不太可能会加速很多。 不过，由于影响性能的因素有很多，因此建议在决定是否使用 PLINQ 前，先度量一下实际结果。 有关详细信息，请参阅[了解 PLINQ 中的加速](understanding-speedup-in-plinq.md)。

## <a name="avoid-writing-to-shared-memory-locations"></a>不要写入共享内存位置

在顺序代码中，从静态变量或类字段中读取或写入静态变量或类字段的情况很常见。 但是，每当多个线程同时访问此类变量时，则很有可能会出现争用条件。 即使可以使用锁来同步对变量的访问，但同步开销可能会对性能造成损害。 因此，建议尽量避免在 PLINQ 查询中访问共享状态，或至少限制对共享状态的访问。

## <a name="avoid-over-parallelization"></a>不要过度并行化

使用 `AsParallel` 方法会产生对源集合进行分区和同步工作线程的开销成本。 计算机上的处理器数量进一步限制了并行化的优点。 仅在一个处理器上运行多个受计算限制的线程时，速度并不会得到提升。 因此，必须要小心，不要过度并行执行查询。

过度并行执行的最常见方案是嵌套查询，如下面的代码片段所示。

[!code-csharp[PLINQ#20](~/samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#20)]
[!code-vb[PLINQ#20](~/samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#20)]

在这种情况下，最好仅并行执行外部数据源（“客户”），除非满足下面的一个或多个条件：

- 内部数据源 (cust.Orders) 已知非常长。

- 正在对每个订单执行开销极大的计算。 （示例中所示的操作开销不大。）

- 已知目标系统具有足够的处理器来处理通过对 `cust.Orders` 上的查询进行并行化所产生的线程数。

在所有情况下，确定最佳查询形式的最好方法是进行测试和测量。 有关详细信息，请参阅[如何：衡量 PLINQ 查询性能](how-to-measure-plinq-query-performance.md)。

## <a name="avoid-calls-to-non-thread-safe-methods"></a>避免调用非线程安全方法

如果通过 PLINQ 查询对非线程安全实例方法执行写入操作，可能会导致数据损坏，程序可能会或可能不会检测不到它。 还可能会导致异常。 在下面的示例中，多个线程尝试同时调用 `FileStream.Write` 方法，类并不支持这样做。

```vb
Dim fs As FileStream = File.OpenWrite(…)
a.AsParallel().Where(...).OrderBy(...).Select(...).ForAll(Sub(x) fs.Write(x))
```

```csharp
FileStream fs = File.OpenWrite(...);
a.AsParallel().Where(...).OrderBy(...).Select(...).ForAll(x => fs.Write(x));
```

## <a name="limit-calls-to-thread-safe-methods"></a>限制对线程安全方法的调用

.NET 中的大多数静态方法是线程安全的，并且可以同时从多个线程中调用。 但是，即使在这些情况下，所涉及到的同步也可能会导致查询速度大幅度下降。

> [!NOTE]
> 可以自行对此进行测试，具体方法是在查询中插入一些 <xref:System.Console.WriteLine%2A> 调用。 文档示例中使用的此方法只为了方便本文演示，请勿在 PLINQ 查询中使用它。

## <a name="avoid-unnecessary-ordering-operations"></a>避免不必要的排序操作

并行执行查询时，PLINQ 将源序列划分为多个分区，可以在多个线程上并发运行。 默认情况下，分区的处理顺序和结果顺序是不可预测的（`OrderBy` 等运算符除外）。 虽然可以指示 PLINQ 暂留任何源序列的顺序，但这会对性能产生不利影响。 最佳做法是，尽量将查询的结构设计为不依赖顺序暂留。 有关详细信息，请参阅 [PLINQ 中的顺序保留](order-preservation-in-plinq.md)。

## <a name="prefer-forall-to-foreach-when-it-is-possible"></a>尽量首选 ForAll（而不是 ForEach）

尽管 PLINQ 对多个线程执行查询，但如果在 `foreach` 循环（Visual Basic 中的 `For Each`）中使用结果，查询结果必须合并回一个线程，并由枚举器串行访问。 在某些情况下，这是不可避免的；不过，应尽量使用 `ForAll` 方法，让每个线程输出自己的结果。例如，通过对线程安全集合（如 <xref:System.Collections.Concurrent.ConcurrentBag%601?displayProperty=nameWithType>）执行写入操作。

此问题还适用于 <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType>。 换言之，强烈建议 `source.AsParallel().Where().ForAll(...)` 首选 `Parallel.ForEach(source.AsParallel().Where(), ...)`。

## <a name="be-aware-of-thread-affinity-issues"></a>注意线程关联问题

某些技术（例如，单线程单元 (STA) 组件的 COM 互操作性、Windows 窗体以及 Windows Presentation Foundation (WPF)）具有要求代码在特定线程上运行的线程关联限制。 例如，在 Windows 窗体和 WPF 中，只能在创建控件的线程上访问该控件。 如果尝试在 PLINQ 查询中访问 Windows 窗体控件的共享状态，且在调试器中运行查询，就会导致异常抛出。 （可以禁用此设置。）不过，如果对 UI 线程使用查询，可以通过枚举查询结果的 `foreach` 循环来访问控件，因为代码只对一个线程执行。

## <a name="dont-assume-that-iterations-of-foreach-for-and-forall-always-execute-in-parallel"></a>不要假定 ForEach、For 和 ForAll 的迭代始终并行执行

请务必注意，<xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType>、<xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> 或 <xref:System.Linq.ParallelEnumerable.ForAll%2A> 循环中的各个迭代可能会（但不需要）并行执行。 因此，应避免编写任何依赖于迭代并行执行的正确性或依赖于按任何特定顺序执行迭代的代码。

例如，此代码有可能会死锁：

```vb
Dim mre = New ManualResetEventSlim()
Enumerable.Range(0, Environment.ProcessorCount * 100).AsParallel().ForAll(Sub(j)
   If j = Environment.ProcessorCount Then
       Console.WriteLine("Set on {0} with value of {1}", Thread.CurrentThread.ManagedThreadId, j)
       mre.Set()
   Else
       Console.WriteLine("Waiting on {0} with value of {1}", Thread.CurrentThread.ManagedThreadId, j)
       mre.Wait()
   End If
End Sub) ' deadlocks
```

```csharp
ManualResetEventSlim mre = new ManualResetEventSlim();
Enumerable.Range(0, Environment.ProcessorCount * 100).AsParallel().ForAll((j) =>
{
    if (j == Environment.ProcessorCount)
    {
        Console.WriteLine("Set on {0} with value of {1}", Thread.CurrentThread.ManagedThreadId, j);
        mre.Set();
    }
    else
    {
        Console.WriteLine("Waiting on {0} with value of {1}", Thread.CurrentThread.ManagedThreadId, j);
        mre.Wait();
    }
}); //deadlocks
```

在此示例中，一个迭代设置一个事件，而所有的其他迭代则等待该事件。 在设置事件的迭代完成之前，任何等待迭代均无法完成。 但是，在设置事件的迭代有机会执行之前，等待迭代可能会阻止用于执行并行循环的所有线程。 这将导致死锁 – 设置事件的迭代将永不会执行，并且等待迭代将永远不会醒来。

具体而言，并行循环的一个迭代绝不应该等待循环的另一个迭代来继续执行。 如果并行循环决定按相反的顺序安排迭代，则会发生死锁。

## <a name="see-also"></a>请参阅

- [并行 LINQ (PLINQ)](introduction-to-plinq.md)
