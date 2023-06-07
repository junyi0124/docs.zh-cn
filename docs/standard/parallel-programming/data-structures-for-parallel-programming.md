---
title: 用于并行编程的数据结构
ms.date: 03/30/2017
helpviewer_keywords:
- data structures, multi-threading
ms.assetid: bdc82f2f-4754-45a1-a81e-fe2e9c30cef9
ms.openlocfilehash: 4e0214afe4dba7f838f420907374f1472d6d3911
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95699009"
---
# <a name="data-structures-for-parallel-programming"></a>用于并行编程的数据结构

.NET 提供了几种对并行编程非常有用的类型，包括一组并发集合类、轻型同步基元和用于迟缓初始化的类型。 可以将这些类型与任何多线程应用代码（包括任务并行库和 PLINQ）结合使用。  
  
## <a name="concurrent-collection-classes"></a>并发回收类  

 <xref:System.Collections.Concurrent?displayProperty=nameWithType> 命名空间中的回收类提供线程安全的添加和删除操作，以尽可能地避免锁定，并在需要锁定时使用细粒度锁定。 并发集合类在访问项时不需要用户代码执行任何锁定。 如果多个线程在回收中添加和删除项，并发回收类可以显著提高 <xref:System.Collections.ArrayList?displayProperty=nameWithType> 和 <xref:System.Collections.Generic.List%601?displayProperty=nameWithType>（具有用户实现的锁定）等类型的性能。  
  
 下表列出了并发集合类：  
  
|类型|说明|  
|----------|-----------------|  
|<xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=nameWithType>|为实现 <xref:System.Collections.Concurrent.IProducerConsumerCollection%601?displayProperty=nameWithType> 的线程安全集合提供阻塞和限制功能。 如果没有槽可用或回收已满，阻止制作者线程。 如果回收为空，阻止使用者线程。 此类型还支持使用者和制作者执行非阻止访问。 可以将 <xref:System.Collections.Concurrent.BlockingCollection%601> 用作基类或后备存储，以便为支持 <xref:System.Collections.Generic.IEnumerable%601> 的任何回收类提供阻止和绑定。|  
|<xref:System.Collections.Concurrent.ConcurrentBag%601?displayProperty=nameWithType>|提供可缩放的添加和获取操作的线程安全包实现。|  
|<xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=nameWithType>|可缩放的并发字典类型。|  
|<xref:System.Collections.Concurrent.ConcurrentQueue%601?displayProperty=nameWithType>|可缩放的并发 FIFO 队列。|  
|<xref:System.Collections.Concurrent.ConcurrentStack%601?displayProperty=nameWithType>|可缩放的并发 LIFO 堆栈。|  
  
 有关详细信息，请参阅[线程安全集合](../collections/thread-safe/index.md)。  
  
## <a name="synchronization-primitives"></a>同步基元  

 通过消除旧多线程处理代码中高昂的锁定机制，<xref:System.Threading?displayProperty=nameWithType> 命名空间中的同步基元实现了细粒度并发和更快速的性能。
  
 下表列出了同步类型：  
  
|类型|说明|  
|----------|-----------------|  
|<xref:System.Threading.Barrier?displayProperty=nameWithType>|通过让每个任务可以在某一点指示自己已到达，并一直阻止到部分或全部任务已到达，让多个线程可以并行处理算法。 有关详细信息，请参阅 [Barrier](../threading/barrier.md)。|  
|<xref:System.Threading.CountdownEvent?displayProperty=nameWithType>|通过提供简单的回收机制，简化分支和联接方案。 有关详细信息，请参阅 [CountdownEvent](../threading/countdownevent.md)。|  
|<xref:System.Threading.ManualResetEventSlim?displayProperty=nameWithType>|类似于 <xref:System.Threading.ManualResetEvent?displayProperty=nameWithType> 的同步基元。 虽然 <xref:System.Threading.ManualResetEventSlim> 是轻型基元，但只能用于进程内通信。|  
|<xref:System.Threading.SemaphoreSlim?displayProperty=nameWithType>|限制可同时访问资源或资源池的线程数的同步基元。 有关详细信息，请参阅 [Semaphore 和 SemaphoreSlim](../threading/semaphore-and-semaphoreslim.md)。|  
|<xref:System.Threading.SpinLock?displayProperty=nameWithType>|互斥锁基元，导致尝试获取锁的线程先在循环中等待或旋转一段时间，再生成量程。 在应缩短锁等待时间的情况下，<xref:System.Threading.SpinLock> 的性能优于其他形式的锁定。 有关详细信息，请参阅 [SpinLock](../threading/spinlock.md)。|  
|<xref:System.Threading.SpinWait?displayProperty=nameWithType>|小的轻型类型，它会旋转一段指定的时间，并最终将线程置于等待状态（如果超出旋转计数的话）。  有关详细信息，请参阅 [SpinWait](../threading/spinwait.md)。|  
  
 有关详细信息，请参阅：  
  
- [如何：使用 SpinLock 进行低级别同步](../threading/how-to-use-spinlock-for-low-level-synchronization.md)  
  
- [如何：使用屏障同步并发操作](../threading/how-to-synchronize-concurrent-operations-with-a-barrier.md)。  
  
## <a name="lazy-initialization-classes"></a>迟缓初始化类  

 通过迟缓初始化，除非需要，否则不分配对象内存。 迟缓初始化可以提升性能，具体是通过在整个程序生存期内均匀分布对象分配。 若要为任何自定义类型启用迟缓初始化，可以包装类型 <xref:System.Lazy%601>。  
  
 下表列出了迟缓初始化类型：  
  
|类型|说明|  
|----------|-----------------|  
|<xref:System.Lazy%601?displayProperty=nameWithType>|提供线程安全的轻型迟缓初始化。|  
|<xref:System.Threading.ThreadLocal%601?displayProperty=nameWithType>|每线程提供迟缓初始化值，其中每线程迟缓调用初始化函数。|  
|<xref:System.Threading.LazyInitializer?displayProperty=nameWithType>|提供静态方法，避免出现需要分配专用迟缓初始化实例的情况。 相反，它们使用引用是为了确保目标在获得访问时已初始化。|  
  
 若要了解详细信息，请参阅[迟缓初始化](../../framework/performance/lazy-initialization.md)  
  
## <a name="aggregate-exceptions"></a>聚合异常  

 <xref:System.AggregateException?displayProperty=nameWithType> 类型可用于捕获对各个线程并发抛出的多个异常，并将它们作为一个异常返回给联接线程。 为此，<xref:System.Threading.Tasks.Task?displayProperty=nameWithType> 和 <xref:System.Threading.Tasks.Parallel?displayProperty=nameWithType> 类型以及 PLINQ 大量使用 <xref:System.AggregateException>。 有关详细信息，请参阅[异常处理](exception-handling-task-parallel-library.md)和[如何：处理 PLINQ 查询中的异常](how-to-handle-exceptions-in-a-plinq-query.md)。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- <xref:System.Threading?displayProperty=nameWithType>
- [并行编程](index.md)
