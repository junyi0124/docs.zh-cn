---
title: JIT 跟踪 ETW 事件
description: 了解实时 (JIT) 跟踪 ETW 事件。 这些事件收集与 JIT 内联和 JIT 尾调用成功或失败有关的信息。
ms.date: 03/30/2017
helpviewer_keywords:
- JIT tracing events [.NET Framework]
- ETW, JIT tracing events (CLR)
ms.assetid: 926adde2-c123-452e-bf4f-4b977bf06ffb
ms.openlocfilehash: fd1fae8731ea927ab64d0c0df83b90d29289a0a0
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96277963"
---
# <a name="jit-tracing-etw-events"></a>JIT 跟踪 ETW 事件

这些事件可收集有关实时 (JIT) 内联和 JIT 尾调用成功或失败的信息。

## <a name="jit-inlining-events"></a>JIT 内联事件

### <a name="methodjitinliningfailed-event"></a>MethodJitInliningFailed 事件

 下表显示了关键字和级别。 （有关详细信息，请参阅 [CLR ETW Keywords and Levels](clr-etw-keywords-and-levels.md)。）  
  
|引发事件的关键字|Level|  
|-----------------------------------|-----------|  
|`JITTracingKeyword` (0x10)|详细级别 (5)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|在发生以下情况时引发|  
|-----------|--------------|-----------------|  
|`MethodJitInliningFailed`|186|JIT 内联失败。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|MethodBeingCompiledNameSpace|win:UnicodeString|正在编译的方法的命名空间。|  
|MethodBeingCompiledName|win:UnicodeString|正在编译的方法的名称。|  
|MethodBeingCompiledNameSignature|win:UnicodeString|正在编译的方法的签名。|  
|InlinerNamespace|win:UnicodeString|JIT 编译器正在尝试为其生成代码的方法的命名空间。|  
|InlinerName|win:UnicodeString|编译器正在尝试为其生成代码的方法的名称。 如果编译器正在尝试将代码内联到 `MethodBeingCompiledName` ，而不是生成对 `MethodBeingCompiledName` 的调用，则此名称可能会与 `InlinerName`不相同。|  
|InlinerNameSignature|win:UnicodeString|内联方的签名。|  
|InlineeNamespace|win:UnicodeString|被内联方的命名空间。|  
|InlineeName|win:UnicodeString|编译器正在尝试内联的方法（不生成对此方法的调用）。|  
|InlineeNameSignature|win:UnicodeString|被内联方的签名。|  
|FailAlways|win:Boolean|提示 JIT 编译器针对被内联方的内联操作将始终失败。|  
|FailReason|win:UnicodeString|INLINE_NEVER 表示前一个内联尝试确定了内联操作因某些其他原因将永远不会成功；否则为任意形式的文本。|  
|ClrInstanceID|win:UnicodeString|CLR 或 CoreCLR 的实例的唯一 ID。|  
  
### <a name="methodjitinliningsucceeded-event"></a>MethodJitInliningSucceeded 事件  

 下表显示了关键字和级别。  
  
|引发事件的关键字|Level|  
|-----------------------------------|-----------|  
|`JITTracingKeyword` (0x10)|详细级别 (5)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|在发生以下情况时引发|  
|-----------|--------------|-----------------|  
|`MethodJitInliningSucceeded`|185|方法内联成功。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|MethodBeingCompiledNameSpace|win:UnicodeString|正在编译的方法的命名空间。|  
|MethodBeingCompiledName|win:UnicodeString|正在编译的方法的名称。|  
|MethodBeingCompiledNameSignature|win:UnicodeString|正在编译的方法的签名。|  
|InlinerNamespace|win:UnicodeString|JIT 编译器正在尝试为其生成代码的方法的命名空间。|  
|InlinerName|win:UnicodeString|编译器正在尝试为其生成代码的方法的名称。 如果编译器正在尝试将代码内联到 `MethodBeingCompiledName` ，而不是生成对 `MethodBeingCompiledName` 的调用，则此名称可能会与 `InlinerName`不相同。|  
|InlinerNameSignature|win:UnicodeString|内联方的签名。|  
|InlineeNamespace|win:UnicodeString|被内联方的命名空间。|  
|InlineeName|win:UnicodeString|编译器正在尝试内联的方法（不生成对此方法的调用）。|  
|InlineeNameSignature|win:UnicodeString|被内联方的签名。|  
|ClrInstanceID|win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|  

## <a name="jit-tail-call-events"></a>JIT 尾调用事件  
  
### <a name="methodjittailcallfailed-event"></a>MethodJITTailCallFailed 事件  

 下表显示了关键字和级别。  
  
|引发事件的关键字|Level|  
|-----------------------------------|-----------|  
|`JITTracingKeyword` (0x10)|详细级别 (5)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|在发生以下情况时引发|  
|-----------|--------------|-----------------|  
|`MethodJitTailCallFailed`|189|方法尾调用失败。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|MethodBeingCompiledNameSpace|win:UnicodeString|正在编译的方法的命名空间。|  
|MethodBeingCompiledName|win:UnicodeString|正在编译的方法的名称。|  
|MethodBeingCompiledNameSignature|win:UnicodeString|正在编译的方法的签名。|  
|CallerNamespace|win:UnicodeString|JIT 编译器正在尝试为其生成代码的方法的命名空间。|  
|CallerName|win:UnicodeString|编译器正在尝试为其生成代码的方法的名称。|  
|CallerNameSignature|win:UnicodeString|调用方的签名。|  
|CalleeNamespace|win:UnicodeString|被调用方的命名空间。|  
|CalleeName|win:UnicodeString|编译器正在尝试尾调用的方法（不生成对此方法的调用）。|  
|CalleeNameSignature|win:UnicodeString|被调用方的签名。|  
|TailPrefix|win:Boolean|尾调用的前缀|  
|FailReason|win:UnicodeString|尾调用失败的原因。|  
|ClrInstanceID|win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|  
  
### <a name="methodjittailcallsucceeded-event"></a>MethodJITTailCallSucceeded 事件  

 下表显示了关键字和级别。  
  
|引发事件的关键字|Level|  
|-----------------------------------|-----------|  
|`JITTracingKeyword` (0x10)|详细级别 (5)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|在发生以下情况时引发|  
|-----------|--------------|-----------------|  
|`MethodJitTailCallSucceeded`|188|方法尾调用成功。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|MethodBeingCompiledNameSpace|win:UnicodeString|正在编译的方法的命名空间。|  
|MethodBeingCompiledName|win:UnicodeString|正在编译的方法的名称。|  
|MethodBeingCompiledNameSignature|win:UnicodeString|正在编译的方法的签名。|  
|CallerNamespace|win:UnicodeString|JIT 编译器正在尝试为其生成代码的方法的命名空间。|  
|CallerName|win:UnicodeString|编译器正在尝试为其生成代码的方法的名称。|  
|CallerNameSignature|win:UnicodeString|调用方的签名。|  
|CalleeNamespace|win:UnicodeString|被调用方的命名空间。|  
|CalleeName|win:UnicodeString|编译器正在尝试尾调用的方法（不生成对此方法的调用）。|  
|CalleeNameSignature|win:UnicodeString|被调用方的签名。|  
|TailPrefix|win:Boolean|尾调用的前缀。|  
|TailCallType|win:UnicodeString|尾调用的类型。|  
|ClrInstanceID|win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|  
  
## <a name="see-also"></a>另请参阅

- [CLR ETW 事件](clr-etw-events.md)
