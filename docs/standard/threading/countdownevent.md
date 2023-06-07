---
title: CountdownEvent
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- synchronization primitives, CountdownEvent
ms.assetid: eec3812a-e20f-4ecd-bfef-6921d508b708
ms.openlocfilehash: 15ff3ee8f4ea18b243f5c3070f1c59df4646a1a4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95676473"
---
# <a name="countdownevent"></a>CountdownEvent

<xref:System.Threading.CountdownEvent?displayProperty=nameWithType> 是在收到信号特定次数后取消阻止等待线程的同步基元。 <xref:System.Threading.CountdownEvent> 适用于以下情况：不得不先使用 <xref:System.Threading.ManualResetEvent> 或 <xref:System.Threading.ManualResetEventSlim> 并手动递减变量，然后再向事件发出信号。 例如，在分支/联接方案中，可以只创建信号计数为 5 的 <xref:System.Threading.CountdownEvent>，然后在线程池中启动五个工作项，并让每个工作项在完成时调用 <xref:System.Threading.CountdownEvent.Signal%2A>。 每次调用 <xref:System.Threading.CountdownEvent.Signal%2A> 都会让信号计数递减 1。 在主线程上，<xref:System.Threading.CountdownEvent.Wait%2A> 调用一直阻止到信号计数为零。  
  
> [!NOTE]
> 对于不需要与旧 .NET Framework 同步 API 交互的代码，请考虑使用 <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 对象或 <xref:System.Threading.Tasks.Parallel.Invoke%2A> 方法，以更轻松地表示分支-联接并行度。  
  
 <xref:System.Threading.CountdownEvent> 还有以下功能：  
  
- 可以使用取消令牌取消等待操作。  
  
- 创建实例后，可以递增信号计数。  
  
- 在 <xref:System.Threading.CountdownEvent.Reset%2A> 方法调用返回 <xref:System.Threading.CountdownEvent.Wait%2A> 后，可以重用实例。  
  
- 实例公开 <xref:System.Threading.WaitHandle>，以与其他 .NET 同步 API（如 <xref:System.Threading.WaitHandle.WaitAll%2A>）集成。  
  
## <a name="basic-usage"></a>基本用法  

 下面的示例展示了如何将 <xref:System.Threading.CountdownEvent> 与 <xref:System.Threading.ThreadPool> 工作项结合使用。  
  
 [!code-csharp[CDS_CountdownEvent#01](../../../samples/snippets/csharp/VS_Snippets_Misc/cds_countdownevent/cs/countdownevent.cs#01)]
 [!code-vb[CDS_CountdownEvent#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_countdownevent/vb/module1.vb#01)]  
  
## <a name="countdownevent-with-cancellation"></a>结合使用 CountdownEvent 和取消令牌  

 下面的示例展示了如何使用取消令牌对 <xref:System.Threading.CountdownEvent> 取消等待操作。 基本模式遵循 .NET Framework 4 中引入的统一取消模型。 有关详细信息，请参阅[托管线程中的取消](cancellation-in-managed-threads.md)。  
  
 [!code-csharp[CDS_CountdownEvent#02](../../../samples/snippets/csharp/VS_Snippets_Misc/cds_countdownevent/cs/countdownevent.cs#02)]
 [!code-vb[CDS_CountdownEvent#02](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_countdownevent/vb/canceleventwait.vb#02)]  
  
 请注意，等待操作不会取消正在向它发出信号的线程。 通常情况下，取消应用于逻辑操作，可以包括事件等待操作，以及此等待操作要同步的所有工作项。 在此示例中，每个工作项都传递有相同的取消令牌副本，以便能够响应取消请求。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Threading.Semaphore?displayProperty=nameWithType>
