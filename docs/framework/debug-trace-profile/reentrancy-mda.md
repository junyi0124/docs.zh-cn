---
title: 重入 MDA
description: 查看可在对象堆损坏或从本机到托管代码进行转换时发生的其他严重错误时激活的重入 MDA。
ms.date: 03/30/2017
helpviewer_keywords:
- unmanaged code, debugging
- transitioning threads unmanaged to managed code
- reentrancy MDA
- reentrancy without an orderly transition
- managed debugging assistants (MDAs), reentrancy
- illegal reentrancy
- MDAs (managed debugging assistants), reentrancy
- threading [.NET Framework], managed debugging assistants
- managed code, debugging
- native debugging, MDAs
ms.assetid: 7240c3f3-7df8-4b03-bbf1-17cdce142d45
ms.openlocfilehash: 0480b1a5aafbc8c4f9645ba83383d3a222d1f1c5
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96264577"
---
# <a name="reentrancy-mda"></a>重入 MDA

先前未通过有序转换执行从托管代码到本机代码的转换的情况下，如果尝试执行从本机代码到托管代码的转换，会激活 `reentrancy` 托管调试助手 (MDA)。  
  
## <a name="symptoms"></a>症状  

 从本机转换到托管代码时，对象堆已损坏或发生其他严重错误。  
  
 沿任一方向在本机代码和托管代码之间进行切换的线程必须执行有序转换。 但是，操作系统中的某些底层扩展性点（例如向量异常处理程序）可从托管代码切换到本机代码而无需执行有序转换。  这些转换由操作系统控制，而不是公共语言运行时 (CLR) 控制。  在这些扩展性点内执行的任何本机代码都必须避免回调入托管代码。  
  
## <a name="cause"></a>原因  

 执行托管代码时激活了某个底层操作系统扩展性点，例如向量异常处理程序。  通过该扩展性点调用的应用程序代码正在尝试回调入托管代码。  
  
 此问题通常是由应用程序代码引起的。  
  
## <a name="resolution"></a>解决方法  

 检查堆栈跟踪，确定激活此 MDA 的线程。  线程正在尝试非法调入托管代码。  堆栈跟踪应该展示使用此扩展性点的应用程序代码、提供此扩展性点的操作系统代码和此扩展性点中断的托管代码。  
  
 例如，尝试从矢量异常处理程序中调用托管代码时会看到 MDA 激活。  堆栈上会看到操作系统异常处理代码和某些触发 <xref:System.DivideByZeroException> 或 <xref:System.AccessViolationException> 等异常的托管代码。  
  
 在此示例中，正确的解决方法是完全以非托管代码实现向量异常处理程序。  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 此 MDA 对 CLR 无任何影响。  
  
## <a name="output"></a>输出  

 MDA 报告正在尝试非法重入。  检查线程的堆栈以确定发生原因和如何更正此问题。 下面是示例输出。  
  
```output
Additional Information: Attempting to call into managed code without
transitioning out first.  Do not attempt to run managed code inside
low-level native extensibility points. Managed Debugging Assistant
'Reentrancy' has detected a problem in 'D:\ConsoleApplication1\  
ConsoleApplication1\bin\Debug\ConsoleApplication1.vshost.exe'.  
```  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <reentrancy />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>示例  

 以下代码示例引发 <xref:System.AccessViolationException>。  支持矢量异常处理的 Windows 版本中，这会调用托管向量异常处理程序。  如果启用 `reentrancy` MDA，MDA 将在尝试从操作系统矢量异常处理支持代码调用到 `MyHandler` 时激活。  
  
```csharp
using System;  
public delegate int ExceptionHandler(IntPtr ptrExceptionInfo);  
  
public class Reenter
{  
    public static ExceptionHandler keepAlive;  
  
    [System.Runtime.InteropServices.DllImport("kernel32", ExactSpelling=true,
        CharSet=System.Runtime.InteropServices.CharSet.Auto)]  
    public static extern IntPtr AddVectoredExceptionHandler(int bFirst,
        ExceptionHandler handler);  
  
    static int MyHandler(IntPtr ptrExceptionInfo)
    {  
        // EXCEPTION_CONTINUE_SEARCH  
        return 0;  
    }  
    void Run() {}  
  
    static void Main()
    {  
        keepAlive = new ExceptionHandler(Reenter.MyHandler);  
        IntPtr ret = AddVectoredExceptionHandler(1, keepAlive);  
        try
        {  
            // Dispatch on null should AV.  
            Reenter r = null;
            r.Run();  
        }
        catch { }  
    }  
}  
```  
  
## <a name="see-also"></a>另请参阅

- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
