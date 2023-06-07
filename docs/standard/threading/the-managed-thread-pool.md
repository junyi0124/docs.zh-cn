---
title: 托管线程池
description: 了解提供后台工作线程的 .NET 线程池
ms.date: 08/02/2018
helpviewer_keywords:
- thread pooling [.NET]
- thread pools [.NET]
- threading [.NET], thread pool
- threading [.NET], pooling
ms.assetid: 2be05b06-a42e-4c9d-a739-96c21d673927
ms.openlocfilehash: 9bc90acd6d7e1bef81e767ce8c257fed78322554
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94826178"
---
# <a name="the-managed-thread-pool"></a>托管线程池

<xref:System.Threading.ThreadPool?displayProperty=nameWithType> 类为你的应用程序提供一个受系统管理的辅助线程池，从而使你能够专注于应用程序任务，而非线程管理。 如果有需要后台处理的短任务，托管的线程池则为利用多个线程的简便方法。 在 Framework 4 及更高版本中使用线程池容易得多，因为可以创建在线程池线程上执行异步任务的 <xref:System.Threading.Tasks.Task> 和 <xref:System.Threading.Tasks.Task%601> 对象。  
  
.NET 将线程池线程用于多种用途，包括[任务并行库 (TPL)](../parallel-programming/task-parallel-library-tpl.md) 操作、异步 I/O 完成、[计时器](timers.md)回调、注册的等待操作、使用委托的异步方法调用和 <xref:System.Net?displayProperty=nameWithType> 套接字连接。  

## <a name="thread-pool-characteristics"></a>线程池特征

线程池线程是[后台](foreground-and-background-threads.md)线程。 每个线程均使用默认的堆栈大小，以默认的优先级运行，并且位于多线程单元中。 一旦线程池中的线程完成任务，它将返回到等待线程队列中。 这时开始即可重用它。 通过这种重复使用，应用程序可以避免产生为每个任务创建新线程的开销。
  
每个进程只有一个线程池。  
  
### <a name="exceptions-in-thread-pool-threads"></a>线程池线程中的异常

线程池线程中未经处理的异常终止该进程。 以下为此规则的三种例外情况：  
  
- <xref:System.Threading.ThreadAbortException?displayProperty=nameWithType> 在线程池线程中引发，因为调用了 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>。  
- <xref:System.AppDomainUnloadedException?displayProperty=nameWithType> 在线程池线程中引发，因为正在卸载应用程序域。  
- 公共语言运行时或主机进程将终止该线程。  
  
有关详细信息，请参阅[托管线程异常](exceptions-in-managed-threads.md)。  
  
### <a name="maximum-number-of-thread-pool-threads"></a>最大线程池线程数

可以排队到线程池中的操作数仅受可用内存限制。 但是，线程池会限制进程中可同时处于活动状态的线程数。 如果所有线程池线程都处于忙碌状态，则其他工作项将进行排队，直到要执行它们的线程空闲。 进程的线程池的默认大小取决于若干因素，例如虚拟地址空间的大小。 进程可以调用 <xref:System.Threading.ThreadPool.GetMaxThreads%2A?displayProperty=nameWithType> 方法，以确定线程数。  
  
可以使用 <xref:System.Threading.ThreadPool.GetMaxThreads%2A?displayProperty=nameWithType> 和 <xref:System.Threading.ThreadPool.SetMaxThreads%2A?displayProperty=nameWithType> 方法来控制最大线程数。  

> [!NOTE]
> 承载公共语言运行时的代码可使用 [`ICorThreadpool::CorSetMaxThreads`](../../framework/unmanaged-api/hosting/icorthreadpool-corsetmaxthreads-method.md) 方法来设置大小。  
  
### <a name="thread-pool-minimums"></a>线程池最小值

线程池根据需要提供新的工作线程或 I/O 完成线程，直到它达到每个类别的指定最小值。 可以使用 <xref:System.Threading.ThreadPool.GetMinThreads%2A?displayProperty=nameWithType> 方法来获取这些最小值。  
  
> [!NOTE]
> 需求较低时，线程池线程的实际数量可以低于最小值。  
  
