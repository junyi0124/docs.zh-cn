---
title: 可靠性最佳做法
description: 请参阅基于 .NET 主机的服务器应用程序中的可靠性的最佳实践，例如 SQL Server。 阻止它们泄漏资源或关闭。
ms.date: 03/30/2017
helpviewer_keywords:
- marking locks
- rebooting databases
- denial of service attacks
- back-out code
- SQL Server [.NET Framework], reliability
- synchronization, reliability
- single-threaded COM components
- slow leaks
- suspending threads
- asynchronous exception handling
- leaked resources [.NET Framework]
- unmanaged memory
- memory, reliability
- threading [.NET Framework], reliability
- process-wide domain shared states
- shared states
- SafeHandle class, reliability
- reliability contracts [.NET Framework]
- cleanup operations
- constrained execution regions
- CERs
- finalizers, reliability
- reliability [.NET Framework]
- blocks, reliability
- finally clauses
- cross-application domain shared states
- catch blocks
- identifying locks
- writing reliable code
- impersonation
- GC.KeepAlive method
- managed threading
- locks, reliability
- STA-dependent features
- fibers
ms.assetid: cf624c1f-c160-46a1-bb2b-213587688da7
ms.openlocfilehash: 134b71153f95dffd4525f307d291ce4389e0ce60
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474236"
---
# <a name="reliability-best-practices"></a>可靠性最佳做法

以下可靠性规则是面向 SQL Server 的；但是，它们也适用于任何基于主机的服务器应用程序。 对 SQL Server 等服务器而言，不泄露资源且不会遭遇停机是极其重要的。  然而，这不能通过为改变对象状态的每个方法写入退出代码来实现。  目标不在于编写出将结合退出代码从每个位置的错误进行恢复的 100% 可靠的托管代码。  那将是一项艰巨的任务，并且成功的可能性较小。  公共语言运行时 (CLR) 无法轻松地向托管代码提供足够强大的保证以使编写出完美的代码成为可行的做法。  请注意，不同于 ASP.NET，SQL Server 仅使用一个进程，在没有将数据库关闭相当长的一段时间的情况下，此进程是无法被回收的。

由于这些较弱的保证以及在单个进程中运行的情况，因此，可靠性基于在必要时终止线程或回收应用程序域并采取预防措施以确保操作系统资源（如句柄或内存）不会泄露。  即使有此更简单的可靠性约束，但还是存在相当重要的可靠性要求：

- 绝不泄露操作系统资源。

- 识别针对 CLR 的各种形式的所有托管锁。

- 绝不中断跨应用程序域的共享状态，使 <xref:System.AppDomain> 回收能够平稳运行。

虽然，编写托管代码来处理 <xref:System.Threading.ThreadAbortException>、<xref:System.StackOverflowException> 和 <xref:System.OutOfMemoryException> 异常在理论上是可能的，但是指望开发人员在开发整个应用程序的过程中编写此类可靠的代码是不切实际的。  因此，带外异常会导致正在执行的线程被终止；如果此被终止的线程正在编辑共享状态（可由此线程是否持有锁来确定），那么 <xref:System.AppDomain> 会被卸载。  当正在编辑共享状态的方法被终止后，此状态将被损坏，因为无法为共享状态的更新编写出可靠的退出代码。

在 .NET Framework 版本 2.0 中，唯一需要可靠性的主机是 SQL Server。  如果程序集将在 SQL Server 上运行，则应该为此程序集的每部分执行可靠性操作，即使对于在数据库中运行时会被禁用的特定功能来说也不例外。  这是必需的操作，因为代码分析引擎是在程序集级别检查代码的，并且无法区分禁用的代码。 另一个 SQL Server 编程注意事项是 SQL Server 在一个进程中运行所有内容，并且 <xref:System.AppDomain> 回收是用于清理如内存和操作系统句柄等所有资源的。

不能依赖终结器、析构函数或 `try/finally` 块来执行退出代码。 它们可能会被中断或不受调用。

异步异常会在意外的位置中被引发，可能由每个计算机指令所引发：<xref:System.Threading.ThreadAbortException>、<xref:System.StackOverflowException> 和 <xref:System.OutOfMemoryException>。

托管线程在 SQL 中不一定是 Win32 线程；它们可能是纤程。

要安全地更改进程范围或跨应用程序域的可变共享状态是极其困难的，并且应尽可能地避免进行更改。

