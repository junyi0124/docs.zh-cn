---
title: dirtyCastAndCallOnInterface MDA
description: 查看 dirtyCastAndCallOnInterface 托管调试助手，这是在早期绑定的 vtable 调用在仅限后期绑定的类接口上完成时调用的。
ms.date: 03/30/2017
helpviewer_keywords:
- managed debugging assistants (MDAs), early bound calls AutoDispatch
- dispatch-only mode
- dirtyCastAndCallOnInterface
- early-bound managed classes
- early bound calls on AutoDispatch
- MDAs (managed debugging assistants), early bound calls AutoDispatch
- EarlyBoundCallOnAutorDispatchClassInteface MDA
ms.assetid: aa388ed3-7e3d-48ea-a0b5-c47ae19cec38
ms.openlocfilehash: 49a2930d91e76856436480512360e8b9f9fd3d7a
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96273576"
---
# <a name="dirtycastandcalloninterface-mda"></a>dirtyCastAndCallOnInterface MDA

当尝试通过 vtable 对已标记为仅后期绑定的类接口进行早期绑定调用时，则会激活 `dirtyCastAndCallOnInterface` 托管调试助手 (MDA)。  
  
## <a name="symptoms"></a>症状  

 当通过 COM 将早期绑定调用放入 CLR 时，应用程序会引发访问冲突或产生意外行为。  
  
## <a name="cause"></a>原因  

 代码正经由仅后期绑定的类接口尝试通过 vtable 进行早期绑定调用。 请注意，默认情况下，类接口标识为仅后期绑定。 也可通过具有 <xref:System.Runtime.InteropServices.ClassInterfaceType.AutoDispatch> 值 (`[ClassInterface(ClassInterfaceType.AutoDispatch)]`) 的 <xref:System.Runtime.InteropServices.ClassInterfaceAttribute> 特性将它们识别为后期绑定。  
  
## <a name="resolution"></a>解决方法  

 建议的解决方法是定义供 COM 使用的显式接口，并让 COM 客户端可以通过此接口而非自动生成的类接口进行调用。 或者，从 COM 进行的调用可以经由 `IDispatch` 转换为后期绑定调用。  
  
 最后，可将类标识为 <xref:System.Runtime.InteropServices.ClassInterfaceType.AutoDual> (`[ClassInterface(ClassInterfaceType.AutoDual)]`)，以便可从 COM 放置早期绑定的调用；但是，由于 <xref:System.Runtime.InteropServices.ClassInterfaceAttribute> 中所述的版本管理限制，强烈建议不要使用 <xref:System.Runtime.InteropServices.ClassInterfaceType.AutoDual>。  
  
## <a name="effect-on-the-runtime"></a>对运行时的影响  

 此 MDA 对 CLR 无任何影响。 它仅报告对后期绑定接口的早期绑定调用的数据。  
  
## <a name="output"></a>输出  

 按早期绑定访问的方法的名称或字段的名称。  
  
## <a name="configuration"></a>Configuration  
  
```xml  
<mdaConfig>  
  <assistants>  
    <dirtyCastAndCallOnInterface />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Runtime.InteropServices.ClassInterfaceAttribute>
- [使用托管调试助手诊断错误](diagnosing-errors-with-managed-debugging-assistants.md)
