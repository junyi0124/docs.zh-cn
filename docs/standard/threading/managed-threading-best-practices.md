---
title: 托管线程处理的最佳做法
description: 了解 .NET 中托管线程处理的最佳做法。 处理复杂的情况，如协调多个线程或处理阻塞线程。
ms.date: 10/15/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- threading [.NET], design guidelines
- threading [.NET], best practices
- managed threading
ms.assetid: e51988e7-7f4b-4646-a06d-1416cee8d557
ms.openlocfilehash: 88cbf266d15a10ff7c56e07a30161e0a800989d5
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95708044"
---
# <a name="managed-threading-best-practices"></a>托管线程处理的最佳做法

多线程处理需在编程时倍加注意。 对于多数任务，通过将执行请求以线程池线程的方式排队，可以降低复杂性。 本主题将探讨更复杂的情形，比如协调多个线程的工作或处理造成阻止的线程。  
  
> [!NOTE]
> 自 .NET Framework 4 起，任务并行库和 PLINQ 提供了 API，可降低多线程编程的一些复杂性和风险。 有关详细信息，请参阅 [.NET 中的并行编程](../parallel-programming/index.md)。  
  
## <a name="deadlocks-and-race-conditions"></a>死锁和争用条件  

 多线程处理解决了吞吐量和响应性问题，但引入此功能会带来新的问题：死锁和争用条件。  
  
### <a name="deadlocks"></a>死锁  

 两个线程中的每一个线程都尝试锁定另外一个线程已锁定的资源时，就会发生死锁。 两个线程都不能继续执行。  
  
 托管线程处理类的许多方法都提供了超时设定，有助于检测死锁。 例如，下面的代码尝试在 `lockObject` 对象上获取锁。 如果在 300 毫秒内没有获取锁，<xref:System.Threading.Monitor.TryEnter%2A?displayProperty=nameWithType> 返回 `false`。  
  
```vb  
If Monitor.TryEnter(lockObject, 300) Then  
    Try  
        ' Place code protected by the Monitor here.  
    Finally  
        Monitor.Exit(lockObject)  
    End Try  
Else  
    ' Code to execute if the attempt times out.  
End If  
```  
  
```csharp  
if (Monitor.TryEnter(lockObject, 300)) {  
    try {  
        // Place code protected by the Monitor here.  
    }  
    finally {  
        Monitor.Exit(lockObject);  
    }  
}  
else {  
    // Code to execute if the attempt times out.  
}  
```  
  