内存不足的情况在 SQL Server 中并不少见。

如果托管在 SQL Server 中的库没有正确地更新其共享状态，那么极有可能代码将在数据库重新启动之后才恢复。  此外，在某些极端情况下，这可能会导致 SQL Server 进程失败，从而导致数据库重启。  重启数据库可能会使网站关闭或者影响到公司运营，从而损害可用性。  操作系统资源（如内存或句柄）缓慢的泄露可能导致服务器最终出现分配失败的情况，并且无法进行恢复；或者可能会导致服务器的性能缓慢下降并降低客户的应用程序可用性。  显然，我们希望避免出现这类情况。

## <a name="best-practice-rules"></a>最佳做法规则

重点介绍对在服务器中运行的托管代码进行的代码评审需要抓住哪些方面以提高框架的稳定性和可靠性。 一般来说，所有这些检查都是好的做法，并且是绝对必须在服务器上进行的。

面对死锁或资源约束时，SQL Server 将中止线程或关闭 <xref:System.AppDomain>。  在此情况下，只能保证运行受约束的执行区域 (CER) 中的退出代码。

### <a name="use-safehandle-to-avoid-resource-leaks"></a>使用 SafeHandle 避免资源泄漏

在 <xref:System.AppDomain> 卸载的情况下，无法依赖正在被执行的 `finally` 块或终结器，因此，通过 <xref:System.Runtime.InteropServices.SafeHandle> 类（而非 <xref:System.IntPtr>、<xref:System.Runtime.InteropServices.HandleRef> 或相似的类）抽象所有操作系统资源访问是很重要的。 这样使 CLR 能够跟踪和关闭你使用的句柄，即使在 <xref:System.AppDomain> 关闭的情况下也不例外。  <xref:System.Runtime.InteropServices.SafeHandle> 将使用 CLR 将始终运行的一个关键终结器。

操作系统句柄从被创建开始到被释放之前都存储在安全句柄中。  在此期间不会出现发生 <xref:System.Threading.ThreadAbortException> 以泄露句柄的情况。  此外，平台调用将引用计数句柄，这样可以关闭对句柄生存期的跟踪，防止 `Dispose` 和正在使用句柄的方法之间出现争用条件的安全问题。

目前具有终结器以简单清理操作系统句柄的大多数类将不再需要终结器了。 相反，终结器将位于 <xref:System.Runtime.InteropServices.SafeHandle> 派生类上。

请注意，<xref:System.Runtime.InteropServices.SafeHandle> 不能取代 <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType>。  显示释放操作系统资源仍然有潜在的资源争用和性能优势。  但要知道显示释放资源的 `finally` 块可能不会执行到完成。

<xref:System.Runtime.InteropServices.SafeHandle> 使你能够实现自己的 <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> 方法，此方法可执行工作以释放句柄，例如将状态传递到操作系统句柄释放例程或释放循环中的句柄集。  CLR 会保证此方法的运行。  在任何情况下，实现 <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> 以确保句柄被释放是创建者的责任。 如果不能做到这点将导致句柄泄露，这通常会导致与句柄相关的本机资源泄露。 因此，构建 <xref:System.Runtime.InteropServices.SafeHandle> 派生类是至关重要的，如此一来，<xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> 实现便不需要任何在调用时可能不可用的资源分配。 请注意，调用在实现 <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> 中可能失败的方法是允许的，只要你的代码可以处理此类失败并且完成协议以释放本机句柄即可。 出于调试目的，<xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> 具有一个 <xref:System.Boolean> 返回值，如果遇到阻止资源释放的灾难性错误，此值可能会被设置为 `false`。 这样做将激活 [releaseHandleFailed](../debug-trace-profile/releasehandlefailed-mda.md) MDA（如果已启用）以辅助确定问题。 它不会以其他任何方式影响运行时；<xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> 将不会为同一资源再次被调用，并且因此将导致句柄泄露。

<xref:System.Runtime.InteropServices.SafeHandle> 在某些上下文中是不合适的。  由于可以在 <xref:System.GC> 终结器线程上运行 <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> 方法，因此，不应将任何需要在特定线程上释放的句柄包装在 <xref:System.Runtime.InteropServices.SafeHandle> 中。

