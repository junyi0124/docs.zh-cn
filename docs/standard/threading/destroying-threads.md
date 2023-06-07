---
title: 销毁线程
description: 了解需销毁 .NET 中的线程时的可选方法，例如协作取消或 Thread.Abort 方法。 了解如何处理 ThreadAbortException。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- destroying threads
- threading [.NET], destroying threads
ms.assetid: df54e648-c5d1-47c9-bd29-8e4438c1db6d
ms.openlocfilehash: bdba09f5709cf99bc0d076e3875a914cc7c5a11e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95723761"
---
# <a name="destroying-threads"></a>销毁线程

若要终止线程的执行，通常使用[协作取消模型](cancellation-in-managed-threads.md)。 有时无法以协作方式停止线程，因为它运行的第三方代码不是为协作取消而设计的。 .NET Framework 中的 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> 方法可用于强制终止托管线程。 调用 <xref:System.Threading.Thread.Abort%2A> 时，公共语言运行时在目标线程中引发目标线程可以捕获的 <xref:System.Threading.ThreadAbortException>。 有关详细信息，请参阅 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>。 .NET 5（包括 .NET Core）及更高版本不支持 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> 方法。 如果需要在 .NET 5+ 中强制终止第三方代码的执行，请在单独的进程中运行该代码，并使用 <xref:System.Diagnostics.Process.Kill%2A?displayProperty=nameWithType>。

> [!NOTE]
> 如果线程在调用 <xref:System.Threading.Thread.Abort%2A> 方法时执行的是非托管代码，运行时将它标记为 <xref:System.Threading.ThreadState.AbortRequested?displayProperty=nameWithType>。 当线程返回到托管代码时，异常就会抛出。  
  
 一旦线程中止，就无法再重启。  
  
 <xref:System.Threading.Thread.Abort%2A> 方法不会导致线程立即中止，因为目标线程可以捕获 <xref:System.Threading.ThreadAbortException>，并在 `finally` 块中执行任意数量的代码。 如果需要等到线程结束，可以调用 <xref:System.Threading.Thread.Join%2A?displayProperty=nameWithType>。 <xref:System.Threading.Thread.Join%2A?displayProperty=nameWithType> 是阻止调用，除非线程实际已停止执行或可选超时间隔已结束，否则不会返回结果。 由于中止的线程可以调用 <xref:System.Threading.Thread.ResetAbort%2A> 方法或在 `finally` 块中执行无限处理，因此如果不指定超时，就无法保证等到线程结束。  
  
 正在等待调用 <xref:System.Threading.Thread.Join%2A?displayProperty=nameWithType> 方法的线程可能会被调用 <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> 的其他线程中断。  
  
## <a name="handling-threadabortexception"></a>处理 ThreadAbortException  

 如果应中止线程，无论是由于在自己的代码中调用 <xref:System.Threading.Thread.Abort%2A> 所致，还是由于卸载正在运行线程的应用域（<xref:System.AppDomain.Unload%2A?displayProperty=nameWithType> 使用 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> 终止线程）所致，线程都必须处理 <xref:System.Threading.ThreadAbortException>，并在 `finally` 子句中执行任何最终处理，如下面的代码所示。  
  
```vb  
Try  
    ' Code that is executing when the thread is aborted.  
Catch ex As ThreadAbortException  
    ' Clean-up code can go here.  
    ' If there is no Finally clause, ThreadAbortException is  
    ' re-thrown by the system at the end of the Catch clause.
Finally  
    ' Clean-up code can go here.  
End Try  
' Do not put clean-up code here, because the exception
' is rethrown at the end of the Finally clause.  
```  
  
```csharp  
try
{  
    // Code that is executing when the thread is aborted.  
}
catch (ThreadAbortException ex)
{  
    // Clean-up code can go here.  
    // If there is no Finally clause, ThreadAbortException is  
    // re-thrown by the system at the end of the Catch clause.
}  
// Do not put clean-up code here, because the exception
// is rethrown at the end of the Finally clause.  
```  
  
 清理代码必须位于 `catch` 子句或 `finally` 子句中，因为系统会在 `finally` 子句末尾或 `catch` 子句（如果没有 `finally` 子句的话）末尾重新抛出 <xref:System.Threading.ThreadAbortException>。  
  
 可以调用 <xref:System.Threading.Thread.ResetAbort%2A?displayProperty=nameWithType> 方法，以防系统重新抛出异常。 不过，只有在自己的代码导致 <xref:System.Threading.ThreadAbortException> 抛出时，才应这样做。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Threading.ThreadAbortException>
- <xref:System.Threading.Thread>
- [使用线程和线程处理](using-threads-and-threading.md)
