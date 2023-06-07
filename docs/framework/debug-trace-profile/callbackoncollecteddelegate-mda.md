---
title: callbackOnCollectedDelegate MDA
description: 查看 .NET 中的 callbackOnCollectedDelegate 托管调试助手 (MDA) ，如果在垃圾回收委托之后发生回调，则会调用该操作。
ms.date: 03/30/2017
dev_langs:
- cpp
helpviewer_keywords:
- MDAs (managed debugging assistants), garbage collection
- managed debugging assistants (MDAs), callback on collected delegates
- MDAs (managed debugging assistants), callback on collected delegates
- callback on delegate after garbage collection
- callbackOnCollectedDelegate MDA
- managed debugging assistants (MDAs), garbage collection
- call back collected delegates
- garbage collection, run-time errors
- delegates [.NET Framework], garbage collection
ms.assetid: 398b0ce0-5cc9-4518-978d-b8263aa21e5b
ms.openlocfilehash: 1d95d01fb1e5a49ca055717ef4701b6f46394df3
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96265227"
---
# <a name="callbackoncollecteddelegate-mda"></a>callbackOnCollectedDelegate MDA

如果委托作为函数指针从托管代码封送到非托管代码，并且对委托进行垃圾回收后在该函数指针上放置了一个回调，则 `callbackOnCollectedDelegate` 托管调试助手 (MDA) 被激活。  
  
## <a name="symptoms"></a>症状  

 在尝试通过从托管委托获得的函数指针调入托管代码时，发生访问冲突。 这些故障不是公共语言运行时 (CLR) bug，但由于 CLR 代码中发生了访问冲突，所以看上去像是运行时 bug。  
  
 故障并不一致；对函数指针的调用有时成功，有时失败。 仅在大负载下或在尝试次数随机时才可能发生这一故障。  
  
## <a name="cause"></a>原因  

 函数指针从其创建且向非托管代码公开的委托被垃圾回收。 当非托管组件尝试调用函数指针时，会产生访问冲突。  
  
 由于故障的发生取决于垃圾回收发生的时间，所以看上去故障像是随机发生的。 如果某个委托符合回收的条件，则垃圾回收可能发生在回调且调用成功之后。 在其他时候，垃圾回收发生在回调之前，回调生成访问冲突，然后程序停止。  
  
 故障的概率取决于封送委托和回调函数指针之间的时间差以及垃圾回收的频率。 如果封送委托和之后的回调之间的时间间隔很短，则故障将是偶发性的。 如果接收函数指针的非托管方法不保存函数指针以供以后使用，而是立即回调函数指针以在返回之前完成其操作，则通常会是这种情况。 同样，当系统负载很大时，将发生更多垃圾回收，这使得回调之前发生垃圾回收的可能性更大。  
  
## <a name="resolution"></a>解决方法  

 一旦将委托作为非托管函数指针封送出去，垃圾回收器就无法跟踪其生存期。 为了追踪非托管函数指针的生存期，您的代码必须保持对委托的引用。 但是在此之前，首先必须确定回收了哪个委托。 此 MDA 激活时，它提供该委托的类型名称。 使用此名称在代码中搜索将该委托传递到非托管代码的平台调用或 COM 签名。 有问题的委托是通过这些调用站点之一传递出去的。 还可以启用 `gcUnmanagedToManaged` MDA，以在每次运行时回调之前强制进行垃圾回收。 这一操作将通过确保垃圾回收始终发生在回调之前消除垃圾回收造成的不确定性。 一旦知悉回收了什么委托后，在托管的一方更改代码以保持对该委托的引用，以便跟踪封送的非托管函数指针的生存期。  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 将委托作为指针函数封送时，运行时分配一个 thunk，用于执行从非托管到托管的转换。 此 thunk 是非托管代码在最终调用托管委托前实际调用的对象。 如果未启用 `callbackOnCollectedDelegate` MDA，收集委托时将删除非托管封送处理代码。 如果启用了 `callbackOnCollectedDelegate` MDA，收集委托时不会立即删除非托管封送处理代码。 相反，在默认情况下，最后 1000 个实例都保持活动状态，然后在调用时更改以激活 MDA。 在收集了另外 1001 个封送的委托后，将最终删除此 thunk。  
  
## <a name="output"></a>输出  

 MDA 报告委托的类型名称，在尝试调用该委托的非托管函数指针前，收集该委托。  
  
## <a name="configuration"></a>Configuration  

 以下示例显示了应用程序配置选项。 它将 MDA 保持为活动状态的 thunk 的数量设置为 1,500。 默认 `listSize` 值为 1,000，最小值为 50，最大值为 2,000。  
  
```xml  
<mdaConfig>  
  <assistants>  
    <callbackOnCollectedDelegate listSize="1500" />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>示例  

 以下示例展示了一种情况，在这种情况下，可激活该 MDA：  
  
```cpp
// Library.cpp : Defines the unmanaged entry point for the DLL application.  
#include "windows.h"  
#include "stdio.h"  
  
void (__stdcall *g_pfTarget)();  
  
void __stdcall Initialize(void __stdcall pfTarget())  
{  
    g_pfTarget = pfTarget;  
}  
  
void __stdcall Callback()  
{  
    g_pfTarget();  
}
```

```csharp
// C# Client  
using System;  
using System.Runtime.InteropServices;  
  
public class Entry  
{  
    public delegate void DCallback();  
  
    public static void Main()  
    {  
        new Entry();  
        Initialize(Target);  
        GC.Collect();  
        GC.WaitForPendingFinalizers();  
        Callback();  
    }  
  
    public static void Target()  
    {
    }  
  
    [DllImport("Library", CallingConvention = CallingConvention.StdCall)]  
    public static extern void Initialize(DCallback pfDelegate);  
  
    [DllImport ("Library", CallingConvention = CallingConvention.StdCall)]  
    public static extern void Callback();  
  
    ~Entry() { Console.Error.WriteLine("Entry Collected"); }  
}  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
- [互操作封送处理](../interop/interop-marshaling.md)
- [gcUnmanagedToManaged](gcunmanagedtomanaged-mda.md)
