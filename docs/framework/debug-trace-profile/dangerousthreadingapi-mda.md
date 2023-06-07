---
title: dangerousThreadingAPI MDA
description: 查看 dangerousThreadingAPI 托管调试助手 (MDA) ，它在当前线程以外的线程上调用时激活。
ms.date: 03/30/2017
helpviewer_keywords:
- suspending threads
- DangerousThreadingAPI MDA
- managed debugging assistants (MDAs), dangerous threading operations
- threading [.NET Framework], suspending
- MDAs (managed debugging assistants), dangerous threading operations
- Suspend method
- threading [.NET Framework], managed debugging assistants
ms.assetid: 3e5efbc5-92e4-4229-b31f-ce368a1adb96
ms.openlocfilehash: 707e3e339cb8a692f862afc15328eef53f0547e5
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96286079"
---
# <a name="dangerousthreadingapi-mda"></a>dangerousThreadingAPI MDA

如果在当前线程以外的线程上调用 <xref:System.Threading.Thread.Suspend%2A?displayProperty=nameWithType> 方法，将激活 `dangerousThreadingAPI` 托管调试助手 (MDA)。  
  
## <a name="symptoms"></a>症状  

 应用程序无响应或无限期挂起。 系统数据或应用程序数据可能暂时处于不可预知状态，甚至在应用程序关闭之后也可能处于不可预知状态。 某些操作未按预期完成。  
  
 由于问题固有的随机性，因此问题的具体表现可能有极大差异。  
  
## <a name="cause"></a>原因  

 一个线程使用 <xref:System.Threading.Thread.Suspend%2A> 方法将另一线程异步挂起。 无法确定何时挂起另一个可能正在操作的线程才安全。 挂起线程会导致数据损坏或不变体中断。 如果一个线程处于挂起状态且从未使用 <xref:System.Threading.Thread.Resume%2A> 方法恢复，则应用程序可能会无限期挂起且应用程序数据可能会遭到损坏。 这些方法已被标记为已过时。  
  
 如果由目标线程保留同步基元，则挂起期间仍然保留这些同步基元。 如果另一线程（例如执行 <xref:System.Threading.Thread.Suspend%2A> 的线程）尝试获取对该基元的锁定，则这可能会导致死锁。 在此情况下，该问题将自身表示为死锁。  
  
## <a name="resolution"></a>解决方法  

 避免需使用 <xref:System.Threading.Thread.Suspend%2A> 和 <xref:System.Threading.Thread.Resume%2A> 的设计。 对于线程之间的协作，请使用 <xref:System.Threading.Monitor>、<xref:System.Threading.ReaderWriterLock>、<xref:System.Threading.Mutex> 或 C# `lock` 语句等同步基元。 如果必须使用这些方法，则请缩短时间范围并最大限度地减少线程处于挂起状态时执行的代码量。  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 此 MDA 对 CLR 无任何影响。 它只报告有关危险线程处理操作的数据。  
  
## <a name="output"></a>输出  

 MDA 识别导致其激活的危险线程处理方法。  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <dangerousThreadingAPI />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>示例  

 以下代码示例演示对造成 `dangerousThreadingAPI` 激活的 <xref:System.Threading.Thread.Suspend%2A> 方法的调用。  
  
```csharp
using System.Threading;  
void FireMda()  
{  
Thread t = new Thread(delegate() { Thread.Sleep(1000); });  
    t.Start();  
    // The following line activates the MDA.  
    t.Suspend();
    t.Resume();  
    t.Join();  
}  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Threading.Thread>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
- [lock 语句](../../csharp/language-reference/keywords/lock-statement.md)