在没有其他代码的情况下，运行时可调用包装器 (RCW) 可以由 CLR 清理。  对于使用平台调用并将 COM 对象视为 `IUnknown*` 或 <xref:System.IntPtr> 的代码，应重新编写此代码才能使用 RCW。  由于非托管的释放方法可能回调到托管代码中，因此 <xref:System.Runtime.InteropServices.SafeHandle> 可能不适用于此方案。

#### <a name="code-analysis-rule"></a>代码分析规则

使用 <xref:System.Runtime.InteropServices.SafeHandle> 以封装操作系统资源。 不要使用 <xref:System.Runtime.InteropServices.HandleRef> 或 <xref:System.IntPtr> 类型的字段。

### <a name="ensure-finalizers-do-not-have-to-run-to-prevent-leaking-operating-system-resources"></a>确保不需要运行终结器来防止操作系统资源泄露

仔细检查终结器以确保即使它们不运行，关键的操作系统资源也不会泄露。  与应用程序正以稳定状态执行或服务器（如 SQL Server）关闭时的普通 <xref:System.AppDomain> 卸载不同，在突然发生的 <xref:System.AppDomain> 卸载期间，对象不会被终结。  请确保出现突然发生的卸载时资源不会泄露，因为虽然无法保证应用程序的正确性，但是要维持服务器的完整性必须保证资源不被泄露。  使用 <xref:System.Runtime.InteropServices.SafeHandle> 以释放任何操作系统资源。

### <a name="ensure-that-finally-clauses-do-not-have-to-run-to-prevent-leaking-operating-system-resources"></a>确保不需要运行 finally 子句来防止操作系统资源泄露

由于不保证 `finally` 子句可以在 CER 外部运行，因此要求库开发人员不要依赖 `finally` 块中的代码来释放非托管资源。  建议的解决方案是使用 <xref:System.Runtime.InteropServices.SafeHandle>。

#### <a name="code-analysis-rule"></a>代码分析规则

使用 <xref:System.Runtime.InteropServices.SafeHandle> 清理操作系统资源，而不是使用 `Finalize`。 不要使用 <xref:System.IntPtr>；使用 <xref:System.Runtime.InteropServices.SafeHandle> 以封装资源。 如果 finally 子句必须运行，请将其放在 CER 中。

### <a name="all-locks-should-go-through-existing-managed-locking-code"></a>所有锁都应通过现有托管锁定代码

CLR 必须知道代码何时在锁中，这样它便知道关闭 <xref:System.AppDomain>，而不仅仅是中止线程。  中止线程可能是危险的操作，因为可能会使由线程运行的数据处于不一致的状态。 因此，必须回收整个 <xref:System.AppDomain>。  未能成功识别锁可能导致死锁或者出现不正确的结果。 使用方法 <xref:System.Threading.Thread.BeginCriticalRegion%2A> 和 <xref:System.Threading.Thread.EndCriticalRegion%2A> 以识别锁定区域。  它们是仅应用于当前线程的 <xref:System.Threading.Thread> 类上的静态方法，帮助防止某个线程编辑另一线程的锁计数。

<xref:System.Threading.Monitor.Enter%2A> 和 <xref:System.Threading.Monitor.Exit%2A> 内置有此 CLR 通知，所以建议使用它们，并且也建议采用使用这些方法的 [lock 语句](../../csharp/language-reference/keywords/lock-statement.md)。

其他锁定机制（如旋转锁和 <xref:System.Threading.AutoResetEvent>）必须调用这些方法才能对 CLR 发出正在进入临界区的通知。  这些方法不采用任何锁；它们通知 CLR 代码正在临界区中执行，中止线程可能导致共享状态不一致。  如果定义了自己的锁类型（如自定义 <xref:System.Threading.ReaderWriterLock> 类），那么则使用这些锁计数方法。

#### <a name="code-analysis-rule"></a>代码分析规则

使用 <xref:System.Threading.Thread.BeginCriticalRegion%2A> 和 <xref:System.Threading.Thread.EndCriticalRegion%2A> 标记并识别所有锁。 不要在循环中使用 <xref:System.Threading.Interlocked.CompareExchange%2A>、<xref:System.Threading.Interlocked.Increment%2A> 和 <xref:System.Threading.Interlocked.Decrement%2A>。  不要对这些方法的 Win32 变量执行平台调用。  不要在循环中使用 <xref:System.Threading.Thread.Sleep%2A>。  不要使用可变字段。

### <a name="cleanup-code-must-be-in-a-finally-or-a-catch-block-not-following-a-catch"></a>清理代码必须位于 finally 或 catch 块中，但不在 catch

