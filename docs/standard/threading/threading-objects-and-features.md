---
title: 线程处理对象和功能
ms.date: 10/01/2018
helpviewer_keywords:
- threading [.NET], features
- managed threading
ms.assetid: 239b2e8d-581b-4ca3-992b-0e8525b9321c
ms.openlocfilehash: 8a66904a6db3fa45d8a42dec4e1e42883c1c3e98
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94826165"
---
# <a name="threading-objects-and-features"></a>线程处理对象和功能

.NET 与 <xref:System.Threading.Thread?displayProperty=nameWithType> 类一起提供了许多可帮助开发多线程应用程序的类。 以下文章是对这些类的概述：

|标题|说明|  
|-----------|-----------------|  
|[托管线程池](the-managed-thread-pool.md)|描述 <xref:System.Threading.ThreadPool?displayProperty=nameWithType> 类，它提供由 .NET 托管的工作线程池。|  
|[计时器](timers.md)|介绍可在多线程环境中使用的 .NET 计时器。|
|[同步基元概述](overview-of-synchronization-primitives.md)|描述可用于同步对共享资源的访问或控制线程交互的类型。|
|[EventWaitHandle](eventwaithandle.md)|描述 <xref:System.Threading.EventWaitHandle?displayProperty=nameWithType> 类，它表示一个线程同步事件。|
|[CountdownEvent](countdownevent.md)|描述 <xref:System.Threading.CountdownEvent?displayProperty=nameWithType> 类，它表示一个在其计数为零时设置的线程同步事件。|
|[Mutex](mutexes.md)|描述 <xref:System.Threading.Mutex?displayProperty=nameWithType> 类，它可授予对共享资源的独占访问权限。|
|[Semaphore 和 SemaphoreSlim](semaphore-and-semaphoreslim.md)|描述 <xref:System.Threading.Semaphore?displayProperty=nameWithType>，它用于限制可同时访问某一共享资源或资源池的线程数。|
|[Barrier](barrier.md)|描述 <xref:System.Threading.Barrier?displayProperty=nameWithType> 类，它可实现分阶段操作中的线程协调屏障模式。|
|[SpinLock](spinlock.md)|描述 <xref:System.Threading.SpinLock?displayProperty=nameWithType> 结构，它是某些低级别锁定方案的 <xref:System.Threading.Monitor?displayProperty=nameWithType> 类的轻型替代项。|
|[SpinWait](spinwait.md)|描述 <xref:System.Threading.SpinWait?displayProperty=nameWithType> 结构，它为基于调整的等待提供支持。|

## <a name="see-also"></a>另请参阅

- <xref:System.Threading.Monitor?displayProperty=nameWithType>
- <xref:System.Threading.WaitHandle?displayProperty=nameWithType>
- <xref:System.ComponentModel.BackgroundWorker?displayProperty=nameWithType>
- <xref:System.Threading.Tasks.Parallel?displayProperty=nameWithType>
- <xref:System.Threading.Tasks.Task?displayProperty=nameWithType>
- [使用线程和线程处理](using-threads-and-threading.md)
- [异步文件 I/O](../io/asynchronous-file-i-o.md)
- [并行编程](../parallel-programming/index.md)
- [任务并行库 (TPL)](../parallel-programming/task-parallel-library-tpl.md)
