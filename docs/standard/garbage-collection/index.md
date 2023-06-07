---
title: .NET 垃圾回收
description: 了解 .NET 中的垃圾回收。 .NET 垃圾回收器管理应用程序的内存分配和释放。
ms.date: 04/21/2020
helpviewer_keywords:
- memory, garbage collection
- garbage collection, automatic memory management
- GC [.NET]
- memory, allocating
- common language runtime, garbage collection
- garbage collector
- cleanup operations
- garbage collection
- memory, releasing
- common language runtime, automatic memory management
- automatic memory management
- runtime, automatic memory management
- runtime, garbage collection
- garbage collection, about
ms.assetid: 22b6cb97-0c80-4eeb-a2cf-5ed7655e37f9
ms.openlocfilehash: 8b1fad3420778c17656614994684930fcd1b62ca
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94827771"
---
# <a name="garbage-collection"></a>垃圾回收

.NET 的垃圾回收器管理应用程序的内存分配和释放。 每当有对象新建时，公共语言运行时都会从托管堆为对象分配内存。 只要托管堆中有地址空间，运行时就会继续为新对象分配空间。 不过，内存并不是无限的。 垃圾回收器最终必须执行垃圾回收来释放一些内存。 垃圾回收器的优化引擎会根据所执行的分配来确定执行回收的最佳时机。 执行回收时，垃圾回收器会在托管堆中检查应用程序不再使用的对象，然后执行必要的操作来回收其内存。  
  
## <a name="in-this-section"></a>本节内容
  
|Title|描述|  
|-----------|-----------------|  
|[垃圾回收的基本知识](fundamentals.md)|描述垃圾回收的工作原理、如何在托管堆上分配对象，以及其他核心概念。|  
|[工作站和服务器垃圾回收](workstation-server-gc.md)|描述了客户端应用的工作站垃圾回收与服务器应用的服务器垃圾回收之间的区别。|
|[后台垃圾回收](background-gc.md)|描述了后台垃圾回收，它是在进行第二代回收时对第 0 代和第 1 代对象的回收。|
|[大型对象堆](large-object-heap.md)|描述了大型对象堆 (LOH) 及其垃圾回收方式。|
|[垃圾回收和性能](performance.md)|介绍了可用来诊断垃圾回收和性能问题的性能检查。|  
|[已引发回收](induced.md)|描述如何完成垃圾回收。|  
|[延迟模式](latency.md)|描述确定垃圾回收侵入性的模式。|  
|[针对共享 Web 承载优化](optimization-for-shared-web-hosting.md)|介绍了如何在多个小网站共用的服务器上优化垃圾回收。|  
|[垃圾回收通知](notifications.md)|介绍了如何确定全面垃圾回收的开始时间和结束时间。|  
|[应用程序域资源监视](app-domain-resource-monitoring.md)|介绍了如何监视应用程序域的 CPU 和内存使用情况。|  
|[弱引用](weak-references.md)|描述允许应用程序访问对象的同时也允许垃圾回收器收集相应对象的功能。|  
  
## <a name="reference"></a>参考

- <xref:System.GC?displayProperty=nameWithType>  
- <xref:System.GCCollectionMode?displayProperty=nameWithType>  
- <xref:System.GCNotificationStatus?displayProperty=nameWithType>  
- <xref:System.Runtime.GCLatencyMode?displayProperty=nameWithType>  
- <xref:System.Runtime.GCSettings?displayProperty=nameWithType>  
- <xref:System.Runtime.GCSettings.LargeObjectHeapCompactionMode%2A?displayProperty=nameWithType>  
- <xref:System.Object.Finalize%2A?displayProperty=nameWithType>  
- <xref:System.IDisposable?displayProperty=nameWithType>  
  
## <a name="see-also"></a>请参阅

- [清理非托管资源](unmanaged.md)
