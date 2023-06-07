---
title: 同步基元概述
description: 了解用于同步对共享资源或控制线程交互的访问的 .NET 线程同步基元
ms.date: 10/01/2018
helpviewer_keywords:
- synchronization, threads
- threading [.NET],synchronizing threads
- managed threading
ms.assetid: b782bcb8-da6a-4c6a-805f-2eb46d504309
ms.openlocfilehash: 9dfaaa6050cc6e9a6b86f991aa6d2ce2a815959a
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94819651"
---
# <a name="overview-of-synchronization-primitives"></a>同步基元概述

.NET 提供了一系列可用于同步对共享资源或协调线程交互的访问的类型。

> [!IMPORTANT]
> 使用相同的同步基元实例保护对共享资源的访问。 如果使用不同的同步基元实例保护同一资源，则将避开同步基元提供的保护。

## <a name="waithandle-class-and-lightweight-synchronization-types"></a>WaitHandle 类和轻量同步类型

多个 .NET 同步基元派生自 <xref:System.Threading.WaitHandle?displayProperty=nameWithType> 类，该类会封装本机操作系统同步句柄并将信号机制用于线程交互。 这些类包括：

- <xref:System.Threading.Mutex?displayProperty=nameWithType>，授予对共享资源的独占访问权限。 如果没有任何线程拥有它，则 mutex 将处于已发出信号状态。
- <xref:System.Threading.Semaphore?displayProperty=nameWithType>，限制可同时访问某一共享资源或资源池的线程数。 当信号量计数大于零时，会将信号量的状态设置为已发出信号；当信号量计数为零时，会将信号量的状态设置为未发出信号。
- <xref:System.Threading.EventWaitHandle?displayProperty=nameWithType>，表示线程同步事件，可以处于已发出信号状态或未发出信号状态。
- <xref:System.Threading.AutoResetEvent?displayProperty=nameWithType>，派生自 <xref:System.Threading.EventWaitHandle>，当发出信号时，会在发布单个等待线程后自动重置为未发出信号状态。
- <xref:System.Threading.ManualResetEvent?displayProperty=nameWithType>，派生自 <xref:System.Threading.EventWaitHandle>，当发出信号时，会保持已发出信号状态，直到调用 <xref:System.Threading.EventWaitHandle.Reset%2A> 方法。

在 .NET Framework 中，由于 <xref:System.Threading.WaitHandle> 派生自 <xref:System.MarshalByRefObject?displayProperty=nameWithType>，因此，这些类型可用于跨应用程序域边界同步线程的活动。

在 .NET Framework、.NET Core 和 .NET 5+ 中，其中的一些类型可以表示已命名的系统同步句柄，这些句柄在整个操作系统中都可见并可用于进程间同步：

- <xref:System.Threading.Mutex>
- <xref:System.Threading.Semaphore>（在 Windows 上）
- <xref:System.Threading.EventWaitHandle>（在 Windows 上）

有关详细信息，请参阅 <xref:System.Threading.WaitHandle> API 参考。

轻量同步类型不依赖于基础操作系统句柄，通常会提供更好的性能。 但是，它们不能用于进程间同步。 将这些类型用于一个应用程序中的线程同步。

其中的一些类型是派生自 <xref:System.Threading.WaitHandle> 的类型的替代项。 例如，<xref:System.Threading.SemaphoreSlim> 是 <xref:System.Threading.Semaphore> 的轻量替代项。

## <a name="synchronization-of-access-to-a-shared-resource"></a>同步对共享资源的访问

.NET 提供了一系列用于控制多个线程对共享资源的访问的同步基元。

### <a name="monitor-class"></a>Monitor 类

