---
title: 暂停和中断线程
description: 了解如何在 .NET 中暂停和中断线程。 了解如何使用方法（如 Thread.Sleep 和 Thread.Interrupt）和异常（如 ThreadInterruptedException）。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- interrupting threads
- threading [.NET], pausing
- pausing threads
ms.assetid: 9fce4859-a19d-4506-b082-7dd0792688ca
ms.openlocfilehash: 157109ad9e9009516b107b271a59267f982c784d
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94826217"
---
# <a name="pausing-and-interrupting-threads"></a>暂停和中断线程

同步线程活动最常见的方法是阻止和释放线程，或者锁定对象或代码区域。 有关这些锁定和阻止机制的详细信息，请参阅[同步基元概述](overview-of-synchronization-primitives.md)。  
  
 也可使线程将自身置于睡眠状态。 当线程被阻止或处于休眠状态时，可以使用 <xref:System.Threading.ThreadInterruptedException> 使它们脱离等待状态。  
  
## <a name="the-threadsleep-method"></a>Thread.Sleep 方法

 调用 <xref:System.Threading.Thread.Sleep%2A?displayProperty=nameWithType> 方法会导致当前线程立即受阻止，时间为传递到方法的毫秒数或时间间隔，结果是将时间片的剩余部分生成给其他线程。 受阻时间过后，休眠线程继续执行。  
  
 一个线程不能调用另一线程上的 <xref:System.Threading.Thread.Sleep%2A?displayProperty=nameWithType>。  <xref:System.Threading.Thread.Sleep%2A?displayProperty=nameWithType> 是一种始终让当前线程进入睡眠状态的静态方法。  
  
 调用 <xref:System.Threading.Thread.Sleep%2A?displayProperty=nameWithType>（值为 <xref:System.Threading.Timeout.Infinite?displayProperty=nameWithType>）可以让线程进入睡眠状态，直到另一个对睡眠线程调用 <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> 方法的线程中断它，或直到 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> 方法调用终止它。  下面的示例阐释了这两种中断休眠线程的方法。  
  
 [!code-csharp[Conceptual.Threading.Resuming#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.Threading.Resuming/cs/Sleep1.cs#1)]
 [!code-vb[Conceptual.Threading.Resuming#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.Threading.Resuming/vb/Sleep1.vb#1)]  
  
## <a name="interrupting-threads"></a>中断线程

 可以中断正在等待的线程，具体操作是对受阻止的线程调用 <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> 方法，从而抛出 <xref:System.Threading.ThreadInterruptedException>，以中断对线程执行的阻止调用。 线程应该捕获 <xref:System.Threading.ThreadInterruptedException> 并执行任何适于继续工作的操作。 如果线程忽略该异常，则运行时捕获异常，并停止该线程。  
  
> [!NOTE]
> 如果在调用 <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> 时，未阻止目标线程，则线程在被阻止前将不会中断。 如果线程永远不被阻止，则它可在不被中断的情况下完成。  
  
 如果等待是托管的等待，则 <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> 都会立即唤醒线程。 如果等待是非托管等待（例如，调用 Win32 [WaitForSingleObject](/windows/desktop/api/synchapi/nf-synchapi-waitforsingleobject) 函数的平台调用），<xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> 都不能控制线程，直到它返回到或调用托管代码。 在托管代码中，该行为如下：  
  
- <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> 将线程从其可能处于的任何等待中唤醒，并导致在目标线程中引发 <xref:System.Threading.ThreadInterruptedException>。  
  
- <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> 将线程从可能处于的任何等待中唤醒，并导致 <xref:System.Threading.ThreadAbortException> 在线程中抛出。 有关详细信息，请参阅[销毁线程](destroying-threads.md)。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Threading.Thread>
- <xref:System.Threading.ThreadInterruptedException>
- <xref:System.Threading.ThreadAbortException>
- [线程处理](index.md)
- [使用线程和线程处理](using-threads-and-threading.md)
- [同步基元概述](overview-of-synchronization-primitives.md)