清理代码不能位于 `catch` 块之后；它应该在 `finally` 或 `catch` 块中。 这应该是普通的良好做法。 通常首选使用 `finally` 块，因为在引发异常时以及在一般情况下达到 `try` 块的末尾时它都运行相同的代码。  在引发意外异常的情况下（例如 <xref:System.Threading.ThreadAbortException>），清理代码将不会运行。  理想情况下，要在 `finally` 中清理的任何非托管资源应该包装在 <xref:System.Runtime.InteropServices.SafeHandle> 中以防止泄露。  请注意，可以有效地使用 C# `using` 键释放对象，包括句柄。

虽然 <xref:System.AppDomain> 回收可以清理终结器线程上的资源，但是将清理代码放在正确的位置中仍然是很重要的。 请注意，如果线程在未持有锁的情况下接收异步异常，那么 CLR 将尝试自己终止线程，而无需回收 <xref:System.AppDomain>。  通过提供更多可用的资源以及进行更好的生命期管理，有助于确保尽早地清理资源。 如果不显示关闭指向某些错误代码路径中的文件的句柄，并且等待 <xref:System.Runtime.InteropServices.SafeHandle> 终结器进行清理，那么下一次你的代码运行时，如果终结器尚未运行，代码尝试访问同一文件可能会失败。  因此，确保清理代码存在并能够正常工作将有助于更彻底、更快速地从失败中恢复，即使这并非绝对必要的操作。

#### <a name="code-analysis-rule"></a>代码分析规则

`catch` 后面的清理代码需要在 `finally` 块中。 将要进行释放的调用放置在 finally 块中。 `catch` 块应以引发或再次引发结束。 虽然将出现异常，例如代码检测是否可以建立网络连接，在此情况下可能会出现大量的异常，但是，正常情况下，任何需要捕获大量异常的代码应该指示应对代码进行测试以检查它是否将成功。

### <a name="process-wide-mutable-shared-state-between-application-domains-should-be-eliminated-or-use-a-constrained-execution-region"></a>应该消除应用程序域之间进程范围的可变共享状态，或使用受约束的执行区域

如简介中所述，以可靠方式编写监视应用程序域之间进程范围的共享状态的托管代码是非常困难的。  进程范围的共享状态是在应用程序域之间共享的任何种类的数据结构，可以在 Win32 代码中、CLR 内部或者在使用远程处理的托管代码中。  在托管代码中正确地编写可变的共享状态是非常困难的，并且处理任何静态的共享状态时必须极其小心。  如果有进程范围或计算机范围的共享状态，请找到方法消除它或使用受约束的执行区域 (CER) 保护此共享状态。  请注意，具有任何未识别并未更正的共享状态的库可能导致需要清理 <xref:System.AppDomain> 卸载的主机（如 SQL Server） 崩溃。

如果代码使用 COM 对象，请避免在应用程序域之间共享此 COM 对象。

### <a name="locks-do-not-work-process-wide-or-between-application-domains"></a>锁定在进程范围内或应用程序域之间不起作用。

过去，<xref:System.Threading.Monitor.Enter%2A> 和 [lock 语句](../../csharp/language-reference/keywords/lock-statement.md)用于创建全局进程锁定。  例如，在对 <xref:System.AppDomain> 敏捷类进行锁定时会发生这种情况，例如来自非共享程序集的 <xref:System.Type> 实例、<xref:System.Threading.Thread> 对象、暂存的字符串以及某些使用远程处理跨应用程序域共享的字符串。  这些锁不再属于进程范围。  若要识别进程范围的应用程序间域锁的存在，请确定锁中的代码是否使用任何外部持久化资源，如磁盘上的文件或者可能使用一个数据库。

请注意，如果受保护的代码使用了外部资源，那么在 <xref:System.AppDomain> 中采用锁可能导致出现问题，因为该代码可能同时跨多个应用程序域运行。  在写入到一个日志文件或绑定到整个进程的套接字时，这可能是个问题。  这些更改意味着除了使用命名的 <xref:System.Threading.Mutex> 或 <xref:System.Threading.Semaphore> 实例外，没有简单的方法使用托管代码来获取全局进程的锁。  创建不在两个应用程序域中同时运行的代码，或使用 <xref:System.Threading.Mutex> 或 <xref:System.Threading.Semaphore> 类。  如果无法更改现有代码，请不要使用 Win32 命名的互斥以实现此同步，因为在纤程模式中运行意味着无法保证同一操作系统线程将获取并释放互斥。  必须使用托管 <xref:System.Threading.Mutex> 类、命名的 <xref:System.Threading.ManualResetEvent>、<xref:System.Threading.AutoResetEvent>，或 <xref:System.Threading.Semaphore>，以 CLR 能识别的方式同步代码锁，而不是使用非托管代码进行同步。

