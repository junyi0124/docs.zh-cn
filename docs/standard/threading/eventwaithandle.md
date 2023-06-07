---
title: EventWaitHandle
ms.date: 03/30/2017
helpviewer_keywords:
- threading [.NET], EventWaitHandle class
- EventWaitHandle class
- event wait handles [.NET]
- threading [.NET], cross-process synchronization
ms.assetid: 11ee0b38-d663-4617-b793-35eb6c64e9fc
ms.openlocfilehash: 078bda2354a6f0aec2215b0c5da2a021f53ff922
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723779"
---
# <a name="eventwaithandle"></a>EventWaitHandle

借助 <xref:System.Threading.EventWaitHandle> 类，线程可以通过发出信号和等待信号进行相互通信。 事件等待句柄（亦简称为“事件”）是可以收到信号以释放一个或多个等待线程的等待句柄。 收到信号后，事件等待句柄便会进行手动或自动重置。 <xref:System.Threading.EventWaitHandle> 类可以表示本地事件等待句柄（本地事件），也可以表示命名系统事件等待句柄（对所有进程可见的命名事件或系统事件）。  
  
> [!NOTE]
> 事件等待句柄不是 .NET [事件](../events/index.md)。 并不涉及任何委托或事件处理程序。 之所以使用“事件”一词是因为，它们一直都被称为操作系统事件，并且向等待句柄发出信号可以向等待线程指明事件已发生。  
  
 本地和命名事件等待句柄均使用系统同步对象。为了确保资源获得释放，这些对象受 <xref:Microsoft.Win32.SafeHandles.SafeWaitHandle> 包装器保护。 可以使用 <xref:System.Threading.WaitHandle.Dispose%2A> 方法，在使用完对象后立即释放资源。  
  
## <a name="event-wait-handles-that-reset-automatically"></a>自动重置的事件等待句柄  

 若要创建自动重置事件，可以在创建 <xref:System.Threading.EventWaitHandle> 对象时指定 <xref:System.Threading.EventResetMode.AutoReset?displayProperty=nameWithType>。 顾名思义，此同步事件在一个等待线程释放后收到信号时自动重置。 若要向事件发出信号，请调用它的 <xref:System.Threading.EventWaitHandle.Set%2A> 方法。  
  
 自动重置事件通常用于一次向一个线程提供对资源的独占访问权限。 线程通过调用 <xref:System.Threading.WaitHandle.WaitOne%2A> 方法来请求获取资源。 如果其他线程都没有等待句柄，此方法返回 `true`，且调用线程可以控制资源。  
  
> [!IMPORTANT]
> 与所有同步机制一样，必须确保在访问受保护的资源前，所有代码路径都在相应的等待句柄上等待。 线程同步具有协作性。  
  
 如果向自动重置事件发出信号时没有线程正在等待，此信号会一直发出到有线程尝试在等待句柄上等待。 此时，事件会释放相应线程并立即重置自身，同时阻止后续线程。  
  
## <a name="event-wait-handles-that-reset-manually"></a>手动重置的事件等待句柄  

 若要创建手动重置事件，可以在创建 <xref:System.Threading.EventWaitHandle> 对象时指定 <xref:System.Threading.EventResetMode.ManualReset?displayProperty=nameWithType>。 顾名思义，此同步事件必须在收到信号后进行手动重置。 调用 <xref:System.Threading.EventWaitHandle.Reset%2A> 方法重置事件前，在事件句柄上等待的线程会立即继续运行，而不受阻止。  
  
 手动重置事件如同畜栏口一样。 如果事件未收到信号，在事件句柄上等待的线程受阻止，如同畜栏中的马一样。 通过调用 <xref:System.Threading.EventWaitHandle.Set%2A> 方法向事件发出信号后，所有等待线程都获得释放，可以继续执行。 在 <xref:System.Threading.EventWaitHandle.Reset%2A> 方法获得调用前，一直向事件发出信号。 这样一来，手动重置事件就非常适用于，阻止需要等待一个线程完成任务的线程。  
  
 就像马离开畜栏一样，获释放的线程需要一定的时间，操作系统才能排入计划和恢复执行。 如果在所有线程恢复执行前 <xref:System.Threading.EventWaitHandle.Reset%2A> 方法获得调用，剩余线程将再次受阻止。 恢复哪些线程以及阻止哪些线程都取决于随机因素，如系统负载、等待计划程序的线程数等。 如果向事件发出信号的线程在发出信号后结束（这是最常见的使用模式），这就不存在问题。 如果希望向事件发出信号的线程在所有等待线程恢复后启动新任务，必须将它一直阻止到所有等待线程都已恢复。 否则，将会出现争用条件，而且代码行为也会变得不可预测。  
  
