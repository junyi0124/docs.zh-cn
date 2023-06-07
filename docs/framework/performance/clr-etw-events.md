---
title: CLR ETW 事件
description: 请参阅有关公共语言运行时的文章 (CLR) Windows (ETW) 事件的事件跟踪。 有两个事件提供程序：运行时提供程序和断开提供程序。
ms.date: 03/30/2017
helpviewer_keywords:
- CLR ETW events
- ETW, common language runtime
- ETW, CLR events
ms.assetid: ef2b31c3-7426-43e7-9924-92339b96556d
ms.openlocfilehash: 8acc792b5217519e2a73c0cdf30c20161373c678
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96283908"
---
# <a name="clr-etw-events"></a>CLR ETW 事件

本部分的主题介绍 Windows (ETW) 事件的事件跟踪。 每个事件都有关联的关键字和级别，详见 [CLR ETW 关键字和级别](clr-etw-keywords-and-levels.md)主题。 CLR 有两个事件提供程序：  
  
- 运行时提供程序，根据启用的关键字（事件类别）引发事件。 CLR 运行时提供程序 GUID 是 e13c0d23-ccbc-4e12-931b-d9cc2eee27e4。  
  
- 断开提供程序，此提供程序具有特殊用途。 CLR 断开提供程序 GUID 是 a669021c-c450-4609-a035-5af59af4df18。  
  
 有关提供程序的详细信息，请参阅 [CLR ETW 提供程序](clr-etw-providers.md)。  
  
## <a name="in-this-section"></a>本节内容  

 [运行时信息事件](runtime-information-etw-events.md)  
 捕获有关运行时的信息，包括 SKU、版本号、激活运行时的方式、启动运行时所使用的命令行参数、GUID（如果适用），以及其他相关信息。  
  
 [异常 Thrown_V1 事件](exception-thrown-v1-etw-event.md)  
 捕获有关引发的异常的信息。  
  
 [争用事件](contention-etw-events.md)  
 捕获有关对运行时使用的监控视器锁或本机锁的争用情况的信息。  
  
 [线程池事件](thread-pool-etw-events.md)  
 捕获有关工作线程池和 I/O 线程池的信息。  
  
 [加载程序事件](loader-etw-events.md)  
 捕获有关加载和卸载应用程序域、程序集和模块的信息。  
  
 [方法事件](method-etw-events.md)  
 捕获有关用于符号解析的 CLR 方法的信息。  
  
 [垃圾回收事件](garbage-collection-etw-events.md)  
 捕获与诊断和调试中的垃圾回收和帮助有关的信息。  
  
 [JIT 跟踪事件](jit-tracing-etw-events.md)  
 捕获有关实时 (JIT) 内联和尾调用的信息。  
  
 [互操作事件](interop-etw-events.md)  
 捕获有关 Microsoft 中间语言 (MSIL) 存根生成和缓存的信息。  
  
 [ARM 事件](application-domain-resource-monitoring-arm-etw-events.md)  
 捕获有关应用程序域的状态的详细诊断信息。  
  
 [安全事件](security-etw-events.md)  
 捕获有关强名称和验证码验证的信息。  
  
 [堆栈事件](stack-etw-event.md)  
 捕获可用于其他事件以在引发事件后生成堆栈跟踪的信息。  
  
## <a name="see-also"></a>另请参阅

- [使用 ETW 改善调试和性能优化](/archive/msdn-magazine/2007/april/event-tracing-improve-debugging-and-performance-tuning-with-etw)
- [控制 .NET Framework 日志记录](controlling-logging.md)
- [CLR ETW 提供程序](clr-etw-providers.md)
- [CLR ETW 关键字和级别](clr-etw-keywords-and-levels.md)
- [公共语言运行时中的 ETW 事件](etw-events-in-the-common-language-runtime.md)