#### <a name="avoid-locktypeofmytype"></a>避免 lock(typeof(MyType))

仅具有一个跨所有应用程序域共享的代码副本的共享程序集中的私有和公用 <xref:System.Type> 对象也存在问题。  对于共享程序集，每个进程只有一个 <xref:System.Type> 实例，意味着多个应用程序域共享同一 <xref:System.Type> 实例。  在 <xref:System.Type> 实例上采用锁是采用影响整个进程而不仅仅是影响 <xref:System.AppDomain> 的锁。  如果一个 <xref:System.AppDomain> 在 <xref:System.Type> 对象上采用锁，接着该线程突然被中止，那么它将不会释放锁。  此锁随后可能导致其他应用程序域出现死锁的情况。

以静态方法采用锁的一个好方法是将静态的内部同步对象添加到代码中。  可以在类构造函数（如果存在）中对其进行初始化；但是，如果不存在，则可以按以下方式对其进行初始化：

```csharp
private static Object s_InternalSyncObject;
private static Object InternalSyncObject
{
    get
    {
        if (s_InternalSyncObject == null)
        {
            Object o = new Object();
            Interlocked.CompareExchange(
                ref s_InternalSyncObject, o, null);
        }
        return s_InternalSyncObject;
    }
}
```

然后在采用锁时，使用 `InternalSyncObject` 属性获取要在其上进行锁定的对象。  如果已在类构造函数中初始化了内部同步对象，则不需要使用此属性。  仔细检查锁初始化代码的方法应如下例所示：

```csharp
public static MyClass SingletonProperty
{
    get
    {
        if (s_SingletonProperty == null)
        {
            lock(InternalSyncObject)
            {
                // Do not use lock(typeof(MyClass))
                if (s_SingletonProperty == null)
                {
                    MyClass tmp = new MyClass(…);
                    // Do all initialization before publishing
                    s_SingletonProperty = tmp;
                }
            }
        }
        return s_SingletonProperty;
    }
}
```

#### <a name="a-note-about-lockthis"></a>有关锁的说明（this）

通常，在可公开访问的单个对象上采用锁是可以接受的。  但是，如果对象是可能导致整个子系统出现死锁的单一对象，也可以考虑使用以上设计模式。  例如，<xref:System.Security.SecurityManager> 对象上的锁可能导致 <xref:System.AppDomain> 内出现死锁，从而使整个 <xref:System.AppDomain> 不可用。 好的做法是不在此类可公开访问的对象上采用锁。  但是，单个集合或数组上的锁通常不存在此问题。

#### <a name="code-analysis-rule"></a>代码分析规则

不要在可跨应用程序域使用或不能明确进行标识的类型上采用锁。 不要在 <xref:System.Type>、<xref:System.Reflection.MethodInfo>、<xref:System.Reflection.PropertyInfo>、<xref:System.String>、<xref:System.ValueType>、<xref:System.Threading.Thread> 或任何派生自 <xref:System.MarshalByRefObject> 的对象上调用 <xref:System.Threading.Monitor.Enter%2A>。

### <a name="remove-gckeepalive-calls"></a>删除 GC。KeepAlive 调用

大量现有代码在应该使用 <xref:System.GC.KeepAlive%2A> 时不使用或在不适合的时候使用它。  在转换成 <xref:System.Runtime.InteropServices.SafeHandle> 后，类不需要调用 <xref:System.GC.KeepAlive%2A>，即假设它们没有终结器但是依赖于 <xref:System.Runtime.InteropServices.SafeHandle> 以终结操作系统句柄。  虽然保留对 <xref:System.GC.KeepAlive%2A> 调用的性能成本可以忽略不计，但是调用 <xref:System.GC.KeepAlive%2A> 是必需或足以解决可能不再存在的生存期问题的方法，这样使代码更难以维护。  但是，当使用 COM 互操作 CLR 可调用包装器 (RCW) 时，代码还是需要 <xref:System.GC.KeepAlive%2A>。

