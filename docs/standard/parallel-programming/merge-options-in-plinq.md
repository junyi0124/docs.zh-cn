---
title: PLINQ 中的合并选项
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, merge options
ms.assetid: e8f7be3b-88de-4f33-ab14-dc008e76c1ba
ms.openlocfilehash: e6212abbc0d9f64765b03c3dd2e9132e9ca96ab7
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730612"
---
# <a name="merge-options-in-plinq"></a>PLINQ 中的合并选项

如果并行执行查询，PLINQ 对源序列进行分区，以便多个线程能够并发处理不同部分，通常是在不同的线程中。 如果要在一个线程（例如，`foreach`（Visual Basic 中的 `For Each`）循环）中使用结果，必须将每个线程的结果合并回一个序列中。 PLINQ 执行的合并类型具体视查询中的运算符而定。 例如，对结果强制施加新顺序的运算符必须缓冲所有线程中的全部元素。 从使用线程（以及应用用户）的角度来看，完全缓冲查询可能会运行很长时间，才能生成第一个结果。 默认情况下，其他运算符进行部分缓冲，并分批生成结果。 默认不缓冲的一个运算符是 <xref:System.Linq.ParallelEnumerable.ForAll%2A>。 它会立即生成所有线程中的所有元素。  
  
 使用 <xref:System.Linq.ParallelEnumerable.WithMergeOptions%2A> 方法（如下面的示例所示），可以向 PLINQ 提供提示，指明要执行的合并类型。  
  
 [!code-csharp[PLINQ#26](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#26)]
 [!code-vb[PLINQ#26](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#26)]  
  
 有关完整的示例，请参阅[如何：在 PLINQ 中指定合并选项](how-to-specify-merge-options-in-plinq.md)。  
  
 如果特定查询无法支持请求执行的选项，将会直接忽略此选项。 大多数情况下，无需为 PLINQ 查询指定合并选项。 不过，在某些情况下，通过测试和度量，可以发现查询在非默认模式下执行效果最佳。 这种做法的常见用途是，强制区块合并运算符流式传输结果，以提升用户界面的响应速度。  
  
## <a name="parallelmergeoptions"></a>ParallelMergeOptions  

 <xref:System.Linq.ParallelMergeOptions> 枚举包括以下选项，用于针对受支持的查询形状，指定在一个线程上使用结果时如何生成查询的最终输出：  
  
- `Not Buffered`  
  
     使用 <xref:System.Linq.ParallelMergeOptions.NotBuffered> 选项，每个处理过的元素一生成就会由各个线程返回。 这种行为类同于“流式传输”输出。 如果查询中有 <xref:System.Linq.ParallelEnumerable.AsOrdered%2A> 运算符，`NotBuffered` 暂留源元素的顺序。 虽然 `NotBuffered` 一有结果就立即开始生成，但生成所有结果的总时间可能仍长于使用其他合并选项之一的用时。  
  
- `Auto Buffered`  
  
     通过 <xref:System.Linq.ParallelMergeOptions.AutoBuffered> 选项，查询将元素收集到缓冲区中，并定期一次性将所有缓冲内容生成到使用线程中。 这类同于分“区块”生成源数据，而不是使用 `NotBuffered` 的“流式传输”行为。 `AutoBuffered` 可能需要花费比 `NotBuffered` 更长的时间，才能在使用线程中生成第一个元素。 缓冲大小和确切的生成行为不可配置，并因与查询相关的各种因素而异。  
  
- `FullyBuffered`  
  
     使用 <xref:System.Linq.ParallelMergeOptions.FullyBuffered> 选项，可以先缓冲整个查询的输出，再生成任何元素。 使用此选项时，虽然在使用线程中生成第一个元素的耗时更长，但完整结果的生成时间可能仍短于使用其他选项的用时。  
  
## <a name="query-operators-that-support-merge-options"></a>支持合并选项的查询运算符  

 下表列出了支持所有合并选项模式的运算符（受指定的限制约束）。  
  
|运算符|限制|  
|--------------|------------------|  
|<xref:System.Linq.ParallelEnumerable.AsEnumerable%2A>|None|  
|<xref:System.Linq.ParallelEnumerable.Cast%2A>|None|  
|<xref:System.Linq.ParallelEnumerable.Concat%2A>|只包含 Array 或 List 源的无序查询。|  
|<xref:System.Linq.ParallelEnumerable.DefaultIfEmpty%2A>|None|  
|<xref:System.Linq.ParallelEnumerable.OfType%2A>|None|  
|<xref:System.Linq.ParallelEnumerable.Reverse%2A>|只包含 Array 或 List 源的无序查询。|  
|<xref:System.Linq.ParallelEnumerable.Select%2A>|None|  
|<xref:System.Linq.ParallelEnumerable.SelectMany%2A>|None|  
|<xref:System.Linq.ParallelEnumerable.Skip%2A>|None|  
|<xref:System.Linq.ParallelEnumerable.Take%2A>|None|  
|<xref:System.Linq.ParallelEnumerable.Where%2A>|None|  
  
 其他所有 PLINQ 查询运算符可能会忽略用户提供的合并选项。 一些查询运算符（例如，<xref:System.Linq.ParallelEnumerable.Reverse%2A> 和 <xref:System.Linq.ParallelEnumerable.OrderBy%2A>）在生成并重新排序所有元素之前，无法生成任何元素。 因此，如果在还包含 <xref:System.Linq.ParallelEnumerable.Reverse%2A> 等运算符的查询中使用 <xref:System.Linq.ParallelMergeOptions>，除非运算符生成了结果，否则将不会在查询中应用合并行为。  
  
 一些运算符处理合并选项的能力，取决于源序列的类型，以及之前是否在查询中使用过 <xref:System.Linq.ParallelEnumerable.AsOrdered%2A> 运算符。 <xref:System.Linq.ParallelEnumerable.ForAll%2A> 始终为 <xref:System.Linq.ParallelMergeOptions.NotBuffered>；它立即生成元素。 <xref:System.Linq.ParallelEnumerable.OrderBy%2A> 始终为 <xref:System.Linq.ParallelMergeOptions.FullyBuffered>；它必须先对整个列表进行排序，再生成元素。  
  
## <a name="see-also"></a>请参阅

- [并行 LINQ (PLINQ)](introduction-to-plinq.md)
- [如何：在 PLINQ 中指定合并选项](how-to-specify-merge-options-in-plinq.md)
