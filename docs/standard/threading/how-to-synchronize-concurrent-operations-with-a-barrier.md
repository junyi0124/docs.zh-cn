---
title: 如何：使用屏障来使并发操作保持同步
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Barrier, how to use
ms.assetid: e1a253ff-e0fb-4df8-95ff-d01a90d4cb19
ms.openlocfilehash: dc0955ba6194dd936da19c634ada797d6cbd3720
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95716468"
---
# <a name="how-to-synchronize-concurrent-operations-with-a-barrier"></a>如何：使用屏障来使并发操作保持同步

下面的示例展示了如何将并发任务与 <xref:System.Threading.Barrier> 同步。  
  
## <a name="example"></a>示例  

 下面的程序用于统计两个线程使用随机算法重新随机选择字词，分别在同一阶段查找一半解决方案时所需的迭代次数（或阶段数）。 在每个线程随机选择字词后，屏障后阶段操作会比较两个结果，以确定整个句子是否按正确的字词顺序呈现。  
  
 [!code-csharp[CDS_Barrier#01](../../../samples/snippets/csharp/VS_Snippets_Misc/cds_barrier/cs/barrier.cs#01)]
 [!code-vb[CDS_Barrier#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_barrier/vb/barrier_vb.vb#01)]  
  
 <xref:System.Threading.Barrier> 对象可防止并行操作中的各个任务在所有任务到达屏障前继续执行。 如果并行操作分阶段执行，且每个阶段需要在任务之间进行同步，此对象就很有用。 在此示例中，操作分两个阶段执行。 在第一阶段中，每个任务都用数据填充缓冲部分。 在每个任务填充完缓冲部分后，任务向屏障发出信号，指明可以继续执行，然后等待。 当所有任务都向屏障发出信号后，就会取消阻止它们，此时第二阶段开始。 屏障是必要的，因为第二阶段要求每个任务都有权访问此时生成的所有数据。 如果没有屏障，首先完成的任务可能会尝试从其他任务尚未填充的缓冲读取数据。 可以按照这种方式同步任意数量的阶段。  
  
## <a name="see-also"></a>另请参阅

- [用于并行编程的数据结构](../parallel-programming/data-structures-for-parallel-programming.md)
