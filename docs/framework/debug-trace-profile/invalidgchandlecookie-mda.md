---
title: invalidGCHandleCookie MDA
description: 查看 invalidGCHandleCookie 托管调试助手 (MDA) ，该程序在尝试从无效 IntPtr cookie 转换为 GCHandle 时激活。
ms.date: 03/30/2017
helpviewer_keywords:
- MDAs (managed debugging assistants), invalid cookies
- cookies, invalid
- managed debugging assistants (MDAs), invalid cookies
- InvalidGCHandleCookie MDA
- invalid cookies
ms.assetid: 613ad742-3c11-401d-a6b3-893ceb8de4f8
ms.openlocfilehash: 36806498445da78c8d9dd5c51c1903b253a39da0
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96272600"
---
# <a name="invalidgchandlecookie-mda"></a>invalidGCHandleCookie MDA

尝试将无效的 <xref:System.IntPtr> Cookie 转换为 <xref:System.Runtime.InteropServices.GCHandle> 时，将激活 `invalidGCHandleCookie` 托管调试助手（MDA）。  
  
## <a name="symptoms"></a>症状  

 尝试从 <xref:System.IntPtr> 使用或检索 <xref:System.Runtime.InteropServices.GCHandle> 时，发生未定义的行为，如访问冲突和内存损坏。  
  
## <a name="cause"></a>原因  

 该 Cookie 可能是无效的，因为它原本不是从 <xref:System.Runtime.InteropServices.GCHandle> 创建的。它表示已被释放的 <xref:System.Runtime.InteropServices.GCHandle>，是不同应用程序域中 <xref:System.Runtime.InteropServices.GCHandle> 的 Cookie，或者作为 <xref:System.Runtime.InteropServices.GCHandle> 被封送到了本机代码、但作为 <xref:System.IntPtr> 被传回到了 CLR，在那里尝试了转换。  
  
## <a name="resolution"></a>解决方法  

 为 <xref:System.Runtime.InteropServices.GCHandle> 指定有效的 <xref:System.IntPtr> Cookie。  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 此 MDA 启用后，调试程序不再能从根源追溯到对象，因为传回的 Cookie 值与 MDA 未启用时返回的值不同。  
  
## <a name="output"></a>输出  

 报告无效的 <xref:System.IntPtr> Cookie 值。  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <invalidGCHandleCookie />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.InteropServices.GCHandle.FromIntPtr%2A>
- <xref:System.Runtime.InteropServices.GCHandle>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