<xref:System.Threading.Monitor?displayProperty=nameWithType> 类通过获取或释放用于标识资源的对象上的 lock 来授予对共享资源的相互独占访问权限。 持有 lock 时，持有 lock 的线程可以再次获取并释放 lock。 阻止任何其他线程获取 lock，<xref:System.Threading.Monitor.Enter%2A?displayProperty=nameWithType> 方法等待释放 lock。 <xref:System.Threading.Monitor.Enter%2A> 方法可获取释放的 lock。 还可以使用 <xref:System.Threading.Monitor.TryEnter%2A?displayProperty=nameWithType> 方法指定线程尝试获取 lock 的持续时间。 由于 <xref:System.Threading.Monitor> 类具有线程关联，因此获取了 lock 的线程必须通过调用 <xref:System.Threading.Monitor.Exit%2A?displayProperty=nameWithType> 方法来释放 lock。

可以通过使用 <xref:System.Threading.Monitor.Wait%2A?displayProperty=nameWithType>、<xref:System.Threading.Monitor.Pulse%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Monitor.PulseAll%2A?displayProperty=nameWithType> 方法来协调用于获取同一对象上的 lock 的线程的交互。

有关详细信息，请参阅 <xref:System.Threading.Monitor> API 参考。

> [!NOTE]
> 使用 C# 中的 [lock](../../csharp/language-reference/keywords/lock-statement.md) 语句和 Visual Basic 中的 [SyncLock](../../visual-basic/language-reference/statements/synclock-statement.md) 语句同步对共享资源的访问，而不是直接使用 <xref:System.Threading.Monitor> 类。 这些语句是使用 <xref:System.Threading.Monitor.Enter%2A> 和 <xref:System.Threading.Monitor.Exit%2A> 方法进行实现，并使用 `try…finally` 块来确保获取的 lock 已始终释放。

### <a name="mutex-class"></a>Mutex 类

<xref:System.Threading.Mutex?displayProperty=nameWithType> 类（与 <xref:System.Threading.Monitor> 类似），授予对共享资源的独占访问权限。 使用 [Mutex.WaitOne](<xref:System.Threading.WaitHandle.WaitOne%2A?displayProperty=nameWithType>) 方法重载之一请求 mutex 的所有权。 <xref:System.Threading.Mutex>（与 <xref:System.Threading.Monitor> 类似）具有线程关联，并且已获取 mutex 的线程必须通过调用 <xref:System.Threading.Mutex.ReleaseMutex%2A?displayProperty=nameWithType> 方法来释放它。

<xref:System.Threading.Mutex> 类（与 <xref:System.Threading.Monitor> 不同）可用于进程间同步。 为此，请使用命名 mutex，它在整个操作系统中都可见。 若要创建命名 mutex 实例，请使用指定了名称的 [Mutex 构造函数](<xref:System.Threading.Mutex.%23ctor%2A>)。 还可以调用 <xref:System.Threading.Mutex.OpenExisting%2A?displayProperty=nameWithType> 方法来打开现有的命名系统 mutex。
  
有关详细信息，请参阅 [Mutex](mutexes.md) 一文和 <xref:System.Threading.Mutex> API 参考。

### <a name="spinlock-structure"></a>SpinLock 结构

<xref:System.Threading.SpinLock?displayProperty=nameWithType> 结构（类似于 <xref:System.Threading.Monitor>），基于 lock 的可用性授予对共享资源的独占访问权限。 当 <xref:System.Threading.SpinLock> 尝试获取不可用的 lock 时，将在反复检查的循环中等待，直到 lock 变为可用。

有关使用 SpinLock 的优缺点的详细信息，请参阅 [SpinLock](spinlock.md) 一文和 <xref:System.Threading.SpinLock> API 参考。

### <a name="readerwriterlockslim-class"></a>ReaderWriterLockSlim 类

<xref:System.Threading.ReaderWriterLockSlim?displayProperty=nameWithType> 类授予对共享资源的独占访问权限以便进行写入，并允许多个线程同时访问资源以便进行读取。 你可能想要使用 <xref:System.Threading.ReaderWriterLockSlim> 同步对支持线程安全读取操作，但需要独占访问权限才能执行写入操作的共享数据结构的访问。 当某个线程请求独占访问时（例如，通过调用 <xref:System.Threading.ReaderWriterLockSlim.EnterWriteLock%2A?displayProperty=nameWithType> 方法），后续读取器和编写器请求将被阻止，直到所有现有读取器均已退出 lock，并且编写器已进入并退出 lock。
  
