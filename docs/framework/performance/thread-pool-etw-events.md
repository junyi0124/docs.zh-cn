---
title: 线程池 ETW 事件
description: 查看线程池 ETW 事件，这些事件收集有关 .NET 中的线程的信息。 线程池事件为工作线程池事件或 i/o 线程池事件。
ms.date: 03/30/2017
helpviewer_keywords:
- thread pool events [.NET Framework]
- ETW, thread pool events (CLR)
ms.assetid: f2a21e3a-3b6c-4433-97f3-47ff16855ecc
ms.openlocfilehash: 8b00c20e82ee1b1efa6a8a123e66a4cfc239143b
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96236164"
---
# <a name="thread-pool-etw-events"></a>线程池 ETW 事件

这些事件收集有关工作线程和 I/O 线程的信息。  
  
 存在两组线程池事件：  
  
- [工作线程池事件](#worker-thread-pool-events)：提供有关应用程序如何使用线程池以及工作负荷如何影响并发控制的信息。  
  
- [I/O 线程池事件](#io-thread-events)：提供有关线程池中创建、停用、恢复或终止的 I/O 线程的信息。  

## <a name="worker-thread-pool-events"></a>工作线程池事件

 这些事件与运行时工作线程池相关，并提供有关线程事件的通知（例如，创建或停止线程时）。 工作线程池使用自适应算法进行并发控制，其中的线程数基于测得的吞吐量来计算。 工作线程池事件可用于了解应用程序如何使用线程池，以及某些工作负荷可能会对并发控制怎样的影响。  
  
### <a name="threadpoolworkerthreadstart-and-threadpoolworkerthreadstop"></a>ThreadPoolWorkerThreadStart 和 ThreadPoolWorkerThreadStop  

 下表显示这些事件的关键字和级别。 （有关详细信息，请参阅 [CLR ETW Keywords and Levels](clr-etw-keywords-and-levels.md)。）  
  
|引发事件的关键字|Level|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|信息性 (4)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|在发生以下情况时引发|  
|-|-|-|  
|`ThreadPoolWorkerThreadStart`|50|创建工作线程。|  
|`ThreadPoolWorkerThreadStop`|51|停止工作线程。|  
|`ThreadPoolWorkerThreadRetirementStart`|52|停用工作线程。|  
|`ThreadPoolWorkerThreadRetirementStop`|53|停用的工作线程再次变为活动状态。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|ActiveWorkerThreadCount|win:UInt32|可用于处理工作的工作线程数，包括已在处理工作的工作线程。|  
|RetiredWorkerThreadCount|win:UInt32|不能用于处理工作但被保留以防之后需要更多线程的工作线程数。|  
|ClrInstanceID|Win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|  
  
### <a name="threadpoolworkerthreadadjustment"></a>ThreadPoolWorkerThreadAdjustment  

 这些线程池事件提供用于了解和调试线程注入（并发控制）算法行为的信息。 这些信息由工作线程池在内部使用。  
  
#### <a name="threadpoolworkerthreadadjustmentsample"></a>ThreadPoolWorkerThreadAdjustmentSample  

 下表显示了关键字和级别。  
  
|引发事件的关键字|Level|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|信息性 (4)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|说明|  
|-----------|--------------|-----------------|  
|`ThreadPoolWorkerThreadAdjustmentSample`|54|指一个示例的信息收集，即具有一定并发级别的即时吞吐量测量。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|吞吐量|win:Double|每个时间单位的完成数。|  
|ClrInstanceID|Win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|  
  
#### <a name="threadpoolworkerthreadadjustmentadjustment"></a>ThreadPoolWorkerThreadAdjustmentAdjustment  

 下表显示了关键字和级别。  
  
|引发事件的关键字|Level|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|信息性 (4)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|说明|  
|-----------|--------------|-----------------|  
|`ThreadPoolWorkerThreadAdjustmentAdjustment`|55|当线程注入（爬山）算法确定并发级别发生更改时，在控件中记录更改。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|AverageThroughput|win:Double|测量示例的平均吞吐量。|  
|NewWorkerThreadCount|win:UInt32|新的活动工作线程数。|  
|原因|win:UInt32|调整的原因。<br /><br /> 0x00 - 预热。<br /><br /> 0x01 - 初始化。<br /><br /> 0x02 - 随机移动。<br /><br /> 0x03 - 攀移。<br /><br /> 0x04 - 更改点。<br /><br /> 0x05 - 稳定。<br /><br /> 0x06 - 匮乏。<br /><br /> 0x07 - 线程已超时。|  
|ClrInstanceID|Win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|  
  
#### <a name="threadpoolworkerthreadadjustmentstats"></a>ThreadPoolWorkerThreadAdjustmentStats  

 下表显示了关键字和级别。  
  
|引发事件的关键字|Level|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|信息性 (4)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|说明|  
|-----------|--------------|-----------------|  
|`ThreadPoolWorkerThreadAdjustmentStats`|56|在线程池上收集数据。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|持续时间|win:Double|收集这些统计信息的时间量（以秒为单位）。|  
|吞吐量|win:Double|在此间隔期间每秒完成的平均数量。|  
|ThreadWave|win:Double|保留以供内部使用。|  
|ThroughputWave|win:Double|保留以供内部使用。|  
|ThroughputErrorEstimate|win:Double|保留以供内部使用。|  
|ThroughputErrorEstimate|win:Double|保留以供内部使用。|  
|ThroughputWave|win:Double|在此间隔期间因活动工作线程计数变化导致的相对吞吐量变化。|  
|置信度|win:Double|ThroughputRatio 字段的有效性测量。|  
|NewcontrolSetting|win:Double|将作为活动线程计数未来变化基线的活动工作线程数。|  
|NewThreadWaveMagnitude|Win:UInt16|活动线程计数的未来变化量值。|  
|ClrInstanceID|Win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|  

## <a name="io-thread-events"></a>I/O 线程事件  

 这些线程池事件针对 I/O 线程池（完成端口）中的线程发生，该过程是异步的。  
  
### <a name="iothreadcreate_v1"></a>IOThreadCreate_V1  

 下表显示了关键字和级别。  
  
|引发事件的关键字|Level|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|信息性 (4)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|在发生以下情况时引发|  
|-|-|-|  
|`IOThreadCreate_V1`|44|在线程池中创建 I/O 线程。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|计数|win:UInt64|I/O 线程数，包括新创建的线程。|  
|NumRetired|win:UInt64|已停用的工作线程数。|  
|ClrInstanceID|Win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|  
  
### <a name="iothreadretire_v1"></a>IOThreadRetire_V1  

 下表显示了关键字和级别。  
  
|引发事件的关键字|Level|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|信息性 (4)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|在发生以下情况时引发|  
|-----------|--------------|-----------------|  
|`IOThreadRetire_V1`|46|I/O 线程变为停用候选项。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|计数|win:UInt64|线程池中剩余的 I/O 线程数。|  
|NumRetired|win:UInt64|已停用的 I/O 线程数。|  
|ClrInstanceID|Win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|  
  
### <a name="iothreadunretire_v1"></a>IOThreadUnretire_V1  

 下表显示了关键字和级别。  
  
|引发事件的关键字|Level|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|信息性 (4)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|在发生以下情况时引发|  
|-----------|--------------|-----------------|  
|`IOThreadUnretire_V1`|47|I/O 线程因在该线程变为停用候选项后的等待期间内达到 I/O 而恢复使用。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|计数|win:UInt64|线程池中的 I/O 线程数，包括这一个。|  
|NumRetired|win:UInt64|已停用的 I/O 线程数。|  
|ClrInstanceID|Win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|  
  
### <a name="iothreadterminate"></a>IOThreadTerminate  

 下表显示了关键字和级别。  
  
|引发事件的关键字|Level|  
|-----------------------------------|-----------|  
|`ThreadingKeyword` (0x10000)|信息性 (4)|  
  
 下表显示了事件信息。  
  
|事件|事件 ID|在发生以下情况时引发|  
|-----------|--------------|-----------------|  
|`IOThreadTerminate`|45|线程池中的 i/o 线程终止。|  
  
 下表显示了事件数据。  
  
|字段名称|数据类型|说明|  
|----------------|---------------|-----------------|  
|计数|win:UInt64|线程池中剩余的 I/O 线程数。|  
|NumRetired|win:UInt64|已停用的 I/O 线程数。|  
|ClrInstanceID|Win:UInt16|CLR 或 CoreCLR 的实例的唯一 ID。|  
  
## <a name="see-also"></a>另请参阅

- [CLR ETW 事件](clr-etw-events.md)
