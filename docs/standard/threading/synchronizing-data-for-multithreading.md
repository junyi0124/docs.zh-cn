---
title: 为多线程处理同步数据
description: 了解如何在 .NET 中同步多线程处理数据。 选择同步代码区域、手动同步或同步上下文等策略。
ms.date: 03/30/2017
helpviewer_keywords:
- synchronization, threads
- threading [.NET], synchronizing threads
- managed threading
ms.assetid: b980eb4c-71d5-4860-864a-6dfe3692430a
ms.openlocfilehash: e1b90bdc5657c1fd22c6e77e31890ff63c3cc3ea
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95727453"
---
# <a name="synchronizing-data-for-multithreading"></a>为多线程处理同步数据

多个线程可以调用单个对象的属性和方法时，对这些调用进行同步处理是非常重要的。 否则，一个线程可能会中断另一个线程正在执行的任务，可能使该对象处于无效状态。 其成员不受这类中断影响的类叫做线程安全类。  
  
.NET 提供了几种策略，用于同步对实例和静态成员的访问：  
  
- 同步代码区域。 可以使用 <xref:System.Threading.Monitor> 类或此类的编译器支持，仅同步需要它的代码块，从而提升性能。  
  
- 手动同步。 可以使用 .NET 类库提供的同步对象。 请参阅[同步基元概述](overview-of-synchronization-primitives.md)，其中介绍了 <xref:System.Threading.Monitor> 类。  
  
- 同步上下文。 仅对于 .NET Framework 和 Xamarin 应用程序，你可以使用 <xref:System.Runtime.Remoting.Contexts.SynchronizationAttribute> 为 <xref:System.ContextBoundObject> 对象启用简单的自动同步。  
  
- <xref:System.Collections.Concurrent?displayProperty=nameWithType> 命名空间中的集合类。 这些类提供了内置的同步添加和删除操作。 有关详细信息，请参阅[线程安全集合](../collections/thread-safe/index.md)。  
  
 公共语言运行时提供一个线程模型，在该模型中，类分为多种类别，这些类别可以根据要求以各种不同的方式进行同步。 下表显示了为具有给定同步类别的字段和方法提供的同步支持。  
  
|类别|全局字段|静态字段|静态方法|实例字段|实例方法|特定代码块|  
|--------------|-------------------|-------------------|--------------------|---------------------|----------------------|--------------------------|  
|无同步|否|否|否|否|否|否|  
|同步上下文|否|否|否|是|是|否|  
|同步代码区域|否|否|仅当标记时|否|仅当标记时|仅当标记时|  
|手动同步|手动|手动|手动|手动|手动|手动|  
  
## <a name="no-synchronization"></a>无同步  

 这是对象的默认情况。 任何线程都可以随时访问任何方法或字段。 一次只能有一个线程访问这些对象。  
  
## <a name="manual-synchronization"></a>手动同步  

 .NET 类库提供大量用于同步线程的类。 请参阅[同步基元概述](overview-of-synchronization-primitives.md)。  
  
## <a name="synchronized-code-regions"></a>同步代码区域  

 可以使用 <xref:System.Threading.Monitor> 类或编译器关键字，同步代码块、实例方法和静态方法。 不支持同步静态字段。  
  
 Visual Basic 和 C# 都支持使用特定语言关键字标记代码块，在 C# 中使用的是 `lock` 语句，在 Visual Basic 中使用的是 `SyncLock` 语句。 由线程执行代码时，会尝试获取锁。 如果该锁已由其他线程获取，则在锁变为可用状态之前，该线程一直处于阻止状态。 线程退出同步代码块时，锁会被释放，与线程的退出方式无关。  
  
> [!NOTE]
> 由于 `lock` 和 `SyncLock` 语句是使用 <xref:System.Threading.Monitor.Enter%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Monitor.Exit%2A?displayProperty=nameWithType> 实现，因此 <xref:System.Threading.Monitor> 的其他方法可以在同步区域内与它们结合使用。  
  
 还可以使用值为 <xref:System.Runtime.CompilerServices.MethodImplOptions.Synchronized?displayProperty=nameWithType> 的 <xref:System.Runtime.CompilerServices.MethodImplAttribute> 修饰方法，其效果和使用 <xref:System.Threading.Monitor> 或其中一个编译器关键字锁定整个方法正文相同。  
  
 <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> 可用于中断对线程执行阻止操作（如等待访问同步代码区域）。 Thread.Interrupt 还用于中断对线程执行 <xref:System.Threading.Thread.Sleep%2A?displayProperty=nameWithType> 等操作。  
  
> [!IMPORTANT]
> 为保护 `static` 方法（Visual Basic 中的 `Shared` 方法），请不要锁定类型，即：C# 中的 `typeof(MyType)`、Visual Basic 中的 `GetType(MyType)` 或 C++ 中的 `MyType::typeid`。 请改用私有静态对象。 同样，不要使用 C# 中的 `this`（Visual Basic 中的 `Me`）锁定实例方法。 请改用私有对象。 类或实例可由不是你自己的代码锁定，这可能会引起死锁或性能问题。  
  
### <a name="compiler-support"></a>编译器支持  

 Visual Basic 和 C# 均支持使用 <xref:System.Threading.Monitor.Enter%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Monitor.Exit%2A?displayProperty=nameWithType> 锁定对象的语言关键字。 Visual Basic 支持 [SyncLock](../../visual-basic/language-reference/statements/synclock-statement.md) 语句；C# 支持 [lock](../../csharp/language-reference/keywords/lock-statement.md) 语句。  
  
 在这两种情况下，如果代码块中引发异常，则 **lock** 或 **SyncLock** 获取的锁将自动释放。 C# 和 Visual Basic 编译器在发出 **try**/**finally** 块时，在 try 的起始处使用 **Monitor.Enter**，在 **finally** 块中使用 **Monitor.Exit**。 如果 **lock** 或 **SyncLock** 块内部引发了异常，则会运行 **finally** 处理程序，从而允许执行任何清除工作。  
  
## <a name="synchronized-context"></a>同步上下文  

仅在 .NET Framework 和 Xamarin 应用程序中，可以使用任何 <xref:System.ContextBoundObject> 上的 <xref:System.Runtime.Remoting.Contexts.SynchronizationAttribute> 来同步所有实例方法和字段。 同一上下文域中的所有对象都共享同一个锁。 允许多个线程访问方法和字段，但在任一时刻只允许一个线程访问。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Runtime.Remoting.Contexts.SynchronizationAttribute>
- [线程与线程处理](threads-and-threading.md)
- [同步基元概述](overview-of-synchronization-primitives.md)
- [SyncLock 语句](../../visual-basic/language-reference/statements/synclock-statement.md)
- [lock 语句](../../csharp/language-reference/keywords/lock-statement.md)