#### <a name="code-analysis-rule"></a>代码分析规则

删除 <xref:System.GC.KeepAlive%2A>。

### <a name="use-the-hostprotection-attribute"></a>使用 HostProtection 特性

<xref:System.Security.Permissions.HostProtectionAttribute> (HPA) 允许使用声明性安全操作来决定主机保护需求，使主机甚至能够阻止完全信任的代码调用不适用于给定主机的某些方法，例如针对 SQL Server 的 <xref:System.Environment.Exit%2A> 或 <xref:System.Windows.Forms.MessageBox.Show%2A>。

HPA 仅影响可托管公共语言运行时且实现主机保护的非托管应用程序，如 SQL Server。 应用后，安全性操作会导致基于类或方法公开的主机资源而创建链接要求。 如果代码在客户端应用程序中运行或在非主机保护的服务器上运行，此特性则会“蒸发”；由于它不会被检测到，因此不会被应用。

> [!IMPORTANT]
> 此特性的目的在于强制执行特定于主机的编程模型准则，而非安全行为。  虽然链接要求是用于检查是否符合编程模型要求，但 <xref:System.Security.Permissions.HostProtectionAttribute> 不是安全权限。

如果此主机没有编程模型要求，则不会出现链接要求。

此特性可以标识以下各项：

- 不适合主机编程模型，但属于良性的方法或类。

- 不适合主机编程模型且可能导致破坏服务器托管的用户代码稳定性的方法或类。

- 不适合主机编程模型且可能导致破坏服务器进程自身稳定性的方法或类。

> [!NOTE]
> 如果要创建由可能在主机保护的环境中执行的应用程序调用的类库，则应该将此特性应用于公开 <xref:System.Security.Permissions.HostProtectionResource> 资源类别的成员。 具有此特性的 .NET Framework 类库成员会导致仅对直接调用方进行检查。  你的库成员也必须使用同一方法对其直接调用方进行检查。

有关 HPA 的详细信息，请参阅 <xref:System.Security.Permissions.HostProtectionAttribute>。

#### <a name="code-analysis-rule"></a>代码分析规则

对于 SQL Server，所有用于引入同步或线程的方法必须使用 HPA 识别。 这包括共享状态、被同步或管理外部进程的方法。 影响 SQL Server 的 <xref:System.Security.Permissions.HostProtectionResource> 值是 <xref:System.Security.Permissions.HostProtectionResource.SharedState>、<xref:System.Security.Permissions.HostProtectionResource.Synchronization> 和 <xref:System.Security.Permissions.HostProtectionResource.ExternalProcessMgmt>。 但是，任何公开任意 <xref:System.Security.Permissions.HostProtectionResource> 的方法都应由 HPA 识别，而不只是使用影响 SQL 的资源的那些方法。

### <a name="do-not-block-indefinitely-in-unmanaged-code"></a>不要在非托管代码中无限期阻止

在非托管代码中而不是在托管代码中阻塞可能导致拒绝服务攻击，因为 CLR 无法中止线程。  已阻塞的线程会阻止 CLR 卸载 <xref:System.AppDomain>，至少是在没有执行某些极端不安全操作的情况下。  使用 Windows 同步基元进行阻止是我们无法允许的一个清晰的示例。  如果可能，应尽可能避免在对套接字的调用中进行阻止 `ReadFile` ，理想情况下，WINDOWS API 应为此类操作提供一种机制来超时。

理想情况下，任何调入本机的方法应使用具有合理、有限的超时的 Win32 调用。  如果允许用户指定超时，则在没有某些特定安全权限的情况下，不应该允许用户指定无限期的超时。  按照一般准则，如果方法将阻塞超过 10 秒，你则需要使用支持超时的版本，或需要其他的 CLR 支持。

下面是一些有问题的 Api 的示例。  虽然在超时的情况下可以创建管道（匿名和命名管道皆可）；但是，代码必须确保它永不使用 NMPWAIT_WAIT_FOREVER 调用 `CreateNamedPipe` 或 `WaitNamedPipe`。  此外，即使指定了超时也可能出现意外的阻塞。  在匿名管道上调用 `WriteFile` 将会在全部字节被写入之前阻塞，这意味着如果缓冲区在其中有未读数据，那么在读取器释放管道缓冲区中的空间之前，`WriteFile` 调用将阻塞。  套接字应该始终使用一些提供超时机制的 API。

