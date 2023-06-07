---
title: Mutexes
ms.date: 03/30/2017
helpviewer_keywords:
- wait handles
- threading [.NET], Mutex class
- Mutex class, about Mutex class
- threading [.NET], cross-process synchronization
ms.assetid: 9dd06e25-12c0-4a9e-855a-452dc83803e2
ms.openlocfilehash: aa5a13b5b1cfcd7305df39c1ff5005deb45eb4ed
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95672170"
---
# <a name="mutexes"></a>Mutexes

<xref:System.Threading.Mutex> 对象可用于提供对资源的独占访问权限。 虽然 <xref:System.Threading.Mutex> 类使用的系统资源比 <xref:System.Threading.Monitor> 类更多，但它可以跨应用域边界进行封送，可用于多个等待操作以及同步不同进程中的线程。 有关托管同步机制的比较，请参阅[同步基元概述](overview-of-synchronization-primitives.md)。  
  
 有关代码示例，请参阅 <xref:System.Threading.Mutex.%23ctor%2A> 构造函数的参考文档。  
  
## <a name="using-mutexes"></a>使用 Mutex  

 线程调用 mutex 的 <xref:System.Threading.WaitHandle.WaitOne%2A> 方法来请求获取所有权。 在 mutex 可用或可选超时间隔结束前，该调用会一直处于阻止状态。 如果没有任何线程拥有它，则 mutex 将处于已发出信号状态。  
  
 线程通过调用 <xref:System.Threading.Mutex.ReleaseMutex%2A> 方法来释放 mutex。 Mutex 具有线程关联；即 mutex 只能由拥有它的线程释放。 如果线程释放的 mutex 不是它拥有的，就会在线程中抛出 <xref:System.ApplicationException>。  
  
 由于 <xref:System.Threading.Mutex> 类派生自 <xref:System.Threading.WaitHandle>，因此还可以调用 <xref:System.Threading.WaitHandle> 的静态 <xref:System.Threading.WaitHandle.WaitAll%2A> 或 <xref:System.Threading.WaitHandle.WaitAny%2A> 方法，以请求获取 <xref:System.Threading.Mutex> 的所有权以及其他等待句柄。  
  
 如果线程拥有 <xref:System.Threading.Mutex>，此线程可以在重复的等待-请求调用中指定相同的 <xref:System.Threading.Mutex>，而不会阻止执行；不过，它必须释放 <xref:System.Threading.Mutex>，次数与释放所有权一样多。  
  
## <a name="abandoned-mutexes"></a>放弃的 mutex  

 如果线程终止而未释放 <xref:System.Threading.Mutex>，则认为已放弃 mutex。 这通常指示存在严重的编程错误，因为该 mutex 正在保护的资源可能会处于不一致状态。 在下一个获取 mutex 的线程中引发 <xref:System.Threading.AbandonedMutexException>。
  
 对于系统范围的 mutex，放弃的 mutex 可能指示应用程序已突然终止（例如，通过使用 Windows 任务管理器终止）。  
  
## <a name="local-and-system-mutexes"></a>本地 mutex 和系统 mutex  

 Mutex 有两种类型：本地 mutex 和命名系统 mutex。 如果使用接受名称的构造函数创建 <xref:System.Threading.Mutex> 对象，此对象与同名的操作系统对象相关联。 命名系统 mutex 在整个操作系统中都可见，并且可用于同步进程活动。 可以创建多个表示同一命名系统 mutex 的 <xref:System.Threading.Mutex> 对象，还能使用 <xref:System.Threading.Mutex.OpenExisting%2A> 方法打开现有的命名系统 mutex。  
  
 本地 mutex 仅存在于进程中。 进程中引用本地 <xref:System.Threading.Mutex> 对象的所有线程都可以使用本地 mutex。 每个 <xref:System.Threading.Mutex> 对象都是单独的本地 mutex。  
  
### <a name="access-control-security-for-system-mutexes"></a>系统 mutex 的访问控制安全性  

可借助 .NET 进行查询和设置命名系统对象的 Windows 访问控制安全性。 建议从创建起立刻开始保护系统 mutex，因为系统对象是全局对象，因此可能被不是你自己的代码锁定。  
  
 若要了解 mutex 访问控制安全性，请参阅 <xref:System.Security.AccessControl.MutexSecurity> 和 <xref:System.Security.AccessControl.MutexAccessRule> 类、<xref:System.Security.AccessControl.MutexRights> 枚举、<xref:System.Threading.Mutex> 类的 <xref:System.Threading.Mutex.GetAccessControl%2A>、<xref:System.Threading.Mutex.SetAccessControl%2A> 和 <xref:System.Threading.Mutex.OpenExisting%2A> 方法，以及 <xref:System.Threading.Mutex.%23ctor%28System.Boolean%2CSystem.String%2CSystem.Boolean%40%2CSystem.Security.AccessControl.MutexSecurity%29> 构造函数。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Threading.Mutex?displayProperty=nameWithType>
- <xref:System.Threading.Mutex.%23ctor%2A>
- <xref:System.Security.AccessControl.MutexSecurity?displayProperty=nameWithType>
- <xref:System.Security.AccessControl.MutexAccessRule?displayProperty=nameWithType>
- <xref:System.Threading.Monitor?displayProperty=nameWithType>
- [线程处理对象和功能](threading-objects-and-features.md)
- [线程与线程处理](threads-and-threading.md)
- [线程处理](index.md)
