---
title: illegalPrepareConstrainedRegion MDA
description: 查看 illegalPrepareConstrainedRegion 托管调试助手，如果 PrepareConstrainedRegions 调用后不是 try 语句，则会调用该助手。
ms.date: 03/30/2017
helpviewer_keywords:
- PrepareConstrainedRegions method
- managed debugging assistants (MDAs), illegal PrepareConstrainedRegions
- try/catch exception handling, managed debugging assistants
- IllegalPrepareConstrainedRegions MDA
- MDAs (managed debugging assistants), illegal PrepareConstrainedRegions
ms.assetid: 2f9b5031-f910-4e01-a196-f89eab313eaf
ms.openlocfilehash: 7cbf04e8605ccf18e89882dd09b96cc7c59330c9
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96272782"
---
# <a name="illegalprepareconstrainedregion-mda"></a>illegalPrepareConstrainedRegion MDA

<xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A?displayProperty=nameWithType> 方法调用不立即出现在异常处理程序的 `try` 语句之前时，会激活 `illegalPrepareConstrainedRegion` 托管调试助手 (MDA)。 此限制处于 MSIL 级别，因此允许调用和 `try` 之间存在非代码生成的源，比如注释。  
  
## <a name="symptoms"></a>症状  

 受约束的执行区域 (CER) 从未以这种方式处理，而是作为简单的异常处理块（`finally` 或 `catch`）处理。 因此，如果内存不足或线程中止，此区域不会运行。  
  
## <a name="cause"></a>原因  

 CER 的准备模式未正确执行。  这是一个错误事件。 <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A>用于标记异常处理程序的方法调用在其块中引入 CER 时 `catch` / `finally` / `fault` / `filter` 必须在语句之前立即使用 `try` 。  
  
## <a name="resolution"></a>解决方法  

 确保对 <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A> 的调用在 `try` 语句之前立即发生。  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 此 MDA 对 CLR 无任何影响。  
  
## <a name="output"></a>输出  

 MDA 显示调用 <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A> 方法的方法名称、MSIL 偏移，以及指示调用不会立即出现在 try 块开头的消息。  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <illegalPrepareConstrainedRegion/>  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>示例  

 以下示例代码演示导致激活此 MDA 的模式。  
  
```csharp
void MethodWithInvalidPCR()  
{  
    RuntimeHelpers.PrepareConstrainedRegions();  
    Object o = new Object();  
    try  
    {  
        …  
    }  
    finally  
    {  
        …  
    }  
}  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareConstrainedRegions%2A>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
- [互操作封送处理](../interop/interop-marshaling.md)
