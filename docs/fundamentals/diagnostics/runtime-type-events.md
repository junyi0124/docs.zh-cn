---
title: 类型系统运行时事件
description: 请参阅收集特定于 .NET 类型系统的诊断信息的 .NET 运行时事件，如 TypeLoadStart 和 TypeLoadStop。
ms.date: 11/13/2020
ms.topic: reference
helpviewer_keywords:
- type system events (CoreCLR)
- ETW, EventPipe, LTTng type system events (CoreCLR)
ms.openlocfilehash: 8eee89cddb0098da2cb449a4be21945adac725e3
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "96591055"
---
# <a name="net-runtime-type-events"></a>.NET 运行时类型事件

这些事件收集与加载类型相关的信息。 有关如何将这些事件用于诊断的详细信息，请参阅 [日志记录和跟踪 .net 应用程序](../../core/diagnostics/logging-tracing.md)

## <a name="typeloadstart-event"></a>TypeLoadStart 事件

|引发事件的关键字|事件|Level|  
|-----------------------------------|-----------|-----------|  
|`TypeDiagnosticKeyword` (0x8000000000) |信息性 (4)|  

|事件|事件 ID|描述|  
|-----------|--------------|-----------------|  
|`TypeLoadStart`|73|类型加载已开始。|

|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|`TypeLoadStartID`|`win:UInt32`|类型加载操作的 ID。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|  

## <a name="typeloadstop-event"></a>TypeLoadStop 事件

|引发事件的关键字|Level|  
|-----------------------------------|-----------|-----------|  
|`TypeDiagnosticKeyword` (0x8000000000) |信息性 (4)|  

|事件|事件 ID|描述|  
|-----------|--------------|-----------------|  
|`TypeLoadStop`|74|类型加载已完成。|

|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|`TypeLoadStartID`|`win:UInt32`|类型加载操作 (的 ID 与相应的 TypeLoadStart 事件的 TypeLoadStartID) 相匹配。|
|`LoadLevel`|`win:UInt16`|类型负载级别。|
|`TypeID`|`win:UInt64`|指向该类型句柄的指针。|
|`TypeName`|`win:UnicodeString`|类型的名称。|
|`ClrInstanceID`|`win:UInt16`|CLR 或 CoreCLR 的实例的唯一 ID。|  
