---
title: 如何：使用 SpinLock 进行低级别同步
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- SpinLock, how to use
ms.assetid: a9ed3e4e-4f29-4207-b730-ed0a51ecbc19
ms.openlocfilehash: 148ef5e9d5c570ef04bc6e716a884db5e688d91a
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94826386"
---
# <a name="how-to-use-spinlock-for-low-level-synchronization"></a>如何：使用 SpinLock 进行低级别同步

下面的示例展示了如何使用 <xref:System.Threading.SpinLock>。 在此示例中，关键部分执行的工作量最少，因而非常适合执行 <xref:System.Threading.SpinLock>。 与标准锁相比，增加一点工作量即可提升 <xref:System.Threading.SpinLock> 的性能。 但是，超过某个点时 SpinLock 将比标准锁开销更大。 可以使用分析工具中的并发分析功能，查看哪种类型的锁可以在程序中提供更好的性能。 有关详细信息，请参阅[并发可视化工具](/visualstudio/profiling/concurrency-visualizer)。  
  
 [!code-csharp[CDS_SpinLock#02](../../../samples/snippets/csharp/VS_Snippets_Misc/cds_spinlock/cs/spinlockdemo.cs#02)]
 [!code-vb[CDS_SpinLock#02](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_spinlock/vb/spinlock_vb.vb#02)]  
  
 如果共享资源上的锁不会保留太长时间，<xref:System.Threading.SpinLock> 可能会很有用。 在这种情况下，多核计算机上的阻止线程可高效旋转几个周期，直到锁被释放。 通过旋转，线程不会受到阻止，这是一个占用大量 CPU 资源的进程。 在某些情况下，<xref:System.Threading.SpinLock> 会停止旋转，以防出现逻辑处理器资源不足或超线程系统上优先级反转的情况。  
  
 此示例使用 <xref:System.Collections.Generic.Queue%601?displayProperty=nameWithType> 类，要求必须有用户同步，才能执行多线程访问。 另一种方法是使用 <xref:System.Collections.Concurrent.ConcurrentQueue%601?displayProperty=nameWithType>，这不需要任何用户锁定。  
  
 请注意，在对 <xref:System.Threading.SpinLock.Exit%2A?displayProperty=nameWithType> 的调用中使用 `false`。 这可提供最佳性能。 在 IA64 架构上指定 `true` 可使用内存界定，这会刷新写入缓冲区以确保锁现在可用于其他线程进入。
  
## <a name="see-also"></a>另请参阅

- [线程处理对象和功能](threading-objects-and-features.md)
- [lock 语句 (C#)](../../csharp/language-reference/keywords/lock-statement.md)
- [SyncLock 语句 (Visual Basic)](../../visual-basic/language-reference/statements/synclock-statement.md)
