---
title: invalidVariant MDA
description: 查看 invalidVariant 托管调试助手，如果在从本机/非托管代码到托管代码的调用中遇到无效的变体，则会调用它。
ms.date: 03/30/2017
helpviewer_keywords:
- MDAs (managed debugging assistants), invalid variant
- VARIANT type errors
- InvalidVariant MDA
- invalid VARIANT types
- managed debugging assistants (MDAs), invalid variant
ms.assetid: d273e070-d1b1-4a53-a9c7-7af837b04a3d
ms.openlocfilehash: f0667a54e950ead27000eb6b93ebd547dea1cfb0
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96281295"
---
# <a name="invalidvariant-mda"></a>invalidVariant MDA

在从本机或非托管代码到托管代码的调用期间，如果遇到无效的 `VARIANT` 结构，将激活 `invalidVariant` 托管调试助手 (MDA)。  
  
## <a name="symptoms"></a>症状  

 在本机和托管代码之间转换的过程中的意外行为涉及将 `VARIANT` 封送处理到对象。  
  
## <a name="cause"></a>原因  

 本机代码正在向托管代码传递格式不正确的 `VARIANT` 结构。  如果 `VARIANT` 无效，运行时则会尝试向某个对象封送 `VARIANT` 并激活 MDA。 无效 `VARIANT` 的示例包括具有 `VARTYPE` VT_EMPTY &#124; VT_BYREF 的 `VARIANT` 或具有 `VARTYPE` VT_VARIANT 的 `VARIANT`。  
  
## <a name="resolution"></a>解决方法  

 传递 `VARIANT` 的本机或非托管代码必须确保 `VARIANT` 格式正确且已初始化。  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 此 MDA 对运行时无任何影响。  
  
## <a name="output"></a>输出  

 MDA 消息指示运行时检测到由非托管模块传递给托管代码的无效 `VARIANT`。  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <invalidVariant />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
- [互操作封送处理](../interop/interop-marshaling.md)