#### <a name="code-analysis-rule"></a>代码分析规则

在非托管代码中在没有超时的情况下进行阻塞是拒绝服务攻击。 不要执行对 `WaitForSingleObject`、`WaitForSingleObjectEx`、`WaitForMultipleObjects`、`MsgWaitForMultipleObjects` 和 `MsgWaitForMultipleObjectsEx` 的平台调用。  不要使用 NMPWAIT_WAIT_FOREVER。

### <a name="identify-any-sta-dependent-features"></a>确定任何依赖于 STA 的功能

识别任何使用 COM 单线程单元 (STA) 的代码。  STA 在 SQL Server 进程中是被禁用的。  必须在 SQL Server 中禁用依赖 `CoInitialize` 的功能，如性能计数器或剪贴板。

### <a name="ensure-finalizers-are-free-of-synchronization-problems"></a>确保终结器无同步问题

多个终结器线程可能在 .NET Framework 的未来版本中存在，这意味着针对同一类型的不同实例的终结器会同时运行。  它们不需要是完全线程安全的；垃圾回收器保证只有一个线程将针对给定的对象实例运行终结器。  但是，必须对终结器进行编码以避免同时在多个不同的对象示例上运行时出现争用条件和死锁的情况。  当在终结器中使用任何外部状态（如写入日志文件）时，必须解决线程问题。  不要依赖终结来提供线程安全性。 不要使用线程本地存储（托管的或本机的）在终结器线程上存储状态。

#### <a name="code-analysis-rule"></a>代码分析规则

终结器不得存在同步问题。 不要在终结器中使用静态的可变状态。

### <a name="avoid-unmanaged-memory-if-possible"></a>如果可能，请避免非托管内存

非托管内存可能会被泄露，正如操作系统句柄一样。 如有可能，请使用 [stackalloc](../../csharp/language-reference/operators/stackalloc.md) 或固定的托管对象（如 [fixed 语句](../../csharp/language-reference/keywords/fixed-statement.md)或使用 byte[] 的 <xref:System.Runtime.InteropServices.GCHandle>）在堆栈上尝试使用内存。 <xref:System.GC> 最终会清理这些内容。 但是，如果必须要分配非托管内存，请考虑使用派生自 <xref:System.Runtime.InteropServices.SafeHandle> 的类以包装内存分配。

请注意，至少存在一种 <xref:System.Runtime.InteropServices.SafeHandle> 不适用的情况。 对于分配或释放内存的 COM 方法调用，通常是一个 DLL 通过 `CoTaskMemAlloc` 分配内存，然后另一个 DLL 使用 `CoTaskMemFree` 释放内存。  在这些位置中使用 <xref:System.Runtime.InteropServices.SafeHandle> 可能不适合，因为它会尝试将非托管内存的生存期绑定到 <xref:System.Runtime.InteropServices.SafeHandle> 的生存期，而不是允许其他 DLL 控制内存的生存期。

### <a name="review-all-uses-of-catchexception"></a>查看 catch 的所有使用情况（异常）

捕获所有异常而不是捕获某个特定异常的 catch 块现在也将捕获异步异常。 检查每个 catch(Exception) 块，以确认没有重要的资源释放、可能被跳过的退出代码以及用于处理 <xref:System.Threading.ThreadAbortException>、<xref:System.StackOverflowException> 或 <xref:System.OutOfMemoryException> 的 catch 块自身中潜在的不正确行为。  请注意，此代码可能会记录或作出它只能发现特定异常的假设，或者假设无论异常何时发生，它的失败都是由同一特定原因所导致的。  可能需要对这些假设进行更新以将 <xref:System.Threading.ThreadAbortException> 包括在内。

请考虑更改捕获所有异常的所有位置以捕获期待将引发的特定类型的异常，例如来自字符串格式化方法的 <xref:System.FormatException>。  这将阻止 catch 块针对意外异常运行，并且将帮助确保代码不会通过捕获意外异常来隐藏 bug。  按照一般规则，绝不要在库代码中处理异常（要求你捕获异常的代码可能指示正在调用的代码中存在设计缺陷）。  在某些情况下，你可能想捕获异常并且引发不同的异常类型以提供更多的数据。  在此情况下则使用嵌套异常，以将失败的真实原因存储在新异常的 <xref:System.Exception.InnerException%2A> 属性中。

#### <a name="code-analysis-rule"></a>代码分析规则

