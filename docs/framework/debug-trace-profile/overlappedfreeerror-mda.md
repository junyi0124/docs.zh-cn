---
title: overlappedFreeError MDA
description: 查看 .NET 中的 overlappedFreeError 托管调试助手 (MDA) ，这可能会在访问冲突或垃圾回收堆损坏时激活。
ms.date: 03/30/2017
helpviewer_keywords:
- OverlappedFreeError MDA
- overlapped free method call error
- managed debugging assistants (MDAs), overlapped structures
- overlapped structures
- MDAs (managed debugging assistants), overlapped structures
- freeing overlapped structures
ms.assetid: b6ab2d48-6eee-4bab-97a3-046b3b0a5470
ms.openlocfilehash: 1bedb8ad3b9801f0d235371a397c8951ff8723b1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96254527"
---
# <a name="overlappedfreeerror-mda"></a>overlappedFreeError MDA

如果在重叠操作完成之前调用 <xref:System.Threading.Overlapped.Free%28System.Threading.NativeOverlapped%2A%29?displayProperty=nameWithType> 方法，将激活 `overlappedFreeError` 托管调试助手 (MDA)。  
  
## <a name="symptoms"></a>症状  

 访问冲突或垃圾回收堆损坏。  
  
## <a name="cause"></a>原因  

 操作完成之前，已释放重叠结构。 使用重叠指针的函数可能稍后会在释放结构后写入结构。 由于另一个对象当前可能占用该区域，这可能会导致堆损坏。  
  
 如果重叠操作未成功开始，则此 MDA 可能不表示错误。  
  
## <a name="resolution"></a>解决方法  

 在调用 <xref:System.Threading.Overlapped.Free%28System.Threading.NativeOverlapped%2A%29> 方法之前，确保已完成使用重叠结构的 I/O 操作。  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 此 MDA 对 CLR 无任何影响。  
  
## <a name="output"></a>输出  

 以下是此 MDA 的示例输出。  
  
 `An overlapped pointer (0x00ea3430) that was not allocated on the GC heap was passed via Pinvoke to the win32 function 'WriteFile' in module 'KERNEL32.DLL'. If the AppDomain is shut down, this can cause heap corruption when the async I/O completes. The best solution is to pass a NativeOverlappedStructure retrieved from a call to System.Threading.Overlapped.Pack(). If the AppDomain exits, the CLR will keep this structure alive and pinned until the I/O completes.`  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <overlappedFreeError/>  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
- [互操作封送处理](../interop/interop-marshaling.md)
