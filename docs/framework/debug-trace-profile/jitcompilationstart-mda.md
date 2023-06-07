---
title: 'jitCompilationStart 托管调试助手 (MDA) '
description: JitCompilationStart 托管调试助手 (MDA) 在实时 (JIT) 编译器开始编译 .NET 函数时报告。
ms.date: 03/30/2017
helpviewer_keywords:
- JIT compilation
- MDAs (managed debugging assistants), JIT compilation
- JitCompilationStart MDA
- managed debugging assistants (MDAs), JIT compilation
ms.assetid: 5ffd2857-d0ba-4342-9824-9ffe04ec135d
ms.openlocfilehash: 228226d90e70d296681e48ffe85dada8eb18209a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96247364"
---
# <a name="jitcompilationstart-mda"></a>jitCompilationStart MDA

激活 `jitCompilationStart` 托管调试助手 (MDA) 以报告实时 (MDA) 编译器何时开始编译函数。  
  
## <a name="symptoms"></a>症状  

 由于将 mscorjit.dll 加载到进程中，因此已采用本机映像格式的程序的工作集大小增加。  
  
## <a name="cause"></a>原因  

不是程序所依赖的所有程序集都已生成为本机格式，或者程序集未正确注册。  

## <a name="resolution"></a>解决方法  

 如果启用此 MDA，则可以确定正在 JIT 编译的函数。 请确保将包含该函数的程序集生成为本机格式并正确注册。
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 此 MDA 在方法进行 JIT 编译前记录消息，因此启用此 MDA 会对性能产生重大影响。 如果方法是内联的，则此 MDA 不会生成单独的消息。  
  
## <a name="output"></a>输出  

 下面的代码示例显示了示例输出。 在这种情况下，输出显示，在程序集测试中，类 "ns2.CO" 上的方法 "m" 是 JIT 编译的。  
  
```output
method name="Test!ns2.C0::m"  
```  
  
## <a name="configuration"></a>Configuration  

 以下配置文件显示多种筛选器，可筛选出首次 JIT 编译时，报告哪些方法。 您可以通过将 name 特性的值设置为来指定报告所有方法 \* 。  
  
```xml  
<mdaConfig>  
  <assistants>  
    <jitCompilationStart>  
      <methods>  
        <match name="C0::m" />  
        <match name="MyMethod" />  
        <match name="C2::*" />  
        <match name="ns0::*" />  
        <match name="ns1.C0::*" />  
        <match name="ns2.C0::m" />  
        <match name="ns2.C0+N0::m" />  
      </methods>  
    </jitCompilationStart >  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>示例  

 以下示例代码用于上述配置文件。  
  
```csharp
using System;  
using System.Reflection;  
using System.Runtime.CompilerServices;  
using System.Runtime.InteropServices;  
  
public class Entry  
{  
    public static void Main(string[] args)  
    {  
        C0.m();  
        C1.MyMethod();  
        C2.m();  
  
        ns0.C0.m();  
        ns0.C0.N0.m();  
        ns0.C1.m();  
  
        ns1.C0.m();  
        ns1.C0.N0.m();  
  
        ns2.C0.m();  
        ns2.C0.N0.m();  
    }  
}  
  
public class C0  
{  
    [MethodImpl(MethodImplOptions.NoInlining)]  
    public static void m() { }  
}  
  
public class C1  
{  
    [MethodImpl(MethodImplOptions.NoInlining)]  
    public static void MyMethod() { }  
}  
  
public class C2  
{  
    [MethodImpl(MethodImplOptions.NoInlining)]  
    public static void m() { }  
}  
  
namespace ns0  
{  
    public class C0  
    {  
        [MethodImpl(MethodImplOptions.NoInlining)]  
        public static void m() { }  
  
        public class N0  
        {  
            [MethodImpl(MethodImplOptions.NoInlining)]  
            public static void m() { }  
        }  
    }  
  
    public class C1  
    {  
        [MethodImpl(MethodImplOptions.NoInlining)]  
        public static void m() { }  
    }  
}  
  
namespace ns1  
{  
    public class C0  
    {  
        [MethodImpl(MethodImplOptions.NoInlining)]  
        public static void m() { }  
        public class N0  
        {  
            [MethodImpl(MethodImplOptions.NoInlining)]  
            public static void m() { }  
        }  
    }  
}  
  
namespace ns2  
{  
    public class C0  
    {  
        [MethodImpl(MethodImplOptions.NoInlining)]  
        public static void m() { }  
  
        public class N0  
        {  
            [MethodImpl(MethodImplOptions.NoInlining)]  
            public static void m() { }  
        }  
    }  
}  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
- [互操作封送处理](../interop/interop-marshaling.md)
