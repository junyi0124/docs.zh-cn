---
title: invalidOverlappedToPinvoke MDA
description: 查看 .NET 中的 invalidOverlappedToPinvoke 托管调试助手 (MDA) ，这可能在崩溃或无法解释堆损坏期间激活。
ms.date: 03/30/2017
helpviewer_keywords:
- overlapped pointers
- managed debugging assistants (MDAs), overlapped pointers
- invalid overlapped pointer to platform invoke
- InvalidOverlappedToPinvoke MDA
- MDAs (managed debugging assistants), overlapped pointers
- pointers, overlapped
ms.assetid: 28876047-58bd-4fed-9452-c7da346d67c0
ms.openlocfilehash: 55b10af87130bd32410508cef2bc3e4e733549c0
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96281317"
---
# <a name="invalidoverlappedtopinvoke-mda"></a>invalidOverlappedToPinvoke MDA

当不是在垃圾回收堆上创建的重叠指针传递到特定的 Win32 函数时，会激活 `invalidOverlappedToPinvoke` 托管调试助手（MDA）。  
  
> [!NOTE]
> 默认情况下，仅当代码中定义了平台调用，并且调试器报告每个方法的 JustMyCode 状态时，才会激活此 MDA。 不理解 JustMyCode 的调试程序（如没有扩展的 MDbg.exe）将不会激活此 MDA。 通过使用配置文件和显式设置 .mda 配置文件中的 `justMyCode="false"` 可以为这些调试程序启用此 MDA `(<invalidOverlappedToPinvoke enable="true" justMyCode="false"/>`)。  
  
## <a name="symptoms"></a>症状  

 故障或无法解释的堆损坏。  
  
## <a name="cause"></a>原因  

 不是在垃圾回收堆上创建的重叠指针被传递到特定的操作系统函数。  
  
 下表显示了此 MDA 跟踪的函数。  
  
|模块|函数|  
|------------|--------------|  
|HttpApi.dll|`HttpReceiveHttpRequest`|  
|IpHlpApi.dll|`NotifyAddrChange`|  
|Kernel32.dll|`ReadFile`|  
|Kernel32.dll|`ReadFileEx`|  
|Kernel32.dll|`WriteFile`|  
|Kernel32.dll|`WriteFileEx`|  
|Kernel32.dll|`ReadDirectoryChangesW`|  
|Kernel32.dll|`PostQueuedCompletionStatus`|  
|MSWSock.dll|`ConnectEx`|  
|WS2_32.dll|`WSASend`|  
|WS2_32.dll|`WSASendTo`|  
|WS2_32.dll|`WSARecv`|  
|WS2_32.dll|`WSARecvFrom`|  
|MQRT.dll|`MQReceiveMessage`|  
  
 在这种情况下，堆损坏的可能性很大，因为进行调用的 <xref:System.AppDomain> 可能会卸载。 如果 <xref:System.AppDomain> 卸载，应用程序代码将释放重叠指针的内存，导致操作完成时发生损坏，或者代码将泄漏内存，导致以后发生问题。  
  
## <a name="resolution"></a>解决方法  

 使用 <xref:System.Threading.Overlapped> 对象，调用 <xref:System.Threading.Overlapped.Pack%2A> 方法以获取可以传递给函数的 <xref:System.Threading.NativeOverlapped> 结构。 如果卸载 <xref:System.AppDomain>，在释放指针之前，CLR 等待异步操作完成。  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 此 MDA 对 CLR 无任何影响。  
  
## <a name="output"></a>输出  

 以下是来自此 MDA 的输出示例。  
  
 `An overlapped pointer (0x00ea3430) that was not allocated on the GC heap was passed via Pinvoke to the Win32 function 'WriteFile' in module 'KERNEL32.DLL'. If the AppDomain is shut down, this can cause heap corruption when the async I/O completes. The best solution is to pass a NativeOverlapped structure retrieved from a call to System.Threading.Overlapped.Pack(). If the AppDomain exits, the CLR will keep this structure alive and pinned until the I/O completes.`  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <invalidOverlappedToPinvoke/>  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
- [互操作封送处理](../interop/interop-marshaling.md)
