---
title: 如何：使用 SpinWait 实现两阶段等待操作
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- SpinWait, how to synchronize two-phase wait
ms.assetid: b2ac4e4a-051a-4f65-b4b9-f8e103aff195
ms.openlocfilehash: e5d58067f70706294308952b9f7cfbf69fa89a58
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95728480"
---
# <a name="how-to-use-spinwait-to-implement-a-two-phase-wait-operation"></a>如何：使用 SpinWait 实现两阶段等待操作

下面的示例展示了如何使用 <xref:System.Threading.SpinWait?displayProperty=nameWithType> 对象实现两阶段等待操作。 在第一阶段中，同步对象 `Latch` 旋转几个周期，同时检查锁是否可用。 在第二阶段中，如果锁可用，`Wait` 方法返回结果，而不使用 <xref:System.Threading.ManualResetEvent?displayProperty=nameWithType> 执行等待操作；否则，`Wait` 执行等待操作。  
  
## <a name="example"></a>示例  

 此示例展示了非常基本的 Latch 同步基元实现。 如果应缩短等待时间，可以使用此数据结构。 此示例只为了方便本文演示。 如果需要在程序中实现 Latch 类型功能，请考虑使用 <xref:System.Threading.ManualResetEventSlim?displayProperty=nameWithType>。  
  
 [!code-csharp[CDS_SpinWait#03](../../../samples/snippets/csharp/VS_Snippets_Misc/cds_spinwait/cs/spinwait03.cs#03)]
 [!code-vb[CDS_SpinWait#03](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_spinwait/vb/spinwait2.vb#03)]  
  
 Latch 使用 <xref:System.Threading.SpinWait> 对象进行原位旋转，仅持续到下一次调用 `SpinOnce` 导致 <xref:System.Threading.SpinWait> 生成线程的时间片。 此时，Latch 对 <xref:System.Threading.ManualResetEvent> 调用 <xref:System.Threading.WaitHandle.WaitOne%2A>，并传入剩余的超时值，促使自己的上下文切换。  
  
 日志输出展示了 Latch 能够通过获取锁（而不是使用 <xref:System.Threading.ManualResetEvent>）提升性能的频率。  
  
## <a name="see-also"></a>另请参阅

- [SpinWait](spinwait.md)
- [线程处理对象和功能](threading-objects-and-features.md)
