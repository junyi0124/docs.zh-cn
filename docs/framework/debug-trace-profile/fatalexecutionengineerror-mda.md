---
title: fatalExecutionEngineError MDA
description: 查看 .NET 中的 fatalExecutionEngineError 托管调试助手 (MDA) ，这可能是由于意外的进程终止而激活。
ms.date: 03/30/2017
helpviewer_keywords:
- corrupted CLR
- fatal execution error
- terminated processes
- unexpected terminations
- fatal errors
- MDAs (managed debugging assistants), fatal errors
- process termination
- FatalExecutionEngineError MDA
- managed debugging assistants (MDAs), fatal errors
ms.assetid: 8b559e44-2393-4e4e-8160-7558d37a4a89
ms.openlocfilehash: a9347338d53755b74b3ff291f75cb6b221134130
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96244270"
---
# <a name="fatalexecutionengineerror-mda"></a>fatalExecutionEngineError MDA

在公共语言运行时 (CLR) 中检测到灾难性错误时，会激活 `fatalExecutionEngineError` 托管调试助手 (MDA)。 进程会终止。  
  
## <a name="symptoms"></a>症状  

 进程意外终止。 无法确定其他症状，因为有多种原因会导致 CLR 故障。  
  
## <a name="cause"></a>原因  

 CLR 已严重损坏。 这通常是由数据损坏导致的，而数据损坏可能涉及许多问题，如调用格式不正确的平台调用函数以及将无效的数据传递到 CLR。  
  
## <a name="resolution"></a>解决方法  

 启用其他 MAD 可能有助于确定此问题。 以下 MDA 对诊断该问题尤为有用：  
  
- [invalidOverlappedToPinvoke](invalidoverlappedtopinvoke-mda.md)  
  
- [overlappedFreeError](overlappedfreeerror-mda.md)  
  
- [pInvokeStackImbalance](pinvokestackimbalance-mda.md)  
  
- [gcUnmanagedToManaged](gcunmanagedtomanaged-mda.md)  
  
- [gcManagedToUnmanaged](gcmanagedtounmanaged-mda.md)  
  
- [callbackOnCollectedDelegate](callbackoncollecteddelegate-mda.md)  
  
- [reportAvOnComRelease](reportavoncomrelease-mda.md)  
  
- [invalidVariant](invalidvariant-mda.md)  
  
- [invalidIUnknown](invalidiunknown-mda.md)  
  
- [raceOnRCWCleanup](raceonrcwcleanup-mda.md)  
  
- [invalidFunctionPointerInDelegate](invalidfunctionpointerindelegate-mda.md)  
  
- [invalidGCHandleCookie](invalidgchandlecookie-mda.md)  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 此 MDA 对运行时无任何影响。  
  
## <a name="output"></a>输出  

 造成灾难性错误的 CLR 函数的地址，发生错误的线程的 ID，以及错误代码。  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <fatalExecutionEngineError />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareMethod%2A>
- <xref:System.Runtime.ConstrainedExecution.Cer>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
