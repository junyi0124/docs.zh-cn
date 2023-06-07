---
title: asynchronousThreadAbort MDA
description: 查看在线程尝试将异步中止放入另一个线程时，如何激活 asynchronousThreadAbort 托管调试助手 (MDA) 。
ms.date: 03/30/2017
helpviewer_keywords:
- asynchronous thread aborts
- AsynchronousThreadAbort MDA
- managed debugging assistants (MDAs), asynchronous thread aborts
- threading [.NET Framework], managed debugging assistants
- MDAs (managed debugging assistants), asynchronous thread aborts
ms.assetid: 9ebe40b2-d703-421e-8660-984acc42bfe0
ms.openlocfilehash: 216c4bbe570fe34b59513bb338f0f2c5da0fc3e2
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96265240"
---
# <a name="asynchronousthreadabort-mda"></a>asynchronousThreadAbort MDA

当线程尝试将异步中止引入到另一个线程时，将激活 `asynchronousThreadAbort` 托管调试助手 (MDA)。 同步线程中止不会激活 `asynchronousThreadAbort` MDA。

## <a name="symptoms"></a>症状

 当主应用程序线程被中止时，应用程序会崩溃，并出现未处理的 <xref:System.Threading.ThreadAbortException>。 如果继续执行应用程序，可能会引起比应用程序崩溃更糟糕的后果，可能导致进一步的数据损坏。

 应为原子操作的操作可能在部分完成后被中断，使应用程序数据处于不可预测的状态。 在代码执行中，<xref:System.Threading.ThreadAbortException> 可能从看似随机的点生成，通常在不应该出现此异常的位置生成。 代码可能无法处理此类异常，从而导致损坏状态。

 由于问题固有的随机性，因此问题的具体表现可能有极大差异。

## <a name="cause"></a>原因

 一个线程中的代码对目标线程调用 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> 方法来引入异步线程中止。 线程中止是异步的，因为对 <xref:System.Threading.Thread.Abort%2A> 进行调用的代码运行在与中止操作的目标不同的线程上。 同步线程中止不应导致此问题，因为执行 <xref:System.Threading.Thread.Abort%2A> 的线程应只在应用程序状态一致的安全检查点执行此操作。

 异步线程中止出现问题，因为它们在目标线程的执行过程中是在不可预测的点被处理的。 若要避免此问题，所编写的代码（为了在可能以此方式被中止的线程上运行）需要在几乎每个代码行上处理 <xref:System.Threading.ThreadAbortException>，负责将应用程序数据放回到一致状态。 在编写代码时考虑到此问题，或编写防止可能出现的所有情况的代码都是不现实的。

 尽管对非托管代码和 `finally` 块的调用将不会被异步中止，但在从任一这些类别中退出时将被立即中止。

 由于问题固有的随机性，因此可能很难确定问题的原因。

## <a name="resolution"></a>解决方法

 避免需要使用异步线程中止的代码设计。 有几种更适合用于中断目标线程的方法，这些方法无需调用 <xref:System.Threading.Thread.Abort%2A>。 最安全的方法是引入一种机制，如向目标线程发出信号以请求中断的一个公用属性。 此目标线程会在某些安全检查点检查信号。 如果它发现已请求中断，则可以正常关闭。

## <a name="effect-on-the-runtime"></a>对运行时的影响

 此 MDA 对 CLR 无任何影响。 它只报告有关异步线程中止的数据。

## <a name="output"></a>输出

 MDA 报告执行中止的线程 ID 以及作为此中止目标的线程 ID。 这两项绝对不会相同，因为这被限制为异步中止。

## <a name="configuration"></a>Configuration

```xml
<mdaConfig>
  <assistants>
    <asynchronousThreadAbort />
  </assistants>
</mdaConfig>
```

## <a name="example"></a>示例

 激活 `asynchronousThreadAbort` MDA 仅需要在单独运行的线程上调用 <xref:System.Threading.Thread.Abort%2A>。 请考虑在线程启动功能的内容是一组更复杂操作（这些操作可能被中止在任意点处中断）的情况下的后果。

```csharp
using System.Threading;
void FireMda()
{
    Thread t = new Thread(delegate() { Thread.Sleep(1000); });
    t.Start();
    // The following line activates the MDA.
    t.Abort();
    t.Join();
}
```

## <a name="see-also"></a>另请参阅

- <xref:System.Threading.Thread>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