有关详细信息，请参阅 <xref:System.Threading.ReaderWriterLockSlim> API 参考。

### <a name="semaphore-and-semaphoreslim-classes"></a>Semaphore 和 SemaphoreSlim 类

<xref:System.Threading.Semaphore?displayProperty=nameWithType> 和 <xref:System.Threading.SemaphoreSlim?displayProperty=nameWithType> 限制可同时访问某一共享资源或资源池的线程数。 请求资源的其他线程将等待，直到任何线程释放信号量。 由于信号量没有线程关联，因此一个线程可以获取信号量，而另一个线程可以释放它。

<xref:System.Threading.SemaphoreSlim> 是 <xref:System.Threading.Semaphore> 的轻量替代项，并且只能在单个流程边界内用于同步。

在 Windows 上，可以将 <xref:System.Threading.Semaphore> 用于进程间同步。 为此，通过使用指定了名称或 <xref:System.Threading.Semaphore.OpenExisting%2A?displayProperty=nameWithType> 方法的 [Semaphore 构造函数](<xref:System.Threading.Semaphore.%23ctor%2A>)之一来创建表示指定了已命名系统信号量的 <xref:System.Threading.Semaphore> 实例。 <xref:System.Threading.SemaphoreSlim> 不支持已命名系统信号量。

有关详细信息，请参阅 [Semaphore 和 SemaphoreSlim](semaphore-and-semaphoreslim.md) 一文以及 <xref:System.Threading.Semaphore> 或 <xref:System.Threading.SemaphoreSlim> API 参考。

## <a name="thread-interaction-or-signaling"></a>线程交互或信号

线程交互（或线程信号）表示线程必须等待来自一个或多个线程的通知或信号才能继续。 例如，如果线程 A 调用线程 B 的 <xref:System.Threading.Thread.Join%2A?displayProperty=nameWithType> 方法，则线程 A 将被阻止，直到完成线程 B。 前面部分中所述的同步基元提供不同的信号机制：通过释放 lock，一个线程通知另一个线程可以通过获取 lock 来继续。

本部分介绍 .NET 提供的其他信号构造。

### <a name="eventwaithandle-autoresetevent-manualresetevent-and-manualreseteventslim-classes"></a>EventWaitHandle、AutoResetEvent、ManualResetEvent 和 ManualResetEventSlim 类

<xref:System.Threading.EventWaitHandle?displayProperty=nameWithType> 类表示一个线程同步事件。

同步事件可以处于未发出信号状态或已发出信号状态。 当事件的状态为未发出信号时，调用了事件的 <xref:System.Threading.WaitHandle.WaitOne%2A?> 重载的线程会被阻止，直到事件处于已发出信号状态。 <xref:System.Threading.EventWaitHandle.Set%2A?displayProperty=nameWithType> 方法可将事件的状态设置为已发出信号。

已发出信号的 <xref:System.Threading.EventWaitHandle> 的行为取决于其重置模式：

- 使用 <xref:System.Threading.EventResetMode.AutoReset?displayProperty=nameWithType> 标志创建的 <xref:System.Threading.EventWaitHandle> 会在释放单个等待线程后自动进行重置。 就像旋转栅在每次发出信号时仅允许一个线程通过一样。 派生自 <xref:System.Threading.EventWaitHandle> 的 <xref:System.Threading.AutoResetEvent?displayProperty=nameWithType> 类表示该行为。
- 在 <xref:System.Threading.EventWaitHandle.Reset%2A> 方法获得调用前，一直向使用 <xref:System.Threading.EventResetMode.ManualReset?displayProperty=nameWithType> 标志创建的 <xref:System.Threading.EventWaitHandle> 发出信号。 就像接收到信号前保持关闭、然后在被关闭前保持打开的大门一样。 派生自 <xref:System.Threading.EventWaitHandle> 的 <xref:System.Threading.ManualResetEvent?displayProperty=nameWithType> 类表示该行为。 <xref:System.Threading.ManualResetEventSlim?displayProperty=nameWithType> 类是 <xref:System.Threading.ManualResetEvent> 的轻量替代项。

