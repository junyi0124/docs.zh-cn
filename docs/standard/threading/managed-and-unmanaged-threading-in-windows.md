---
title: Windows 中的托管和非托管线程处理
ms.date: 10/24/2018
elpviewer_keywords:
- threading [.NET], unmanaged
- threading [.NET], managed
- threading [.NET], managed
- threads and fibers [.NET]
- managed threading
ms.assetid: 4fb6452f-c071-420d-9e71-da16dee7a1eb
ms.openlocfilehash: 67d8fdb5f2e49930b25328c1dfd30a6105fd636f
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94826321"
---
# <a name="managed-and-unmanaged-threading-in-windows"></a>Windows 中的托管和非托管线程处理

所有线程的管理都是通过 <xref:System.Threading.Thread> 类完成的，包括由公共语言运行时创建的线程以及在运行时以外创建并进入托管环境以执行代码的线程。 运行时监视其进程中曾经在托管执行环境中执行过代码的所有线程。 它不跟踪任何其他线程。 线程可以通过 COM 互操作（原因是运行时将托管对象作为 COM 对象向非托管领域公开）、COM [DllGetClassObject](/windows/desktop/api/combaseapi/nf-combaseapi-dllgetclassobject) 函数和平台调用进入托管执行环境。  
  
 当非托管线程进入运行时（如通过 COM 可调用包装）时，系统将检查该线程的线程本地存储区以查找内部托管 <xref:System.Threading.Thread> 对象。 若找到一个对象，运行时就会注意到该线程。 但如果一个也找不到，则运行时将生成新的 <xref:System.Threading.Thread> 对象并将其安装在该线程的线程本地存储区中。  
  
 在托管线程处理中， <xref:System.Threading.Thread.GetHashCode%2A?displayProperty=nameWithType> 是稳定的托管线程标识。 在线程的生存期内，它不会与来自其他任何线程的值相冲突，不管你是从哪个应用程序域获取该值。  
  
## <a name="mapping-from-win32-threading-to-managed-threading"></a>从 Win32 线程处理到托管线程处理的映射

 下表将 Win32 线程处理元素映射为其近似的运行时等效元素。 注意，此映射不表示具有相同的功能。 例如， **TerminateThread** 不执行 **finally** 子句或释放资源，并且不能被禁止。 但 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> 可以执行所有回滚代码，回收所有资源，并可以使用 <xref:System.Threading.Thread.ResetAbort%2A>来拒绝。 请确保在对功能进行假设之前仔细阅读该文档。  
  
|在 Win32 中|在公共语言运行时中|  
|--------------|------------------------------------|  
|**CreateThread**|**Thread** 和 <xref:System.Threading.ThreadStart>的组合。|  
|**TerminateThread**|<xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>|  
|**SuspendThread**|<xref:System.Threading.Thread.Suspend%2A?displayProperty=nameWithType>|  
|**ResumeThread**|<xref:System.Threading.Thread.Resume%2A?displayProperty=nameWithType>|  
|**休眠**|<xref:System.Threading.Thread.Sleep%2A?displayProperty=nameWithType>|  
|线程句柄上的 **WaitForSingleObject**|<xref:System.Threading.Thread.Join%2A?displayProperty=nameWithType>|  
|**ExitThread**|无等效项|  
|**GetCurrentThread**|<xref:System.Threading.Thread.CurrentThread%2A?displayProperty=nameWithType>|  
|**SetThreadPriority**|<xref:System.Threading.Thread.Priority%2A?displayProperty=nameWithType>|  
|无等效项|<xref:System.Threading.Thread.Name%2A?displayProperty=nameWithType>|  
|无等效项|<xref:System.Threading.Thread.IsBackground%2A?displayProperty=nameWithType>|  
|接近 **CoInitializeEx** (OLE32.DLL)|<xref:System.Threading.Thread.ApartmentState%2A?displayProperty=nameWithType>|  
  
## <a name="managed-threads-and-com-apartments"></a>托管线程和 COM 单元

