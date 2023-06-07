---
title: SpinWait
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- synchronization primitives, SpinWait
ms.assetid: 36012f42-34e5-4f86-adf4-973f433ed6c6
ms.openlocfilehash: 2031ebc30d0789181786053d401e69da782a619f
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94826204"
---
# <a name="spinwait"></a>SpinWait

<xref:System.Threading.SpinWait?displayProperty=nameWithType> 是一种轻型同步类型，可用于低级方案，以避免执行内核事件所需的高成本上下文切换和内核转换。 在多核计算机上，如果不得长时间保留资源，更高效的做法是，先让等待线程在用户模式下旋转几十或几百个周期，再重试获取资源。 如果资源在旋转后可用，便节省了几千个周期。 如果资源仍不可用，那么也只花了几个周期，仍可以进入基于内核的等待。 这种“旋转后等待”的组合有时称为“两阶段等待操作”  。  
  
 <xref:System.Threading.SpinWait> 旨在与包装内核事件（如 <xref:System.Threading.ManualResetEvent>）的 .NET 类型结合使用。 <xref:System.Threading.SpinWait> 本身也可以仅在一个程序中用于提供基本的旋转功能。  
  
 <xref:System.Threading.SpinWait> 不仅仅只是空循环。 谨慎实现后，它可以提供适用于一般情况的正确旋转行为，并且本身能够在旋转时间够长（大致是内核转换所需的时间长度）时自行启动上下文切换。 例如，在单核计算机上，<xref:System.Threading.SpinWait> 会立即生成线程的时间片，因为旋转会阻止所有线程取得进展。 即使在多核计算机上，<xref:System.Threading.SpinWait> 也会生成时间片，以防等待线程阻止优先级较高的线程或垃圾回收器。 因此，若要在两阶段等待操作中使用 <xref:System.Threading.SpinWait>，建议在 <xref:System.Threading.SpinWait> 本身启动上下文切换前，先调用内核等待。 <xref:System.Threading.SpinWait> 提供每次调用 <xref:System.Threading.SpinWait.SpinOnce%2A> 前都可以检查的 <xref:System.Threading.SpinWait.NextSpinWillYield%2A> 属性。 如果此属性返回 `true`，启动自己的等待操作。 有关示例，请参阅[如何：使用 SpinWait 实现两阶段等待操作](how-to-use-spinwait-to-implement-a-two-phase-wait-operation.md)。  
  
 如果不想执行两阶段等待操作，只是想一直旋转到某条件为 true，可以启用 <xref:System.Threading.SpinWait> 执行上下文切换，让它成为 Windows 操作系统环境中的合法成员。 下面的基本示例展示了无锁堆栈中的 <xref:System.Threading.SpinWait>。 如果需要高性能的线程安全堆栈，请考虑使用 <xref:System.Collections.Concurrent.ConcurrentStack%601?displayProperty=nameWithType>。  
  
 [!code-csharp[CDS_SpinWait#05](../../../samples/snippets/csharp/VS_Snippets_Misc/cds_spinwait/cs/spinwait.cs#05)]
 [!code-vb[CDS_SpinWait#05](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_spinwait/vb/cds_spinwait1.vb#05)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Threading.Thread.SpinWait%2A>
- [线程处理对象和功能](threading-objects-and-features.md)
