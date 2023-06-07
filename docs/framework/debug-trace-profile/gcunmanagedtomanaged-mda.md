---
title: gcUnmanagedToManaged MDA
description: 查看 .NET 中的 gcManagedToUnmanaged 托管调试助手。 由于在转换到托管代码期间发生了垃圾堆损坏，此 MDA 可能会激活。
ms.date: 03/30/2017
helpviewer_keywords:
- MDAs (managed debugging assistants), garbage collection
- GC unmanaged to managed
- transitioning threads unmanaged to managed code
- GcUnmanagedToManaged MDA
- threading [.NET Framework], garbage collection
- managed debugging assistants (MDAs), garbage collection
- threading [.NET Framework], managed debugging assistants
- garbage collection, run-time errors
- unmanaged to managed garbage collection
ms.assetid: 103eb3a3-1cf0-4406-8a9a-a7798fdc22d1
ms.openlocfilehash: 61fbdbf0d3941aa3e876748ae4d76cd7ad0c0977
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96244218"
---
# <a name="gcunmanagedtomanaged-mda"></a>gcUnmanagedToManaged MDA

每当一个线程从托管代码转换到非托管代码，`gcUnmanagedToManaged` 托管调试助手 (MDA) 就会导致垃圾回收。  
  
## <a name="symptoms"></a>症状  

 使用 COM 和平台调用来运行非托管用户组件的应用程序在 CLR 中导致了一个非确定性的访问冲突。  
  
## <a name="cause"></a>原因  

 如果一个应用程序运行非托管用户组件，则这些组件可能损坏了已垃圾回收的堆。 在垃圾回收器尝试审核对象图时，这会在 CLR 中导致访问冲突。  
  
## <a name="resolution"></a>解决方法  

 通过在每次托管转换之前强制垃圾回收来启用此助手，可以减少从非托管组件损坏已垃圾回收的堆到发生访问冲突之间的时间。  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 导致每当发生从非托管代码到托管代码的线程转换时都进行垃圾回收。  
  
## <a name="output"></a>输出  

 此 MDA 不会产生任何输出。  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <gcUnmanagedToManaged/>  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
- [gcManagedToUnmanaged](gcmanagedtounmanaged-mda.md)
- [互操作封送处理](../interop/interop-marshaling.md)
