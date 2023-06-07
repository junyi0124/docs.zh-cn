---
title: 被动回收
ms.date: 03/30/2017
helpviewer_keywords:
- garbage collection, forced
ms.assetid: 019008fe-4708-4e65-bebf-04fd9941e149
ms.openlocfilehash: 25e94221355569931a31b566a53434cbed9ea93f
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95714232"
---
# <a name="induced-collections"></a>被动回收

在大多数情况下，垃圾回收器可以确定执行回收的最佳时间，应让其独立运行。 在某些不常见的情况下，强制回收可以提高应用程序的性能。 在这种情况下，可以使用 <xref:System.GC.Collect%2A?displayProperty=nameWithType> 方法强制执行垃圾回收，从而诱导垃圾回收。  
  
 如果应用代码中特定点使用的内存量大量减少，请使用 <xref:System.GC.Collect%2A?displayProperty=nameWithType> 方法。 例如，如果应用使用包含多个控件的复杂对话框，那么在对话框关闭时调用 <xref:System.GC.Collect%2A> 可以立即回收对话框占用的内存，从而提升性能。 请确保应用程序不会过于频繁地引发垃圾回收，否则当垃圾回收器无效率地尝试回收对象时，可能会使性能降低。 可以向 <xref:System.GC.Collect%2A> 方法提供 <xref:System.GCCollectionMode.Optimized?displayProperty=nameWithType> 枚举值，以便仅在回收能够提高效率时才进行回收，如下一部分所述。  
  
## <a name="gc-collection-mode"></a>GC 回收模式  

 可以使用包含 <xref:System.GCCollectionMode> 值的 <xref:System.GC.Collect%2A?displayProperty=nameWithType> 方法重载之一，指定强制回收的行为，如下所示。  
  
|`GCCollectionMode` 值|说明|  
|------------------------------|-----------------|  
|<xref:System.GCCollectionMode.Default>|对正在运行的 .NET 版本使用默认的垃圾回收设置。|  
|<xref:System.GCCollectionMode.Forced>|强制立即执行垃圾回收。 这相当于调用 <xref:System.GC.Collect?displayProperty=nameWithType> 重载。 它会导致对所有分代进行完全阻塞回收。<br /><br /> 强制执行即时完全阻止式垃圾回收前，还可以将 <xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode%2A?displayProperty=nameWithType> 属性设置为 <xref:System.Runtime.GCLargeObjectHeapCompactionMode.CompactOnce?displayProperty=nameWithType>，从而压缩大型对象堆。|  
|<xref:System.GCCollectionMode.Optimized>|使垃圾回收器可以确定当前时间是否是回收对象的最佳时间。<br /><br /> 垃圾回收器可能判定回收效率不够高，因此回收不合理，在这种情况下将返回而不回收对象。|  
  
## <a name="background-or-blocking-collections"></a>后台回收或阻塞回收  

 可以调用 <xref:System.GC.Collect%28System.Int32%2CSystem.GCCollectionMode%2CSystem.Boolean%29?displayProperty=nameWithType> 方法重载，指定诱导回收是否是阻止式。 执行的回收类型取决于此方法的 `mode` 和 `blocking` 参数组合。 `mode` 是 <xref:System.GCCollectionMode> 枚举的成员，且 `blocking` 值为 <xref:System.Boolean>。 下表汇总了 `mode` 和 `blocking` 参数的交互。  
  
|`mode`|`blocking` = `true`|`blocking` = `false`|  
|------------|--------------------------|---------------------------|  
|<xref:System.GCCollectionMode.Forced> 或 <xref:System.GCCollectionMode.Default>|尽快执行阻塞回收。 如果后台回收正在进行且分代为 0 或 1，<xref:System.GC.Collect%28System.Int32%2CSystem.GCCollectionMode%2CSystem.Boolean%29> 方法会立即触发阻止式回收，并在回收完成后返回结果。 如果后台回收正在进行且 `generation` 参数为 2，此方法会等到后台回收完成，再触发第 2 代阻止式回收，然后返回结果。|尽快执行回收。 <xref:System.GC.Collect%28System.Int32%2CSystem.GCCollectionMode%2CSystem.Boolean%29> 方法请求执行后台回收，但这并没有保证；阻止式回收仍可执行，具体视环境而定。 如果后台回收正在进行，该方法将立即返回。|  
|<xref:System.GCCollectionMode.Optimized>|可能会执行阻止式回收，具体视垃圾回收器的状态和 `generation` 参数而定。 垃圾回收器会尽量提供最佳性能。|根据垃圾回收器的状态，有时可执行回收。 <xref:System.GC.Collect%28System.Int32%2CSystem.GCCollectionMode%2CSystem.Boolean%29> 方法请求执行后台回收，但这并没有保证；阻止式回收仍可执行，具体视环境而定。 垃圾回收器会尽量提供最佳性能。 如果后台回收正在进行，该方法将立即返回。|  
  
## <a name="see-also"></a>另请参阅

- [延迟模式](latency.md)
- [垃圾回收](index.md)
