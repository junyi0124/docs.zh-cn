---
title: 滞后时间模式
ms.date: 03/30/2017
helpviewer_keywords:
- garbage collection, intrusiveness
- garbage collection, latency modes
ms.assetid: 96278bb7-6eab-4612-8594-ceebfc887d81
ms.openlocfilehash: 2e7b30a50e2513c567abf2116ab5495e717a8e22
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94831190"
---
# <a name="latency-modes"></a>延迟模式

若要回收对象，垃圾回收器 (GC) 必须停止应用程序中所有正在执行的线程。 垃圾回收器处于活动状态的时间段称为延迟  。

在某些情况下（例如当应用程序检索数据或显示内容时），关键时刻可能发生完整的垃圾回收，从而妨碍性能。 可以通过将 <xref:System.Runtime.GCSettings.LatencyMode%2A?displayProperty=nameWithType> 属性设置为其中一个 <xref:System.Runtime.GCLatencyMode?displayProperty=nameWithType> 值来调节垃圾回收的干扰。

## <a name="low-latency-settings"></a>低延迟设置

使用“低”延迟设置意味着垃圾回收器对应用程序的干扰较少。 垃圾回收在回收内存方面较为保守。

<xref:System.Runtime.GCLatencyMode?displayProperty=nameWithType> 枚举提供两种低延迟设置：

- [GCLatencyMode.LowLatency](xref:System.Runtime.GCLatencyMode.LowLatency) 禁止第 2 代回收，仅执行第 0 代和第 1 代回收。 只能在短时间内使用。 在更长时间内，如果系统处于内存压力下，垃圾回收器将触发一次回收，这样会暂时暂停应用程序并中断对时间要求很急的操作。 此设置仅对工作站垃圾回收可用。

- [GCLatencyMode.SustainedLowLatency](xref:System.Runtime.GCLatencyMode.SustainedLowLatency) 禁止第 2 代前台回收，仅执行第 0 代、第 1 代回收和第 2 代后台回收。 它可以长时间使用，并对工作站和服务器垃圾回收都可用。 如果后台垃圾回收已禁用，则无法使用此设置。

在低延迟期间，除非发生以下情况，否则禁止第 2 代回收：

- 系统收到操作系统的低内存通知。

- 应用程序代码通过调用 <xref:System.GC.Collect%2A?displayProperty=nameWithType> 方法并将 `generation` 参数指定为 2 来包含回收。

## <a name="scenarios"></a>方案

下表列出了使用 <xref:System.Runtime.GCLatencyMode> 值的应用程序方案：

|延迟模式|应用程序方案|
|------------------|---------------------------|
|<xref:System.Runtime.GCLatencyMode.Batch>|对于不具有用户界面 (UI) 或服务器端操作的应用程序。<br /><br />禁用后台垃圾回收后，这将是工作站和服务器垃圾回收的默认模式。 <xref:System.Runtime.GCLatencyMode.Batch> 模式还会替代 [gcConcurrent](../../framework/configure-apps/file-schema/runtime/gcconcurrent-element.md) 设置，即它会阻止后台或并发回收。|
|<xref:System.Runtime.GCLatencyMode.Interactive>|对于具有 UI 的大多数应用程序。<br /><br />这是工作站和服务器垃圾回收的默认模式。 但是，如果托管了某个应用，则优先考虑托管进程的垃圾回收器设置。|
|<xref:System.Runtime.GCLatencyMode.LowLatency>|对于具有短期时效性操作（操作期间垃圾回收器的干扰可能会引起中断）的应用程序。 例如，呈现动画或数据采集功能的应用程序。|
|<xref:System.Runtime.GCLatencyMode.SustainedLowLatency>|适用于在有限但有可能更长的时间内具有时效性操作并且在此期间垃圾回收器中断具有破环性的应用程序。 例如，需要随着交易时间内的市场数据变化做出快速响应的应用程序。<br /><br />此模式会比其他模式产生更大的托管堆大小。 由于它不压缩托管堆，因此可能产生更多碎片。 确保有足够的可用内存。|

## <a name="guidelines-for-using-low-latency"></a>低延迟使用指南

使用 [GCLatencyMode.LowLatency](xref:System.Runtime.GCLatencyMode.LowLatency) 模式时，请注意以下指导原则：

- 尽可能地缩短低延迟时段。

- 避免在低延迟时段分配大量内存。 由于垃圾回收回收的对象较少可能出现低内存通知。

- 在低延迟模式下，最大限度减少新的分配次数，尤其是分配到大型对象堆和固定对象的次数。

- 知道可以分配的线程。 由于 <xref:System.Runtime.GCSettings.LatencyMode%2A> 属性设置属于进程范围的设置，因此可以在分配的任何线程上生成 <xref:System.OutOfMemoryException> 异常。

- 将低延迟代码包装在受约束的执行区域中。 有关详细信息，请参阅[受约束的执行区域](../../framework/performance/constrained-execution-regions.md)。

- 在低延迟期间，可以通过调用 <xref:System.GC.Collect%28System.Int32%2CSystem.GCCollectionMode%29?displayProperty=nameWithType> 方法强制进行第 2 代回收。

## <a name="see-also"></a>另请参阅

- <xref:System.GC?displayProperty=nameWithType>
- [已引发回收](induced.md)
- [垃圾回收](index.md)