可以标记一个托管线程以指示它将承载一个 [单线程](/windows/desktop/com/single-threaded-apartments) 或 [多线程](/windows/desktop/com/multithreaded-apartments) 单元。 （有关 COM 线程处理体系结构的详细信息，请参阅 [进程、线程和单元](/windows/desktop/com/processes--threads--and-apartments)。）<xref:System.Threading.Thread.GetApartmentState%2A> 类的 <xref:System.Threading.Thread.SetApartmentState%2A>、<xref:System.Threading.Thread.TrySetApartmentState%2A> 和 <xref:System.Threading.Thread> 方法返回并分配线程的单元状态。 如果未设置该状态，则 <xref:System.Threading.Thread.GetApartmentState%2A> 返回 <xref:System.Threading.ApartmentState.Unknown?displayProperty=nameWithType>。  
  
 只有当线程处于 <xref:System.Threading.ThreadState.Unstarted?displayProperty=nameWithType> 状态时才可以设置该属性；但一个线程只能设置一次。  
  
 如果在启动线程之前未设置单元状态，则该线程被初始化为多线程单元 (MTA)。 终结器线程和由 <xref:System.Threading.ThreadPool> 控制的所有线程都是 MTA。  
  
> [!IMPORTANT]
> 对于应用程序启动代码，控制单元状态的唯一方式是将 <xref:System.MTAThreadAttribute> 或 <xref:System.STAThreadAttribute> 应用于入口点过程。
  
 向 COM 公开的托管对象的行为就如同它们聚合了自由线程封送拆收器一样。 换言之，它们可以通过自由线程的方式从任何 COM 单元中调用。 唯一不显示这种自由线程行为的托管对象是那些从 <xref:System.EnterpriseServices.ServicedComponent> 或 <xref:System.Runtime.InteropServices.StandardOleMarshalObject> 派生的对象。  
  
 在托管领域中，不支持 <xref:System.Runtime.Remoting.Contexts.SynchronizationAttribute> ，除非使用上下文和上下文绑定的托管实例。 如果使用的是企业服务，对象必须派生自 <xref:System.EnterpriseServices.ServicedComponent>（它本身派生自 <xref:System.ContextBoundObject>）。  
  
 当托管代码调用至 COM 对象时，它始终遵循 COM 规则。 换言之，它遵循 OLE32 的规定，通过 COM 单元代理和 COM+ 1.0 上下文包装来调用。  
  
## <a name="blocking-issues"></a>阻止问题  

在阻止了非托管代码中的线程的操作系统中，如果线程进行非托管调用，则运行时将不会为 <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> 或 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>控制该线程。 对于 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>，运行时将该线程标记为 **Abort** ，并在重新进入托管代码时控制该线程。 使用托管阻止而不使用非托管阻止更为可取。 <xref:System.Threading.WaitHandle.WaitOne%2A?displayProperty=nameWithType>、<xref:System.Threading.WaitHandle.WaitAny%2A?displayProperty=nameWithType>, <xref:System.Threading.WaitHandle.WaitAll%2A?displayProperty=nameWithType>, <xref:System.Threading.Monitor.Enter%2A?displayProperty=nameWithType>, <xref:System.Threading.Monitor.TryEnter%2A?displayProperty=nameWithType>, <xref:System.Threading.Thread.Join%2A?displayProperty=nameWithType>, <xref:System.GC.WaitForPendingFinalizers%2A?displayProperty=nameWithType>等都对 <xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> 和 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType>做出响应。 而且，如果你的线程处于单线程单元，则当你的线程被阻止时，单元中的所有托管阻止操作都将正确发送消息。  

## <a name="threads-and-fibers"></a>线程和纤程

.NET 线程处理模型不支持[纤程](/windows/desktop/procthread/fibers)。 不得调用通过使用纤程实现的任何非托管函数。 此类调用可能会导致 .NET 运行时崩溃。

## <a name="see-also"></a>另请参阅

- <xref:System.Threading.Thread.ApartmentState%2A?displayProperty=nameWithType>
- <xref:System.Threading.ThreadState>
- <xref:System.EnterpriseServices.ServicedComponent>
- <xref:System.Threading.Thread>
- <xref:System.Threading.Monitor>
