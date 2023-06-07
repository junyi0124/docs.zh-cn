---
title: 堆栈 ETW 事件
description: 阅读有关 stack ETW 事件的信息，该事件应与其他事件结合使用，以在引发事件后生成堆栈跟踪。
ms.date: 03/30/2017
helpviewer_keywords:
- stack event [.NET Framework]
- ETW, stack event (CLR)
ms.assetid: f612fa5b-4b62-4593-a19e-85c9b1018dce
ms.openlocfilehash: 3b890e587abd5cb1b7315fe41897f24638fd4604
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96236203"
---
# <a name="stack-etw-event"></a>堆栈 ETW 事件

堆栈事件应与其他事件结合使用，在引发事件后生成堆栈跟踪。 启用运行时提供程序时，记录该事件。 这是一个发生频率非常高的事件，因为每当引发另一个运行时事件时，都将引发此事件。 因此，我们建议谨慎使用此事件。  
  
 下表显示了关键字和级别。 （有关详细信息，请参阅 [CLR ETW Keywords and Levels](clr-etw-keywords-and-levels.md)。）  
  
|引发事件的关键字|Level|  
|-----------------------------------|-----------|  
|`StackKeyword` (0x40000000)|LogAlways(0)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|在发生以下情况时引发|  
|-----------|--------------|-----------------|  
|`CLRStackWalk`|82|与其他事件结合使用，在事件发生后生成堆栈跟踪。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|ClrInstanceID|win:Uint16|唯一运行时标识符。|  
|Reserved1|win:UInt8|保留。|  
|Reserved2|win:UInt8|保留。|  
|FrameCount|win:UInt32|堆栈跟踪中的帧数。|  
|堆栈|win:Pointer|指令指针的列|  
  
## <a name="see-also"></a>另请参阅

- [CLR ETW 事件](clr-etw-events.md)
