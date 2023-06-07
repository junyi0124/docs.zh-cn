---
title: invalidMemberDeclaration MDA
description: 查看 invalidMemberDeclaration 托管调试助手，如果在未调用托管方法的情况下将错误 HRESULT 返回到 COM，则会调用它。
ms.date: 03/30/2017
helpviewer_keywords:
- invalid member declaration
- InvalidMemberDeclaration MDA
- marshaling, run-time errors
- managed debugging assistants (MDAs), marshaling
- MDAs (managed debugging assistants), marshaling
ms.assetid: a84dd9a3-d6cf-4824-989a-ecbbf443eeb4
ms.openlocfilehash: c8d6c69fc6eb4f5f690b02809fdc492da675c3b1
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96272548"
---
# <a name="invalidmemberdeclaration-mda"></a>invalidMemberDeclaration MDA

激活 `invalidMemberDeclaration` 托管调试助手 (MDA) 以报告在确定如何封送要从 COM 调用的成员的参数期间发生的错误。  
  
## <a name="symptoms"></a>症状  

 一个失败 HRESULT 被返回到 COM，且未调用托管方法。  
  
## <a name="cause"></a>原因  

 这很有可能是因为不兼容其中一个参数上的 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 特性。  
  
## <a name="resolution"></a>解决方法  

 指定参数上的有效 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 特性。  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 此 MDA 对 CLR 无任何影响。  
  
## <a name="output"></a>输出  

 信息性消息包括成员名称、类型名称和错误消息。  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <invalidMemberDeclaration/>  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
- [互操作封送处理](../interop/interop-marshaling.md)
