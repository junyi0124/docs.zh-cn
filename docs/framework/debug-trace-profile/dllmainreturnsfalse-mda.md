---
title: dllMainReturnsFalse MDA
description: 了解 .NET 中的 dllMainReturnsFalse 托管调试助手。 如果 DLL 初始化失败，则会激活此 MDA。
ms.date: 03/30/2017
helpviewer_keywords:
- managed debugging assistants (MDAs), DllMain returns false
- DllMainReturnsFalse MDA
- DllMain function
- MDAs (managed debugging assistants), DllMain returns false
ms.assetid: e2abdd04-f571-4b97-8c16-2221b8588429
ms.openlocfilehash: 83f38c4918c1354477627b70a62e60cbdc7de275
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96273511"
---
# <a name="dllmainreturnsfalse-mda"></a>dllMainReturnsFalse MDA

如果用户程序集的托管 `DllMain` 函数（因 DLL_PROCESS_ATTACH 原因而调用）返回 FALSE，则将激活 `dllMainReturnsFalse` 托管调试助手 (MDA)。  
  
## <a name="symptoms"></a>症状  

 `DllMain` 函数返回 FALSE，表示其未正确执行。 这会导致一些未确定的问题，因为 `DllMain` 函数通常包含重要的初始化代码。  
  
## <a name="cause"></a>原因  

 因 DLL_PROCESS_ATTACH 原因调用 `DllMain` 函数，在上传时初始化 DLL。 如果它返回 FALSE，则意味着该 DLL 初始化失败。  
  
## <a name="resolution"></a>解决方法  

 分析失败的 DLL 的 `DllMain` 函数的代码，找出初始化失败的原因。  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 此 MDA 对 CLR 无任何影响。 它只报告有关 `DllMain` 的返回值的数据。  
  
## <a name="output"></a>输出  

 一条指示 `DllMain` 函数（因 DLL_PROCESS_ATTACH 原因调用）返回 FALSE 的消息。 请注意，此 MDA 仅在托管代码中实现 `DllMain` 时才激活。  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <dllMainReturnsFalse />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>另请参阅

- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
