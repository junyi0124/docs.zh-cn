---
title: Semaphore 和 SemaphoreSlim
description: 了解 Semaphore 和 SemaphoreSlim。 Semaphore 类是环绕 Win32 信号灯对象的精简包装器。 SemaphoreSlim 类是快速轻量级信号灯。
ms.date: 03/30/2017
helpviewer_keywords:
- counting semaphores
- semaphores
- threading [.NET], cross-process synchronization
- Semaphore class, about Semaphore class
- SemaphoreSlim class, about SemaphoreSlim class
- threading [.NET], Semaphore class
ms.assetid: 7722a333-b974-47a2-a7c0-f09097fb644e
ms.openlocfilehash: 34ffe11f7211d2d8b282bfd27f8c48328a5cb6d1
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95681894"
---
# <a name="semaphore-and-semaphoreslim"></a>Semaphore 和 SemaphoreSlim

<xref:System.Threading.Semaphore?displayProperty=nameWithType> 类表示一个命名（系统范围内）或本地信号量。 它是环绕 Win32 信号量对象的精简包装器。 Win32 信号量是计数信号量，该可用于控制对资源池的访问。  
  
 <xref:System.Threading.SemaphoreSlim> 类表示一个轻量、快速的信号量，可在等待时间预计很短的情况下用于在单个进程内等待。 <xref:System.Threading.SemaphoreSlim> 尽可能多地依赖公共语言运行时 (CLR) 提供的同步基元。 但是，它还提供延迟初始化、基于内核的等待句柄，作为在多个信号量上进行等待的必要支持。 <xref:System.Threading.SemaphoreSlim> 也支持使用取消标记，但不支持命名信号量或使用用于同步的等待句柄。  
  
## <a name="managing-a-limited-resource"></a>管理有限资源  

 线程通过调用从 <xref:System.Threading.WaitHandle> 类中继承的 <xref:System.Threading.WaitHandle.WaitOne%2A> 方法进入信号量，无论对于 <xref:System.Threading.Semaphore?displayProperty=nameWithType> 对象、<xref:System.Threading.SemaphoreSlim.Wait%2A?displayProperty=nameWithType> 或 <xref:System.Threading.SemaphoreSlim.WaitAsync%2A?displayProperty=nameWithType> 方法还是 <xref:System.Threading.SemaphoreSlim> 对象都适用。 当调用返回时，信号量计数会减少。 当线程请求进入且计数为零时，此线程受到阻止。 线程通过调用 <xref:System.Threading.Semaphore.Release%2A?displayProperty=nameWithType> 或 <xref:System.Threading.SemaphoreSlim.Release%2A?displayProperty=nameWithType> 方法释放信号量时，允许受阻线程进入。 受阻线程进入信号量无保证的顺序，比如先进先出 (FIFO) 或按后进先出 (LIFO)。  
  
 一个线程可通过反复调用 <xref:System.Threading.Semaphore?displayProperty=nameWithType> 对象的 <xref:System.Threading.WaitHandle.WaitOne%2A> 方法或 <xref:System.Threading.SemaphoreSlim> 对象的 <xref:System.Threading.SemaphoreSlim.Wait%2A> 方法进入信号量。 为了释放信号量，线程可以调用 <xref:System.Threading.Semaphore.Release?displayProperty=nameWithType> 或 <xref:System.Threading.SemaphoreSlim.Release?displayProperty=nameWithType> 方法重载相同次数，或调用 <xref:System.Threading.Semaphore.Release%28System.Int32%29?displayProperty=nameWithType> 或 <xref:System.Threading.SemaphoreSlim.Release%28System.Int32%29?displayProperty=nameWithType> 方法重载并指定要释放的项的数量。  
  
### <a name="semaphores-and-thread-identity"></a>信号量和线程标识  

 两种信号量类型不会在对 <xref:System.Threading.WaitHandle.WaitOne%2A>、<xref:System.Threading.SemaphoreSlim.Wait%2A>、<xref:System.Threading.Semaphore.Release%2A> 和 <xref:System.Threading.SemaphoreSlim.Release%2A?displayProperty=nameWithType> 方法的调用上强制线程标识。 例如，信号量的一种常见使用方案涉及制造者线程和使用者线程，其中一个线程始终增加信号量计数，另一个始终减少信号量计数。  
  
 程序员有责任确保线程不会过多次地地释放信号量。 例如，假定信号量的最大计数为 2 并且线程 A 和线程 B 都进入了该信号量。 如果线程 B 中的编程错误导致它两次成功调用了 `Release`。 信号灯计数已满，当线程 A 最终调用 `Release` 时，<xref:System.Threading.SemaphoreFullException> 抛出。  
  
## <a name="named-semaphores"></a>命名信号量  

 Windows 操作系统允许信号量拥有名称。 命名信号量是系统范围的。 即，一旦创建了命名信号量，它对所有进程中的所有线程均可见。 因此，命名信号量可用于同步进程以及线程的活动。  
  
 你可以通过使用指定了名称的构造函数之一来创建表示指定了已命名系统信号量的 <xref:System.Threading.Semaphore> 对象。  
  
> [!NOTE]
> 因为命名信号量是系统范围的，所以它可能有多个 <xref:System.Threading.Semaphore> 对象，这些对象表示同一命名信号量。 每次调用构造函数或 <xref:System.Threading.Semaphore.OpenExisting%2A?displayProperty=nameWithType> 方法都会创建一个新的 <xref:System.Threading.Semaphore> 对象。 反复指定相同的名称会创建多个表示同一命名信号量的对象。  
  
 使用命名信号量时务必谨慎。 因为它们是系统范围的，使用同一名称的另一进程可能会意外地进入你的信号量。 同一台计算机上的恶意代码执行可将此作为拒绝服务攻击的基础。  
  
 使用访问控制安全性来保护表示命名信号量的 <xref:System.Threading.Semaphore> 对象，最好通过使用指定 <xref:System.Security.AccessControl.SemaphoreSecurity?displayProperty=nameWithType> 对象的构造函数。 你还可以通过使用 <xref:System.Threading.Semaphore.SetAccessControl%2A?displayProperty=nameWithType> 方法应用访问控制安全，但这会使窗口在创建信号量的时间以及信号量受到保护的时间之间留下漏洞。 使用访问控制安全机制来保护信号量有助于阻止恶意攻击，但不能解决的意外的名称冲突问题。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Threading.Semaphore>
- <xref:System.Threading.SemaphoreSlim>
- [线程处理对象和功能](threading-objects-and-features.md)
