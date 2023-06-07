---
title: PLINQ 中的顺序保留
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, order preservation
ms.assetid: 10d202bc-19e1-4b5c-bbf1-9a977322a9ca
ms.openlocfilehash: 997bb80b6e30d4769613c4a1df647e6cd475a8ed
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95730599"
---
# <a name="order-preservation-in-plinq"></a>PLINQ 中的顺序保留

在 PLINQ 中，目标是在保持正确性的同时，最大限度地提升性能。 虽然查询应尽可能快地运行，但仍应生成正确结果。 在某些情况下，为了满足正确性要求，必须暂留源序列的顺序；不过，顺序暂留的计算成本可能非常高。 因此，默认情况下，PLINQ 不暂留源序列的顺序。 在这方面，PLINQ 类似于 [!INCLUDE[vbtecdlinq](../../../includes/vbtecdlinq-md.md)]，但与确实暂留顺序的 LINQ to Objects 不同。  
  
 若要替代默认行为，可以对源序列使用 <xref:System.Linq.ParallelEnumerable.AsOrdered%2A> 运算符，启用顺序暂留。 稍后，可以使用 <xref:System.Linq.ParallelEnumerable.AsUnordered%2A> 方法，在查询中禁用顺序暂留。 使用这两种方法时，查询的处理依据为，确定是并行执行还是顺序执行查询的启发。 有关详细信息，请参阅[了解 PLINQ 中的加速](understanding-speedup-in-plinq.md)。  
  
 下面的示例展示了无序并行查询，以筛选符合条件的所有元素，而完全不尝试对结果进行排序。  
  
 [!code-csharp[PLINQ#8](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#8)]
 [!code-vb[PLINQ#8](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#8)]  
  
 此查询不一定会生成源序列中符合条件的前 1000 个城市，而会生成符合条件的 1000 个城市中的一部分。 PLINQ 查询运算符将源序列划分为多个子序列，并将它们作为并发任务进行处理。 如果未指定顺序暂留，每个分区生成的结果以任意顺序传递到查询的下一个阶段。 此外，在继续处理其余元素前，分区可能还会生成一部分结果。 因此，顺序可能每次都不同。 应用无法对此进行控制，因为它取决于操作系统如何将线程排入计划。  
  
 下面的示例对源序列使用 <xref:System.Linq.ParallelEnumerable.AsOrdered%2A> 运算符，以替代默认行为。 这样可确保 <xref:System.Linq.ParallelEnumerable.Take%2A> 方法返回源序列中符合条件的前 1000 个城市。  
  
 [!code-csharp[PLINQ#9](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#9)]
 [!code-vb[PLINQ#9](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#9)]  
  
 不过，此查询可能不会像无序版本那样快速运行，因为它必须跟踪整个分区中的原始顺序，并且在合并时确保顺序是一致的。 因此，建议仅在需要时，才将 <xref:System.Linq.ParallelEnumerable.AsOrdered%2A>用于需要它的查询部分。 如果不再需要顺序暂留，请使用 <xref:System.Linq.ParallelEnumerable.AsUnordered%2A> 禁用它。 为此，下面的示例撰写了两个查询。  
  
 [!code-csharp[PLINQ#6](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#6)]
 [!code-vb[PLINQ#6](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinq2_vb.vb#6)]  
  
 请注意，PLINQ 暂留查询其余部分的顺序强制施加运算符生成的序列顺序。 也就是说，<xref:System.Linq.ParallelEnumerable.OrderBy%2A> 和 <xref:System.Linq.ParallelEnumerable.ThenBy%2A> 等运算符被视为后跟 <xref:System.Linq.ParallelEnumerable.AsOrdered%2A> 调用。  
  
## <a name="query-operators-and-ordering"></a>查询运算符和顺序  

 下面的查询运算符将顺序暂留引入查询中的所有后续操作，或一直运行到 <xref:System.Linq.ParallelEnumerable.AsUnordered%2A> 获得调用：  
  
- <xref:System.Linq.ParallelEnumerable.OrderBy%2A>  
  
- <xref:System.Linq.ParallelEnumerable.OrderByDescending%2A>  
  
- <xref:System.Linq.ParallelEnumerable.ThenBy%2A>  
  
- <xref:System.Linq.ParallelEnumerable.ThenByDescending%2A>  
  
 在某些情况下，下面的 PLINQ 查询运算符可能需要有序的源序列，才能生成正确结果：  
  
- <xref:System.Linq.ParallelEnumerable.Reverse%2A>  
  
- <xref:System.Linq.ParallelEnumerable.SequenceEqual%2A>  
  
- <xref:System.Linq.ParallelEnumerable.TakeWhile%2A>  
  
- <xref:System.Linq.ParallelEnumerable.SkipWhile%2A>  
  
- <xref:System.Linq.ParallelEnumerable.Zip%2A>  
  
 一些 PLINQ 查询运算符的行为因源序列是有序还是无序而异。 下表列出了这些运算符。  
  
|运算符|源序列为有序时的结果|源序列为无序时的结果|  
|--------------|------------------------------------------------|--------------------------------------------------|  
|<xref:System.Linq.ParallelEnumerable.Aggregate%2A>|非关联或非交换操作的非确定性输出|非关联或非交换操作的非确定性输出|  
|<xref:System.Linq.ParallelEnumerable.All%2A>|不适用|不适用|  
|<xref:System.Linq.ParallelEnumerable.Any%2A>|不适用|不适用|  
|<xref:System.Linq.ParallelEnumerable.AsEnumerable%2A>|不适用|不适用|  
|<xref:System.Linq.ParallelEnumerable.Average%2A>|非关联或非交换操作的非确定性输出|非关联或非交换操作的非确定性输出|  
|<xref:System.Linq.ParallelEnumerable.Cast%2A>|有序结果|无序结果|  
|<xref:System.Linq.ParallelEnumerable.Concat%2A>|有序结果|无序结果|  
|<xref:System.Linq.ParallelEnumerable.Count%2A>|不适用|不适用|  
|<xref:System.Linq.ParallelEnumerable.DefaultIfEmpty%2A>|不适用|不适用|  
|<xref:System.Linq.ParallelEnumerable.Distinct%2A>|有序结果|无序结果|  
|<xref:System.Linq.ParallelEnumerable.ElementAt%2A>|返回指定元素|任意元素|  
|<xref:System.Linq.ParallelEnumerable.ElementAtOrDefault%2A>|返回指定元素|任意元素|  
|<xref:System.Linq.ParallelEnumerable.Except%2A>|无序结果|无序结果|  
|<xref:System.Linq.ParallelEnumerable.First%2A>|返回指定元素|任意元素|  
|<xref:System.Linq.ParallelEnumerable.FirstOrDefault%2A>|返回指定元素|任意元素|  
|<xref:System.Linq.ParallelEnumerable.ForAll%2A>|非确定性并行执行|非确定性并行执行|  
|<xref:System.Linq.ParallelEnumerable.GroupBy%2A>|有序结果|无序结果|  
|<xref:System.Linq.ParallelEnumerable.GroupJoin%2A>|有序结果|无序结果|  
|<xref:System.Linq.ParallelEnumerable.Intersect%2A>|有序结果|无序结果|  
|<xref:System.Linq.ParallelEnumerable.Join%2A>|有序结果|无序结果|  
|<xref:System.Linq.ParallelEnumerable.Last%2A>|返回指定元素|任意元素|  
|<xref:System.Linq.ParallelEnumerable.LastOrDefault%2A>|返回指定元素|任意元素|  
|<xref:System.Linq.ParallelEnumerable.LongCount%2A>|不适用|不适用|  
|<xref:System.Linq.ParallelEnumerable.Min%2A>|不适用|不适用|  
|<xref:System.Linq.ParallelEnumerable.OrderBy%2A>|对序列重新排序|开始新的有序部分|  
|<xref:System.Linq.ParallelEnumerable.OrderByDescending%2A>|对序列重新排序|开始新的有序部分|  
|<xref:System.Linq.ParallelEnumerable.Range%2A>|暂无（默认值与 <xref:System.Linq.ParallelEnumerable.AsParallel%2A> 相同）|不适用|  
|<xref:System.Linq.ParallelEnumerable.Repeat%2A>|暂无（默认值与 <xref:System.Linq.ParallelEnumerable.AsParallel%2A> 相同）|不适用|  
|<xref:System.Linq.ParallelEnumerable.Reverse%2A>|反转|不执行任何操作|  
|<xref:System.Linq.ParallelEnumerable.Select%2A>|有序结果|无序结果|  
|<xref:System.Linq.ParallelEnumerable.Select%2A>（已编入索引）|有序结果|无序结果。|  
|<xref:System.Linq.ParallelEnumerable.SelectMany%2A>|有序结果。|无序结果|  
|<xref:System.Linq.ParallelEnumerable.SelectMany%2A>（已编入索引）|有序结果。|无序结果。|  
|<xref:System.Linq.ParallelEnumerable.SequenceEqual%2A>|有序比较|无序比较|  
|<xref:System.Linq.ParallelEnumerable.Single%2A>|不适用|不适用|  
|<xref:System.Linq.ParallelEnumerable.SingleOrDefault%2A>|不适用|不适用|  
|<xref:System.Linq.ParallelEnumerable.Skip%2A>|跳过第一个 n 元素 |跳过所有 n 元素 |  
|<xref:System.Linq.ParallelEnumerable.SkipWhile%2A>|有序结果。|非确定性。 以当前任意顺序执行 SkipWhile|  
|<xref:System.Linq.ParallelEnumerable.Sum%2A>|非关联或非交换操作的非确定性输出|非关联或非交换操作的非确定性输出|  
|<xref:System.Linq.ParallelEnumerable.Take%2A>|获取前 `n` 个元素|获取任意 `n` 个元素|  
|<xref:System.Linq.ParallelEnumerable.TakeWhile%2A>|有序结果|非确定性。 以当前任意顺序执行 TakeWhile|  
|<xref:System.Linq.ParallelEnumerable.ThenBy%2A>|补充 `OrderBy`|补充 `OrderBy`|  
|<xref:System.Linq.ParallelEnumerable.ThenByDescending%2A>|补充 `OrderBy`|补充 `OrderBy`|  
|<xref:System.Linq.ParallelEnumerable.ToArray%2A>|有序结果|无序结果|  
|<xref:System.Linq.ParallelEnumerable.ToDictionary%2A>|不适用|不适用|  
|<xref:System.Linq.ParallelEnumerable.ToList%2A>|有序结果|无序结果|  
|<xref:System.Linq.ParallelEnumerable.ToLookup%2A>|有序结果|无序结果|  
|<xref:System.Linq.ParallelEnumerable.Union%2A>|有序结果|无序结果|  
|<xref:System.Linq.ParallelEnumerable.Where%2A>|有序结果|无序结果|  
|<xref:System.Linq.ParallelEnumerable.Where%2A>（已编入索引）|有序结果|无序结果|  
|<xref:System.Linq.ParallelEnumerable.Zip%2A>|有序结果|无序结果|  
  
 无序结果并不是主动随机无序；就是没有应用任何特殊顺序逻辑。 在某些情况下，无序查询可能会暂留源序列的顺序。 对于使用已编入索引的 Select 运算符的查询，PLINQ 保证输出元素按索引升序排列，但不保证向元素分配哪些索引。  
  
## <a name="see-also"></a>另请参阅

- [并行 LINQ (PLINQ)](introduction-to-plinq.md)
- [并行编程](index.md)
