---
title: 数据并行和任务并行中的潜在缺陷
description: 了解数据并行和任务并行中的潜在缺陷，因为并行增加了在顺序代码中没有出现的复杂性。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parallel programming, pitfalls
ms.assetid: 1e357177-e699-4b8f-9e49-56d3513ed128
ms.openlocfilehash: 3b01da23c45ca460d10ac63a7c86cd050bee3630
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730534"
---
# <a name="potential-pitfalls-in-data-and-task-parallelism"></a>数据并行和任务并行中的潜在缺陷

在许多情况下，与普通的顺序循环相比，<xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> 可以显著提升性能。 但是，对循环进行并行化的工作增加了复杂性，可能会导致在顺序代码中出现不常见或根本不会遇到的问题。 本主题列出了一些在编写并行循环时要避免的做法。  
  
## <a name="do-not-assume-that-parallel-is-always-faster"></a>不要假定并行的速度始终更快  

 在某些情况下，并行循环可能比它等效的顺序循环的运行速度更慢。 基本的经验法则是具有较少迭代和快速用户委托的并行循环未必会快很多。 但是，由于性能会涉及到很多因素，因此我们建议始终衡量实际的结果。  
  
## <a name="avoid-writing-to-shared-memory-locations"></a>避免写入共享内存位置  

 在顺序代码中，从静态变量或类字段中读取或写入静态变量或类字段的情况很常见。 但是，每当多个线程同时访问此类变量时，则很有可能会出现争用条件。 即使可以使用锁来同步对变量的访问，但同步开销可能会对性能造成损害。 因此，我们建议尽可能地避免在一个并行循环中访问共享状态，或至少限制对共享状态的访问。 为此，最好使用 <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> 的重载，以便在循环执行期间使用 <xref:System.Threading.ThreadLocal%601?displayProperty=nameWithType> 变量存储线程本地状态。 有关详细信息，请参阅[如何：编写具有线程局部变量的 Parallel.For 循环](how-to-write-a-parallel-for-loop-with-thread-local-variables.md)和[如何：使用分区本地变量编写 Parallel.ForEach 循环](how-to-write-a-parallel-foreach-loop-with-partition-local-variables.md)。  
  
## <a name="avoid-over-parallelization"></a>避免过度并行化  

 通过使用并行循环，将会产生对源集合进行分区和同步工作线程的开销成本。 计算机上的处理器数量进一步限制了并行化的优点。 仅在一个处理器上运行多个受计算限制的线程时，速度并不会得到提升。 因此，必须要小心，不要对循环进行过度并行化。  
  
 在嵌套的循环中，最有可能发生过度并行化的情况。 在大多数情况下，除非满足以下一个或多个条件，否则最好仅对外部循环进行并行化：  
  
- 已知内部循环非常长。  
  
- 正在对每个订单执行开销极大的计算。 （示例中所示的操作开销不大。）  
  
- 已知目标系统具有足够的处理器来处理通过对 `cust.Orders` 上的查询进行并行化所产生的线程数。  
  
 在所有情况下，确定最佳查询形式的最好方法是进行测试和测量。  
  