## <a name="features-common-to-automatic-and-manual-events"></a>自动和手动事件的常见功能  

 通常情况下，一个或多个线程在 <xref:System.Threading.EventWaitHandle> 上一直受阻止到未受阻止的线程调用 <xref:System.Threading.EventWaitHandle.Set%2A> 方法，此方法释放等待线程之一（如果是自动重置事件）或全部线程（如果是手动重置事件）。 线程可以向 <xref:System.Threading.EventWaitHandle> 发出信号，然后调用静态 <xref:System.Threading.WaitHandle.SignalAndWait%2A?displayProperty=nameWithType> 方法以原子操作的形式在其中受阻止。  
  
 <xref:System.Threading.EventWaitHandle> 对象可以与静态 <xref:System.Threading.WaitHandle.WaitAll%2A?displayProperty=nameWithType> 和 <xref:System.Threading.WaitHandle.WaitAny%2A?displayProperty=nameWithType> 方法结合使用。 由于 <xref:System.Threading.EventWaitHandle> 和 <xref:System.Threading.Mutex> 类均派生自 <xref:System.Threading.WaitHandle>，因此可以将这两个类与这些方法结合使用。  
  
### <a name="named-events"></a>命名事件  

 Windows 操作系统允许命名事件等待句柄。 命名事件的范围覆盖整个系统。 也就是说，一旦创建，命名事件就对所有进程中的全部线程可见。 因此，命名事件可用于同步进程和线程的活动。  
  
 可以使用指定事件名称的构造函数之一，创建表示命名系统事件的 <xref:System.Threading.EventWaitHandle> 对象。  
  
> [!NOTE]
> 由于命名事件的范围覆盖整个系统，因此可能有多个 <xref:System.Threading.EventWaitHandle> 对象表示同一命名事件。 每次调用构造函数或 <xref:System.Threading.EventWaitHandle.OpenExisting%2A> 方法，都会新建一个 <xref:System.Threading.EventWaitHandle> 对象。 重复指定相同的名称也会创建多个表示同一命名事件的对象。  
  
 建议谨慎使用命名事件。 由于命名事件的范围覆盖整个系统，因此同名的另一进程可能会意外阻止线程。 同一台计算机上的恶意代码执行可将此作为拒绝服务攻击的基础。  
  
 借助访问控制安全性，可以保护表示命名事件的 <xref:System.Threading.EventWaitHandle> 对象，最好使用指定 <xref:System.Security.AccessControl.EventWaitHandleSecurity> 对象的构造函数。 还可以使用 <xref:System.Threading.EventWaitHandle.SetAccessControl%2A> 方法应用访问控制安全性，但这会导致在创建和保护事件等待句柄的时间间隔易受攻击。 虽然应用访问控制安全性来保护事件有助于防止恶意攻击，但不能解决无意间的名称冲突问题。  
  
> [!NOTE]
> 与 <xref:System.Threading.EventWaitHandle> 类不同，派生类 <xref:System.Threading.AutoResetEvent> 和 <xref:System.Threading.ManualResetEvent> 只能表示本地等待句柄。 无法表示命名系统事件。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Threading.EventWaitHandle>
- <xref:System.Threading.WaitHandle>
- <xref:System.Threading.AutoResetEvent>
- <xref:System.Threading.ManualResetEvent>