查看托管代码中捕获所有对象或捕获所有异常的所有 catch 块。  在 c # 中，这意味着标记 `catch` {} 和 `catch(Exception)` {} 。  请考虑将异常类型描述得非常具体，或者查看代码以确保在它捕获到意外异常类型时不会以错误的方式执行。

### <a name="do-not-assume-a-managed-thread-is-a-win32-thread--it-is-a-fiber"></a>不要假设托管线程是一个 Win32 线程–它是一个纤程

虽然使用托管线程本地存储的确有效，但你不能再次使用非托管线程本地存储或再次假设代码将在当前操作系统线程上运行。 不要更改如线程的区域设置等设置。 不要通过平台调用对 `InitializeCriticalSection` 或 `CreateMutex` 进行调用，因为它们要求进入锁的操作系统线程也能退出锁。 由于使用纤程时将不存在此问题，所以不能直接在 SQL 中使用 Win32 临界区和互斥。  请注意，托管 <xref:System.Threading.Mutex> 类不会处理这些线程关联问题。

可以在托管 <xref:System.Threading.Thread> 对象上安全使用大部分状态，包括托管线程本地存储和线程当前的 UI 区域性。 还可以使用 <xref:System.ThreadStaticAttribute>，这将使现有静态变量的值只能由当前托管线程访问（这是在 CLR 中执行纤程本地存储的另一种方法）。 出于编程模型的原因，在 SQL 中运行时不能更改线程当前的区域性。

#### <a name="code-analysis-rule"></a>代码分析规则

SQL Server 在纤程模式中运行；不要使用线程本地存储。 请避免执行对 `TlsAlloc`、`TlsFree`、`TlsGetValue` 和 `TlsSetValue.` 的平台调用

### <a name="let-sql-server-handle-impersonation"></a>让 SQL Server 处理模拟

由于模拟在线程级别上进行操作且 SQL 可以在纤程模式中运行，因此托管代码不应该模拟用户，并且不应该调用 `RevertToSelf`。

#### <a name="code-analysis-rule"></a>代码分析规则

让 SQL Server 处理模拟。 不要使用 `RevertToSelf`、`ImpersonateAnonymousToken`、`DdeImpersonateClient`、`ImpersonateDdeClientWindow`、`ImpersonateLoggedOnUser`、`ImpersonateNamedPipeClient`、`ImpersonateSelf`、`RpcImpersonateClient`、`RpcRevertToSelf`、`RpcRevertToSelfEx` 或 `SetThreadToken`。

### <a name="do-not-call-threadsuspend"></a>不调用 Thread：：挂起

挂起线程的功能可能在一个简单的操作中实现，但是它可能会导致死锁。  如果持有锁的线程由另一个线程挂起，后者又尝试采用相同的锁，则会发生死锁。  <xref:System.Threading.Thread.Suspend%2A> 当前会对安全性、类加载、远程处理和反射造成干扰。

#### <a name="code-analysis-rule"></a>代码分析规则

不要调用 <xref:System.Threading.Thread.Suspend%2A>。 请考虑改为使用真正的同步基元，如 <xref:System.Threading.Semaphore> 或 <xref:System.Threading.ManualResetEvent>。

### <a name="protect-critical-operations-with-constrained-execution-regions-and-reliability-contracts"></a>通过受约束的执行区域和可靠性协定保护关键操作

在执行更新共享状态或需要确定性的完全成功或完全失败的复杂操作时，请确保此操作受到受约束的执行区域 (CER) 的保护。 这将保证代码在所有情况下都可运行，甚至在线程突然中止或 <xref:System.AppDomain> 突然卸载的情况下也不例外。

一个 CER 是 `try/finally` 调用后面紧跟的一个特定 <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A> 块。

这样做将指示实时编译器首先在 finally 块中准备所有代码，然后才运行 `try` 块。 这保证代码会在 finally 块中生成并且将在所有情况下运行。 CER 中具有空 `try` 块的情况并不罕见。 使用 CER 防止出现异步线程中止和内存不足异常。 有关进一步为极深代码处理堆栈溢出的 CER 形式，请参阅 <xref:System.Runtime.CompilerServices.RuntimeHelpers.ExecuteCodeWithGuaranteedCleanup%2A>。

## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.ConstrainedExecution>
- [SQL Server 编程和宿主保护特性](sql-server-programming-and-host-protection-attributes.md)