## <a name="avoid-calls-to-non-thread-safe-methods"></a>避免调用非线程安全方法  

 如果从并行循环中写入非线程安全实例方法，可能会导致出现程序可能检测到也可能检测不到的数据损坏。 还可能会导致异常。 在下面的示例中，多个线程尝试同时调用 <xref:System.IO.FileStream.WriteByte%2A?displayProperty=nameWithType> 方法，类并不支持这样做。  
  
 [!code-csharp[TPL_Pitfalls#04](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_pitfalls/cs/pitfalls.cs#04)]
 [!code-vb[TPL_Pitfalls#04](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_pitfalls/vb/pitfalls_vb.vb#04)]  
  
## <a name="limit-calls-to-thread-safe-methods"></a>限制调用线程安全方法  

 .NET 中的大多数静态方法是线程安全的，并且可以同时从多个线程中调用。 但是，即使在这些情况下，所涉及到的同步也可能会导致查询速度大幅度下降。  
  
> [!NOTE]
> 可以自行对此进行测试，具体方法是在查询中插入一些 <xref:System.Console.WriteLine%2A> 调用。 尽管出于演示目的，在文档示例中使用了此方法，但除非必要，否则不要在并行循环中使用它。  
  
## <a name="be-aware-of-thread-affinity-issues"></a>注意线程关联问题  

 某些技术（例如，单线程单元 (STA) 组件的 COM 互操作性、Windows 窗体以及 Windows Presentation Foundation (WPF)）具有要求代码在特定线程上运行的线程关联限制。 例如，在 Windows 窗体和 WPF 中，只能在创建控件的线程上访问该控件。 举例来说，这意味着，除非将线程调度器配置为仅将工作安排在 UI 线程上，否则你将无法从并行循环中更新列表控件。 有关详细信息，请参阅[指定同步上下文](xref:System.Threading.Tasks.TaskScheduler#specifying-a-synchronization-context)。  
  
## <a name="use-caution-when-waiting-in-delegates-that-are-called-by-parallelinvoke"></a>在由 Parallel.Invoke 调用的委托中等待时请谨慎使用  

 在某些情况下，任务并行库将对任务进行内联操作，这意味着它将在当前正在执行的线程上的任务上运行。 （有关详细信息，请参阅[任务计划程序](xref:System.Threading.Tasks.TaskScheduler)。）此性能优化在某些情况下可能会导致死锁。 例如，两个任务可能运行相同的委托代码，该代码在事件发生时将发出信号，然后等待另一个任务发出信号。 如果在相同线程上将第二个任务内联为第一个，并且第一个任务进入等待状态，则第二个任务将永远无法发出其事件信号。 为了避免发生这种情况，可以在等待操作上指定超时，或使用显式线程构造函数来帮助确保一个任务无法阻止另一个任务。  
  
## <a name="do-not-assume-that-iterations-of-foreach-for-and-forall-always-execute-in-parallel"></a>不要假定 ForEach、For 和 ForAll 的迭代始终并行执行  

 请务必注意，<xref:System.Threading.Tasks.Parallel.For%2A>、<xref:System.Threading.Tasks.Parallel.ForEach%2A> 或 <xref:System.Linq.ParallelEnumerable.ForAll%2A> 循环中的各个迭代可能会（但不需要）并行执行。 因此，应避免编写任何依赖于迭代并行执行的正确性或依赖于按任何特定顺序执行迭代的代码。 例如，此代码有可能会死锁：  
  
 [!code-csharp[TPL_Pitfalls#01](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_pitfalls/cs/pitfalls.cs#01)]
 [!code-vb[TPL_Pitfalls#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_pitfalls/vb/pitfalls_vb.vb#01)]  
  
 在此示例中，一个迭代设置一个事件，而所有的其他迭代则等待该事件。 在设置事件的迭代完成之前，任何等待迭代均无法完成。 但是，在设置事件的迭代有机会执行之前，等待迭代可能会阻止用于执行并行循环的所有线程。 这将导致死锁 – 设置事件的迭代将永不会执行，并且等待迭代将永远不会醒来。  
  
 具体而言，并行循环的一个迭代绝不应该等待循环的另一个迭代来继续执行。 如果并行循环决定按相反的顺序安排迭代，则会发生死锁。  
  
## <a name="avoid-executing-parallel-loops-on-the-ui-thread"></a>避免在 UI 线程上执行并行循环  

 务必要使应用程序的用户界面 (UI) 保持响应状态。 如果操作包含足够的工作来保证并行化，则可能不应在 UI 线程上运行该操作。  相反，而是应卸载要在后台线程上运行的该操作。 例如，如果要使用并行循环来计算随后应在 UI 控件中呈现的某些数据，则应考虑在任务实例内执行循环，而不是直接在 UI 事件处理程序中执行。  只有先当核心计算完成后，才应将 UI 更新封送回 UI 线程。  
  
 如果确实要在 UI 线程上运行并行循环，请当心，避免从循环内更新 UI 控件。 如果尝试从在 UI 线程上执行的并行循环内更新 UI 控件，将可能会导致状态损坏、异常、更新延迟甚至死锁，具体将取决于 UI 更新的调用方式。 在下面的示例中，在所有的迭代完成之前，并行循环将阻止它在其上执行的 UI 线程。 不过，如果循环的迭代是对后台线程运行（就像 <xref:System.Threading.Tasks.Parallel.For%2A> 一样），调用 Invoke 可能会导致将消息提交到 UI 线程，并阻止等待处理相应消息。 由于 UI 线程受阻止而无法运行 <xref:System.Threading.Tasks.Parallel.For%2A>，因此消息永远无法得到处理，且 UI 线程死锁。  
  
 [!code-csharp[TPL_Pitfalls#02](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_pitfalls/cs/pitfalls.cs#02)]
 [!code-vb[TPL_Pitfalls#02](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_pitfalls/vb/pitfalls_vb.vb#02)]  
  
 下面的示例演示如何通过在任务实例内运行循环来避免死锁。 循环不会阻止 UI 线程，并且消息可得到处理。  
  
 [!code-csharp[TPL_Pitfalls#03](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_pitfalls/cs/pitfalls.cs#03)]
 [!code-vb[TPL_Pitfalls#03](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_pitfalls/vb/pitfalls_vb.vb#03)]  
  
## <a name="see-also"></a>请参阅

- [并行编程](index.md)
- [PLINQ 的潜在问题](potential-pitfalls-with-plinq.md)
- [并行编程模式：了解并使用 .NET Framework 4 应用并行模式](https://www.microsoft.com/download/details.aspx?id=19222)
