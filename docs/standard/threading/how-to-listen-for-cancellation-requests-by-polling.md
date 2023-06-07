---
title: 如何：通过轮询侦听取消请求
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- cancellation, how to poll for requests
ms.assetid: c7f2f022-d08e-4e00-b4eb-ae84844cb1bc
ms.openlocfilehash: a527fb7f0f9e3c78b3161fdfed0f1d9f3d52798b
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723722"
---
# <a name="how-to-listen-for-cancellation-requests-by-polling"></a>如何：通过轮询侦听取消请求

下面的示例展示了一种方便用户代码定期轮询取消令牌，以确定是否已通过调用线程发出取消请求的方式。 此示例使用 <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 类型，但相同的模式适用于 <xref:System.Threading.ThreadPool?displayProperty=nameWithType> 类型或 <xref:System.Threading.Thread?displayProperty=nameWithType> 类型直接创建的异步操作。  
  
## <a name="example"></a>示例  

 若要轮询，必须有某种循环或递归代码，可用于定期读取布尔 <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> 属性的值。 如果使用的是 <xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 类型，且正在等待任务在调用线程上完成，可以使用 <xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A> 方法来检查属性并抛出异常。 通过使用此方法，可确保抛出正确的异常来响应请求。 如果使用的是 <xref:System.Threading.Tasks.Task>，那么调用此方法优于手动抛出 <xref:System.OperationCanceledException>。 如果无需抛出异常，可以直接检查属性，并通过方法返回结果（如果属性是 `true` 的话）。  
  
 [!code-csharp[Cancellation#11](../../../samples/snippets/csharp/VS_Snippets_Misc/cancellation/cs/cancellationex11.cs#11)]
 [!code-vb[Cancellation#11](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cancellation/vb/cancellationex11.vb#11)]  
  
 调用 <xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A> 非常快，不会在循环中产生很大的开销。  
  
 如果调用的是 <xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A>，那么如果除了抛出异常之外，还要执行其他工作来响应取消，只需显式检查 <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> 属性即可。 在此示例中，可以看到代码实际访问属性两次：一次是显式访问，另一次是在 <xref:System.Threading.CancellationToken.ThrowIfCancellationRequested%2A> 方法中。 不过，由于读取 <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> 属性在每次访问时只涉及一个易失读取指令，因此从性能角度来看，双重访问的意义并不大。 最好仍调用此方法，而不是手动抛出 <xref:System.OperationCanceledException>。  
  
## <a name="see-also"></a>另请参阅

- [托管线程中的取消](cancellation-in-managed-threads.md)
