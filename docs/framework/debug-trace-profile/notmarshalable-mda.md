---
title: notMarshalable MDA
description: 查看 notMarshalable 托管调试助手，如果调用没有服务或在 COM 接口指针的错误上下文中出现，则可以激活。
ms.date: 03/30/2017
helpviewer_keywords:
- managed debugging assistants (MDAs), interface pointer not marshalable
- interface pointer not marshalable MDA
- MDAs (managed debugging assistants), interface pointer not marshalable
- marshaling, run-time errors
- managed debugging assistants (MDAs), marshaling
- marshalable interface pointers
- MDAs (managed debugging assistants), marshaling
- notMarshalable MDA
ms.assetid: 96e7b2c1-843f-4d64-b519-740c3a18b50a
ms.openlocfilehash: 344c275d8645b16de3ecb06517297df06323ced4
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96254553"
---
# <a name="notmarshalable-mda"></a>notMarshalable MDA

当公共语言运行时 (CLR) 尝试跨上下文封送接口时，如果遇到 COM 接口指针且没有有效的注册代理/存根或 `IMarshal` 接口实现不正确，将激活 `notMarshalable` 托管调试助手 (MDA)。  
  
## <a name="symptoms"></a>症状  

 调用得不到响应，或在 COM 接口指针的错误上下文中进行调用。  
  
## <a name="cause"></a>原因  

 尝试跨上下文封送接口时，没有有效的注册代理/存根或 `IMarshal` 不正确。  
  
## <a name="resolution"></a>解决方法  

 请确保注册了代理存根，且 `IMarshal` 实现有效。  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 此 MDA 对运行时无任何影响。  
  
## <a name="output"></a>输出  

 描述问题的消息。  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <notMarshalable/>  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
- [互操作封送处理](../interop/interop-marshaling.md)