### <a name="race-conditions"></a>争用条件  

 争用条件是程序的结果取决于两个或更多个线程中的哪一个先到达某一特定代码块时出现的一种 bug。 多次运行程序会产生不同的结果，并且无法预测任何给定运行的结果。  
  
 争用条件的一个简单例子是递增一个字段。 假定某个类有一个私有 **static** 字段（在 Visual Basic 中为 **Shared**），每创建该类的一个实例时它都递增一次，使用的代码是 `objCt++;` (C#) 或 `objCt += 1` (Visual Basic)。 此操作要求将 `objCt` 的值加载到寄存器中，使该值递增，然后将其存储到 `objCt` 中。  
  
 在多线程应用程序中，一个已加载并递增该值的线程可能会被另一个线程抢先，抢先的线程执行全部三个步骤；第一个线程继续执行并存储其值时，它会覆盖 `objCt`，但不考虑该值在其暂停执行期间已更改这一事实。  
  
 通过使用 <xref:System.Threading.Interlocked> 类的方法（如 <xref:System.Threading.Interlocked.Increment%2A?displayProperty=nameWithType>），可以轻松避免这种争用条件。 若要了解在多个线程间同步数据的其他技巧，请参阅[为多线程处理同步数据](synchronizing-data-for-multithreading.md)。  
  
 争用条件也可能会在同步多个线程的活动时发生。 编写每一行代码时，都必须考虑出现以下情况时会发生什么情况：一个线程在执行该行代码（或构成该行的任何机器指令）前，其他线程抢先执行了该代码。  
  
## <a name="static-members-and-static-constructors"></a>静态成员和静态构造函数  

 在类的类构造函数（C# 中为 `static` 构造函数、Visual Basic 中为 `Shared Sub New`）完成运行之前，该类不会初始化。 为防止对未初始化的类型执行代码，在类构造函数完成运行之前，公共语言运行时会禁止从其他线程到类的 `static` 成员（在 Visual Basic 中为 `Shared` 成员）的所有调用。  
  
 例如，如果某个类构造函数启动了一个新线程，并且该线程过程调用了该类的 `static` 成员，则在该类构造函数完成之前，会一直禁止新线程。  
  
 以上情况适用于可拥有 `static` 构造函数的任意类型。  

## <a name="number-of-processors"></a>处理器数量

系统中是有多个处理器还是仅有一个处理器会影响多线程体系结构。 有关详细信息，请参阅[处理器数量](/previous-versions/dotnet/netframework-1.1/1c9txz50(v=vs.71)#number-of-processors)。

使用 <xref:System.Environment.ProcessorCount?displayProperty=nameWithType> 属性来确定在运行时可用的处理器数量。
  
## <a name="general-recommendations"></a>一般性建议  

 使用多线程时需考虑以下准则：  
  
- 请勿使用 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> 终止其他线程。 对另一个线程调用 **Abort** 无异于引发该线程的异常，也不知道该线程已处理到哪个位置。  
  
- 请勿使用 <xref:System.Threading.Thread.Suspend%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Thread.Resume%2A?displayProperty=nameWithType> 同步多个线程的活动。 请使用 <xref:System.Threading.Mutex>、<xref:System.Threading.ManualResetEvent>、<xref:System.Threading.AutoResetEvent> 和 <xref:System.Threading.Monitor>。  
  
- 不要从主程序中控制工作线程的执行（如使用事件）。 而应设计程序，使工作线程负责等待任务可用，然后执行任务，并在完成时通知程序的其他部分。 如果不阻止工作线程，请考虑使用线程池线程。 <xref:System.Threading.Monitor.PulseAll%2A?displayProperty=nameWithType> 非常适用于阻止工作线程。  
  
- 不要将类型用作锁定对象。 也就是说，避免一些代码，如 C# 中的 `lock(typeof(X))` 或 Visual Basic 中的 `SyncLock(GetType(X))`，或避免使用 <xref:System.Threading.Monitor.Enter%2A?displayProperty=nameWithType> 和 <xref:System.Type> 对象。 对于给定类型，每个应用域只有一个 <xref:System.Type?displayProperty=nameWithType> 实例。 如果锁定对象的类型是“公共的”，那么不属于自己的代码也能锁定该对象，从而导致死锁。 有关其他问题，请参阅[可靠性最佳做法](../../framework/performance/reliability-best-practices.md)。  
  
- 锁定实例时要谨慎，例如，C# 中的 `lock(this)` 或 Visual Basic 中的 `SyncLock(Me)`。 如果应用程序中不属于该类型的其他代码锁定了该对象，则会发生死锁。  
  
- 请务必确保已进入监视器的线程始终离开该监视器，即使线程在监视器中时发生异常也是如此。 C# 的 [lock](../../csharp/language-reference/keywords/lock-statement.md) 语句和 Visual Basic 的 [SyncLock](../../visual-basic/language-reference/statements/synclock-statement.md) 语句可自动提供此行为，同时使用 **finally** 块来确保调用 <xref:System.Threading.Monitor.Exit%2A?displayProperty=nameWithType>。 如果无法确保调用 **Exit**，请考虑将设计更改为使用 **Mutex**。 Mutex 在当前拥有它的线程终止后会自动释放。  
  
- 请务必针对需要不同资源的任务使用多线程，避免向单个资源指定多个线程。 例如，任何涉及 I/O 的任务都会从其拥有自己的线程这一点得到好处，因为此线程在 I/O 操作期间将阻止，从而允许其他线程执行。 用户输入是另一种可从专用线程获益的资源。 在单处理器计算机上，涉及大量计算的任务可与用户输入和涉及 I/O 的任务并存，但多个计算量大的任务将相互竞争。  
  
- 对于简单的状态更改，请考虑使用 <xref:System.Threading.Interlocked> 类的方法，而不是 `lock` 语句（Visual Basic 中的 `SyncLock`）。 虽然 `lock` 语句是实用的通用工具，但 <xref:System.Threading.Interlocked> 类提升了更新（必须是原子操作）的性能。 如果不存在争用，它会在内部执行一个锁定前缀。 查看代码时，请注意类似于以下示例所示的代码。 在第一个示例中，状态变量是递增的：  
  
    ```vb  
    SyncLock lockObject  
        myField += 1  
    End SyncLock  
    ```  
  
    ```csharp  
    lock(lockObject)
    {  
        myField++;  
    }  
    ```  
  
     若要提升性能，可以使用 <xref:System.Threading.Interlocked.Increment%2A> 方法代替 `lock` 语句，如下所示：  
  
    ```vb  
    System.Threading.Interlocked.Increment(myField)  
    ```  
  
    ```csharp  
    System.Threading.Interlocked.Increment(myField);  
    ```  
  
    > [!NOTE]
    > 为大于 1 的原子增量使用 <xref:System.Threading.Interlocked.Add%2A> 方法。  
  
     在第二个示例中，仅当引用类型变量为空引用（在 Visual Basic 中为 `Nothing`）时才会将其更新。  
  
    ```vb  
    If x Is Nothing Then  
        SyncLock lockObject  
            If x Is Nothing Then  
                x = y  
            End If  
        End SyncLock  
    End If  
    ```  
  
    ```csharp  
    if (x == null)  
    {  
        lock (lockObject)  
        {  
            x ??= y;
        }  
    }  
    ```  
  
     可以改用 <xref:System.Threading.Interlocked.CompareExchange%2A> 方法提升性能，如下所示：  
  
    ```vb  
    System.Threading.Interlocked.CompareExchange(x, y, Nothing)  
    ```  
  
    ```csharp  
    System.Threading.Interlocked.CompareExchange(ref x, y, null);  
    ```  
  
    > [!NOTE]
    > <xref:System.Threading.Interlocked.CompareExchange%60%601%28%60%600%40%2C%60%600%2C%60%600%29> 方法重载为引用类型提供类型安全的替代项。
  
## <a name="recommendations-for-class-libraries"></a>类库相关建议  

 为多线程处理设计类库时，请考虑以下准则：  
  
- 如果可能，请避免同步需求。 对于大量使用的代码更应如此。 例如，可以将一个算法调整为容忍争用情况，而不是完全消除争用情况。 不必要的同步会降低性能，并且可能导致出现死锁和争用情况。  
  
- 默认情况下使静态数据（在 Visual Basic 中为 `Shared`）是线程安全的。  
  
- 默认情况下不要使实例数据是线程安全的。 通过添加锁来创建线程安全代码会降低性能、加剧锁争用情况，并且可能导致出现死锁。 在常见应用程序模型中，一次只有一个线程执行用户代码，从而最大限度降低线程安全性的需求。 出于此原因，.NET 类库在默认情况下不是线程安全的类库。  
  
- 避免提供可更改静态状态的静态方法。 在常见服务器方案中，静态状态可在各个请求之间共享，这意味着多个线程可同时执行该代码。 这可能导致线程出现 bug。 请考虑使用一种设计模式，将数据封装到在各请求之间不共享的实例中。 此外，如果同步静态数据，更改状态的静态方法间的调用可导致死锁或冗余同步，进而降低性能。  
  
## <a name="see-also"></a>请参阅

- [线程处理](index.md)
- [线程与线程处理](threads-and-threading.md)
