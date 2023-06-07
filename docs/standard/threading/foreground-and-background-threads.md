---
title: 前台和后台线程
description: 使用 .NET 中的 Thread.IsBackground 属性确定或更改线程是后台线程还是前台线程。
ms.date: 03/30/2017
helpviewer_keywords:
- threading [.NET], foreground
- threading [.NET], background
- foreground threads
- background threads
ms.assetid: cfe0d632-dd35-47e0-91ad-f742a444005e
ms.openlocfilehash: 9f0ea1d53eb2f96b8a56cacc089cf90eb2f079a0
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94819898"
---
# <a name="foreground-and-background-threads"></a>前台线程和后台线程

托管线程可以是后台线程，也可以是前台线程。 后台线程和前台线程几乎完全相同，只有一处不同，即后台线程不会确保托管执行环境一直运行。 一旦托管进程（其中 .exe 文件为托管程序集）中的所有前台线程都停止，系统会停止并关闭所有后台线程。  
  
> [!NOTE]
> 如果运行时因进程正在关闭而停止后台线程，不会在线程中抛出任何异常。 不过，如果线程因 <xref:System.AppDomain.Unload%2A?displayProperty=nameWithType> 方法卸载应用域而停止，前台线程和后台线程中都会抛出 <xref:System.Threading.ThreadAbortException>。  
  
 <xref:System.Threading.Thread.IsBackground%2A?displayProperty=nameWithType> 属性可用于确定是后台线程还是前台进程，也可用于更改线程状态。 可以随时将线程的 <xref:System.Threading.Thread.IsBackground%2A> 属性更改为 `true`，将线程更改为后台线程。  
  
> [!IMPORTANT]
> 线程的前台或后台状态不会影响线程中抛出未经处理的异常。 前台线程或后台线程中未经处理的异常会导致应用程序终止。 请参阅[托管线程异常](exceptions-in-managed-threads.md)。  
  
 属于托管线程池的线程（即 <xref:System.Threading.Thread.IsThreadPoolThread%2A> 属性为 `true` 的线程）是后台线程。 从非托管代码进入托管执行环境的所有线程都会标记为后台线程。 默认情况下，通过新建并启动 <xref:System.Threading.Thread> 对象生成的所有线程都是前台线程。  
  
 如果使用线程监视活动（如套接字连接），请将它的 <xref:System.Threading.Thread.IsBackground%2A> 属性设置为 `true`，以便线程不会阻止进程终止。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Threading.Thread.IsBackground%2A?displayProperty=nameWithType>
- <xref:System.Threading.Thread>
- <xref:System.Threading.ThreadAbortException>
