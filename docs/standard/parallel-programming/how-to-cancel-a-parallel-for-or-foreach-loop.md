---
title: 如何：取消 Parallel.For 或 ForEach Loop
description: 向 ParallelOptions 参数中的方法提供取消标记对象，取消 .NET 中的 Parallel.For 或 Parallel.ForEach 循环。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parallel foreach loop, how to cancel
- parallel for loops, how to cancel
ms.assetid: 9d19b591-ea95-4418-8ea7-b6266af9905b
ms.openlocfilehash: 842873afcba1bba3a00ccaa8c95310bd1efd3a92
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95713309"
---
# <a name="how-to-cancel-a-parallelfor-or-foreach-loop"></a>如何：取消 Parallel.For 或 ForEach Loop

<xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType> 方法支持通过使用取消令牌进行取消。 若要详细了解取消的大致信息，请参阅[取消](../threading/cancellation-in-managed-threads.md)。 在并行循环中，将 <xref:System.Threading.CancellationToken> 提供给 <xref:System.Threading.Tasks.ParallelOptions> 参数中的方法，再将并行调用封闭到 try-catch 块中。  
  
## <a name="example"></a>示例  

 下面的示例展示了如何取消调用 <xref:System.Threading.Tasks.Parallel.ForEach%2A?displayProperty=nameWithType>。 可以采用相同的方法来取消调用 <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType>。  
  
 [!code-csharp[TPL_Parallel#29](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/parallel_cancel.cs#29)]
 [!code-vb[TPL_Parallel#29](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/cancelloop.vb#29)]  
  
 如果发送取消信号的令牌与<xref:System.Threading.Tasks.ParallelOptions> 实例中指定的令牌相同，并行循环会在取消时抛出一个 <xref:System.OperationCanceledException>。 如果导致取消发生的是其他一些令牌，循环会抛出带 <xref:System.OperationCanceledException> 的 <xref:System.AggregateException> 作为 InnerException。  
  
## <a name="see-also"></a>请参阅

- [数据并行](data-parallelism-task-parallel-library.md)
- [PLINQ 和 TPL 中的 Lambda 表达式](lambda-expressions-in-plinq-and-tpl.md)
