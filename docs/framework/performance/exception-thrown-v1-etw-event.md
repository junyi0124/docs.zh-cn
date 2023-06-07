---
title: 异常 Thrown_V1 ETW 事件
description: 查看有关 ExceptionThrown_V1 ETW 事件的详细信息。 将为引发的异常提供事件数据（如字段名称、数据类型和说明）。
ms.date: 03/30/2017
helpviewer_keywords:
- ExceptionThrown_V1 event [.NET Framework]
- ETW, ExceptionThrown_V1 event (CLR)
ms.assetid: 0d3da389-6b7b-40f6-a877-fac546d6019c
ms.openlocfilehash: c1ba994b291bd278a95e34beb0b02ed6581786e8
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96263472"
---
# <a name="exception-thrown_v1-etw-event"></a>异常 Thrown_V1 ETW 事件

该事件捕获有关引发的异常的信息。  
  
 下表显示了引发事件的关键字以及事件的级别。 （有关详细信息，请参阅 [CLR ETW Keywords and Levels](clr-etw-keywords-and-levels.md)。）  
  
|引发事件的关键字|Level|  
|-----------------------------------|-----------|  
|`ExceptionKeyword` (0x8000)|警告 (2)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|在发生以下情况时引发|  
|-----------|--------------|-----------------|  
|`ExceptionThrown_V1`|80|引发托管异常。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|异常类型|win:UnicodeString|异常的类型，例如，`System.NullReferenceException`。|  
|异常消息|win:UnicodeString|实际的异常消息。|  
|EIPCodeThrow|win:Pointer|指向异常发生位置的指令指针。|  
|ExceptionHR|win:UInt32|异常 [HRESULT](/openspecs/windows_protocols/ms-erref/0642cb2f-2075-4469-918c-4441e69c548a)。|  
|ExceptionFlags|win:UInt16|0x01: HasInnerException（参阅 Visual Basic 文档中的 [CLR ETW 事件](clr-etw-events.md)）。<br /><br /> 0x02: IsNestedException。<br /><br /> 0x04: IsRethrownException。<br /><br /> 0x08： IsCorruptedStateException (指示进程状态已损坏;请参阅 [处理损坏状态异常](/archive/msdn-magazine/2009/february/clr-inside-out-handling-corrupted-state-exceptions)) 。<br /><br /> 0x10: IsCLSCompliant（从 <xref:System.Exception> 派生的异常符合 CLS，此外的其他异常均不符合 CLS）。|  
|ClrInstanceID|win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|  
  
## <a name="see-also"></a>另请参阅

- [CLR ETW 事件](clr-etw-events.md)