达到最小值时，线程池可以创建其他线程或等待，直到一些任务完成。 线程池创建和销毁工作线程以优化吞吐量，吞吐量被定义为每个单位时间完成的任务数。 线程过少可能无法实现可用资源的最优利用，而线程过多则可能增加资源争用。  
  
> [!CAUTION]
> 可以使用 <xref:System.Threading.ThreadPool.SetMinThreads%2A?displayProperty=nameWithType> 方法来增加最小空闲线程数。 但是，不必要地增加这些值可能导致性能问题。 如果在同一时间开始太多的任务，则所有任务均可能会很慢。 大多数情况下，使用自己的分配线程算法，线程池将更好地执行任务。  

## <a name="using-the-thread-pool"></a>使用线程池

使用线程池的最简单方法是使用[任务并行库 (TPL)](../parallel-programming/task-parallel-library-tpl.md)。 默认情况下，TPL 类型（例如 <xref:System.Threading.Tasks.Task> 和 <xref:System.Threading.Tasks.Task%601>）使用线程池线程来运行任务。

也可以通过从托管代码调用 <xref:System.Threading.ThreadPool.QueueUserWorkItem%2A?displayProperty=nameWithType>（或从非托管代码调用 [`ICorThreadpool::CorQueueUserWorkItem`](../../framework/unmanaged-api/hosting/icorthreadpool-corqueueuserworkitem-method.md)）并传递表示执行任务的方法的 <xref:System.Threading.WaitCallback?displayProperty=nameWithType> 委托来使用线程池。

使用线程池的另一种方法是通过使用 <xref:System.Threading.ThreadPool.RegisterWaitForSingleObject%2A?displayProperty=nameWithType> 方法并传递在发出信号或超时的时候调用 <xref:System.Threading.WaitOrTimerCallback?displayProperty=nameWithType> 委托所表示的方法的 <xref:System.Threading.WaitHandle?displayProperty=nameWithType>，从而对与等待操作相关的工作项排队。 线程池线程用于调用回调方法。  

有关示例，请查看引用的 API 页面。
  
## <a name="skipping-security-checks"></a>跳过安全检查

线程池还提供 <xref:System.Threading.ThreadPool.UnsafeQueueUserWorkItem%2A?displayProperty=nameWithType> 和 <xref:System.Threading.ThreadPool.UnsafeRegisterWaitForSingleObject%2A?displayProperty=nameWithType> 方法。 仅当确定调用方的堆栈与执行排队的任务期间进行的任何安全检查无关时，方可使用这些方法。 <xref:System.Threading.ThreadPool.QueueUserWorkItem%2A?displayProperty=nameWithType> 和 <xref:System.Threading.ThreadPool.RegisterWaitForSingleObject%2A?displayProperty=nameWithType> 均捕获调用方堆栈，当线程开始执行任务时，调用方的堆栈被合并到线程池线程的堆栈中。 如果安全检查是必需的，则必须检查整个堆栈。 尽管此检查提供了安全性，但它也具有性能成本。  

## <a name="when-not-to-use-thread-pool-threads"></a>何时不使用线程池线程

有几种应用场景，其中适合创建并管理自己的线程，而非使用线程池线程：  
  
- 需要一个前台线程。  
- 需要具有特定优先级的线程。  
- 拥有会导致线程长时间阻塞的任务。 线程池具有最大线程数，因此大量被阻塞的线程池线程可能会阻止任务启动。  
- 需将线程放入单线程单元。 所有 <xref:System.Threading.ThreadPool> 线程均位于多线程单元中。  
- 需具有与线程关联的稳定标识，或需将一个线程专用于一项任务。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Threading.ThreadPool?displayProperty=nameWithType>
- <xref:System.Threading.Tasks.Task?displayProperty=nameWithType>
- <xref:System.Threading.Tasks.Task%601?displayProperty=nameWithType>
- [任务并行库 (TPL)](../parallel-programming/task-parallel-library-tpl.md)
- [如何：从任务中返回值](../parallel-programming/how-to-return-a-value-from-a-task.md)
- [线程处理对象和功能](threading-objects-and-features.md)
- [线程与线程处理](threads-and-threading.md)
- [异步文件 I/O](../io/asynchronous-file-i-o.md)
- [计时器](timers.md)
