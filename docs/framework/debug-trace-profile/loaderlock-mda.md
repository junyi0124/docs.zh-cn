---
title: loaderLock MDA
description: 查看 .NET 中的 loaderLock 托管调试助手 (MDA) ，它检测到在持有 Windows OS 加载程序锁的线程上运行托管代码的尝试。
ms.date: 03/30/2017
helpviewer_keywords:
- deadlocks [.NET Framework]
- LoaderLock MDA
- MDAs (managed debugging assistants), loader locks
- managed debugging assistants (MDAs), loader locks
- operating system loader locks
- loader locks
- locks, threads
ms.assetid: 8c10fa02-1b9c-4be5-ab03-451d943ac1ee
ms.openlocfilehash: 0e8946a3482798f0a20d6304c7d42d2a5832ae61
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96240558"
---
# <a name="loaderlock-mda"></a>loaderLock MDA

`loaderLock` 托管调试助手 (MDA) 检测在持有 Microsoft Windows 操作系统加载程序锁的线程上执行托管代码的尝试。  任何此类执行都是非法的，因为这样可能会导致死锁，并导致在操作系统的加载程序已初始化 DLL 之前使用 DLL。  
  
## <a name="symptoms"></a>症状  

 在操作系统的加载程序锁中执行代码时，最常见故障是：线程在尝试调用其它也需要加载程序锁的 Win32 函数时，将产生死锁。  `LoadLibrary`、`GetProcAddress`、`FreeLibrary` 和 `GetModuleHandle` 就是此类函数的示例。  应用程序可能不会直接调用这些函数；公共语言运行时 (CLR) 可能会将这些函数作为更高级别调用（例如 <xref:System.Reflection.Assembly.Load%2A>）或对平台调用方法的首次调用的结果进行调用。  
  
 如果一个线程正在等待另一个线程开始或结束，也可能发生死锁。  线程开始或结束执行时，必须获取操作系统的加载程序锁，才能向受影响的 DLL 交付事件。  
  
 最后，还存在这样的情况：在操作系统的加载程序正确初始化 DLL 之前，调入 DLL。  死锁故障可通过检查死锁中涉及的所有线程的堆栈进行诊断，但这不同于死锁故障，在不使用此 MDA 的情况下很难诊断出未初始化的 DLL 的使用。  
  
## <a name="cause"></a>原因  

 除非采用了特殊措施（例如，与 /NOENTRY 链接），否则为 .NET Framework 1.0 或 1.1 版生成的混合托管/非托管 C++ 程序集通常尝试在加载程序锁内执行托管代码。
  
 为 .NET Framework 2.0 版生成的混合托管/非托管 C++ 程序集与使用违反操作系统规则的非托管 DLL 的应用程序相同，风险减低，不太容易受到这些问题的影响。  例如，如果非托管 DLL 的 `DllMain` 入口点调用`CoCreateInstance` 获取已向 COM 公开的托管对象，结果就是尝试在加载程序锁内执行托管代码。 有关 .NET Framework 2.0 及更高版本中加载程序锁问题的详细信息，请参阅[混合程序集的初始化](/cpp/dotnet/initialization-of-mixed-assemblies)。  
  
## <a name="resolution"></a>解决方法  

 在 Visual C++ .NET 2002 和 Visual C++ .NET 2003 中，使用 `/clr` 编译器选项编译的 DLL 在加载时可能会发生不确定的死锁；此问题被称为混合 DLL 加载或加载程序锁问题。 在 Visual C++ 2005 及更高版本中，已经从混合 DLL 加载进程中消除了几乎所有的不确定性。 但是，还有一些其他情况会导致加载程序锁定发生（肯定会发生这种情况）。 有关其余加载程序锁的原因和解决方法的详细介绍，请参阅[混合程序集的初始化](/cpp/dotnet/initialization-of-mixed-assemblies)。 如果该主题没有确定加载程序锁问题，则需要检查线程的堆栈，确定出现加载程序锁的原因以及如何更正此问题。 查看堆栈跟踪，确定激活此 MDA 的线程。  该线程在持有操作系统的加载程序锁定的同时，尝试非法调入托管代码。  可能会在堆栈上看到 DLL 的 `DllMain` 或等效的入口点。  操作系统关于可从这样的入口点合法执行的操作的规则具有很多限制。  这些规则排除任何托管执行。  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 通常，进程中的若干线程将发生死锁。  其中的某个线程可能是负责执行垃圾回收的线程，因此这种死锁对整个进程具有重大影响。  此外，它将阻止任何需要操作系统的加载程序锁的其他操作，例如加载和卸载程序集或 DLL 以及启动或停止线程。  
  
 在某些特殊情况下，可能还会在未初始化就被调用的 DLL 中触发访问冲突或类似问题。  
  
## <a name="output"></a>输出  

 此 MDA 报告正在尝试进行非法托管执行。  需要检查线程的堆栈，确定出现加载程序锁的原因以及如何更正此问题。  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <loaderLock/>  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>另请参阅

- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