在 Windows 上，可以将 <xref:System.Threading.EventWaitHandle> 用于进程间同步。 为此，通过使用指定了名称或 <xref:System.Threading.EventWaitHandle.OpenExisting%2A?displayProperty=nameWithType> 方法的 [EventWaitHandle 构造函数](<xref:System.Threading.EventWaitHandle.%23ctor%2A>)之一来创建表示指定了已命名系统信号量的 <xref:System.Threading.EventWaitHandle> 实例。

有关详细信息，请参阅文章 [EventWaitHandle](eventwaithandle.md)。 对于 API 参考，请参阅 <xref:System.Threading.EventWaitHandle>、<xref:System.Threading.AutoResetEvent>、<xref:System.Threading.ManualResetEvent> 和 <xref:System.Threading.ManualResetEventSlim>。

### <a name="countdownevent-class"></a>CountdownEvent 类

<xref:System.Threading.CountdownEvent?displayProperty=nameWithType> 类表示当其计数为零时将被设置的事件。 当 <xref:System.Threading.CountdownEvent.CurrentCount?displayProperty=nameWithType> 大于零时，调用了 <xref:System.Threading.CountdownEvent.Wait%2A?displayProperty=nameWithType> 的线程会被阻止。 调用 <xref:System.Threading.CountdownEvent.Signal%2A?displayProperty=nameWithType> 会递减事件的计数。

与可用于通过来自一个线程的信号取消阻止多个线程的 <xref:System.Threading.ManualResetEvent> 或 <xref:System.Threading.ManualResetEventSlim> 相反，你可以使用 <xref:System.Threading.CountdownEvent> 通过来自多个线程的信号取消阻止一个或多个线程。

有关详细信息，请参阅 [CountdownEvent](countdownevent.md) 一文和 <xref:System.Threading.CountdownEvent> API 参考。

### <a name="barrier-class"></a>Barrier 类

<xref:System.Threading.Barrier?displayProperty=nameWithType> 类表示线程执行屏障。 调用了 <xref:System.Threading.Barrier.SignalAndWait%2A?displayProperty=nameWithType> 方法的线程发出信号，它已到达屏障并等待其他参与线程到达屏障。 当所有参与线程到达屏障时，它们将继续前进，屏障将进行重置，使之再次可用。

在继续执行下一个计算阶段之前，当一个或多个线程需要其他线程的结果时，可以使用 <xref:System.Threading.Barrier>。

有关详细信息，请参阅 [Barrier](barrier.md) 一文和 <xref:System.Threading.Barrier> API 参考。

## <a name="interlocked-class"></a>Interlocked 类

<xref:System.Threading.Interlocked?displayProperty=nameWithType> 类提供了可对变量执行简单原子操作的静态方法。 这些原子操作包括添加、递增和递减、交换、取决于比较的条件交换以及读取 64 位整数值的操作。

有关详细信息，请参阅 <xref:System.Threading.Interlocked> API 参考。

## <a name="spinwait-structure"></a>SpinWait 结构

<xref:System.Threading.SpinWait?displayProperty=nameWithType> 结构为基于自旋的等待提供支持。 如果线程必须等待事件收到信号或必须满足某种条件，但实际等待时间应短于使用等待句柄或以其他方式阻止线程所需的等待时间，你可能想要使用该结构。 通过使用 <xref:System.Threading.SpinWait>，你可以指定等待期间要旋转的一小段时间，且只在特定时间不满足条件时让行（例如，通过等待或休眠）。

有关详细信息，请参阅 [SpinWait](spinwait.md) 一文和 <xref:System.Threading.SpinWait> API 参考。

## <a name="see-also"></a>请参阅

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- [线程安全集合](../collections/thread-safe/index.md)
- [线程处理对象和功能](threading-objects-and-features.md)
