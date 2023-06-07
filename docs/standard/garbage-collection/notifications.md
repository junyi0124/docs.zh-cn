---
title: 垃圾回收通知
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- garbage collection, notifications
ms.assetid: e12d8e74-31e3-4035-a87d-f3e66f0a9b89
ms.openlocfilehash: 70343851ba73af9041014e8654f5df82d8389c39
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734772"
---
# <a name="garbage-collection-notifications"></a>垃圾回收通知

在有些情况下，公共语言运行时执行的完整垃圾回收（即第 2 代回收）可能会对性能产生负面影响。 特别是，处理大量请求的服务器可能会出现此问题；在这种情况下，长时间垃圾回收会导致请求超时。为了防止在关键时期发生完全回收，可以接收即将执行完全垃圾回收的通知，再采取措施将工作负载重定向到另一个服务器实例。 也可以自行诱导回收，前提是当前服务器实例不需要处理请求。  
  
 <xref:System.GC.RegisterForFullGCNotification%2A> 方法注册为，在运行时检测到即将执行完全垃圾回收时发出通知。 此通知分为两个部分：完全垃圾回收何时即将执行，以及完全垃圾回收何时完成。  
  
> [!WARNING]
> 只有阻止垃圾回收会引发通知。 如果 [\<gcConcurrent>](../../framework/configure-apps/file-schema/runtime/gcconcurrent-element.md) 配置元素已启用，后台垃圾回收不会发出通知。  
  
 若要确定何时发出通知，请使用 <xref:System.GC.WaitForFullGCApproach%2A> 和 <xref:System.GC.WaitForFullGCComplete%2A> 方法。 通常，在 `while` 循环中使用这些方法，以持续获取表示通知状态的 <xref:System.GCNotificationStatus> 枚举。 如果值为 <xref:System.GCNotificationStatus.Succeeded>，可以执行下列操作：  
  
- 为了响应使用 <xref:System.GC.WaitForFullGCApproach%2A> 方法获得的通知，可以重定向工作负载，并能自行诱导回收。  
  
- 为了响应使用 <xref:System.GC.WaitForFullGCComplete%2A> 方法获得的通知，可以让当前服务器实例再次用于处理请求。 也可以收集信息。 例如，可以使用 <xref:System.GC.CollectionCount%2A> 方法记录回收次数。  
  
 <xref:System.GC.WaitForFullGCApproach%2A> 和 <xref:System.GC.WaitForFullGCComplete%2A> 方法要配合使用。 使用一个方法，而不使用另一个方法，可能会生成不确定的结果。  
  
## <a name="full-garbage-collection"></a>完全垃圾回收  

 如果发生下列任一情况，运行时就会执行完全垃圾回收：  
  
- 足够多的内存已提升到第 2 代，导致执行下一个第 2 代回收。  
  
- 足够多的内存已提升到大型对象堆，导致执行下一个第 2 代回收。  
  
- 由于其他因素，导致第 1 代回收升级为第 2 代回收。  
  
 在 <xref:System.GC.RegisterForFullGCNotification%2A> 方法中指定的阈值适用于前两种情况。 不过，在第一种情况下，不一定会在与指定的阈值相称的时间收到通知，原因有下面两个：  
  
- 运行时不检查每个小型对象分配（出于性能考虑）。  
  
- 只有第 1 代回收将内存提升到第 2 代。  
  
 第三种情况也加剧了通知接收时间的不确定性。 可以在此期间重定向请求，或在可以更好适应时自行诱导回收，从而减轻不合时宜的完全垃圾回收造成的影响。尽管并不保证有效，但确实证明这是非常实用的方法。  
  
## <a name="notification-threshold-parameters"></a>通知阈值参数  

 <xref:System.GC.RegisterForFullGCNotification%2A> 方法包含两个参数，用于指定第 2 代对象和大型对象堆的阈值。 如果达到这些值，就应发出垃圾回收通知。 下表介绍了这些参数。  
  
|参数|描述|  
|---------------|-----------------|  
|`maxGenerationThreshold`|介于 1 和 99 之间的数字，指定根据在第 2 代中提升的对象，应何时发出通知。|  
|`largeObjectHeapThreshold`|介于 1 和 99 之间的数字，指定根据大型对象堆中分配的对象，应何时发出通知。|  
  
 如果指定的值过高，很可能会出现的情况是，将会收到通知，但在运行时执行回收前等待的时间太长。 如果自行诱导回收，回收的对象可能会多于在运行时执行回收时回收的对象。  
  
 如果指定的值过低，在运行时执行回收前等待通知的时间可能不够长。  
  
## <a name="example"></a>示例  
  
### <a name="description"></a>描述  

 在下面的示例中，一组服务器处理传入的 Web 请求。 为了模拟处理请求的工作负载，将字节数组添加到 <xref:System.Collections.Generic.List%601> 集合中。 每个服务器都会注册获取垃圾回收通知，再对 `WaitForFullGCProc` 用户方法启动线程，以持续监视 <xref:System.GC.WaitForFullGCApproach%2A> 和 <xref:System.GC.WaitForFullGCComplete%2A> 方法返回的 <xref:System.GCNotificationStatus> 枚举。  
  
 在通知发出时，<xref:System.GC.WaitForFullGCApproach%2A> 和 <xref:System.GC.WaitForFullGCComplete%2A> 方法调用它们各自的事件处理用户方法：  
  
- `OnFullGCApproachNotify`  
  
     此方法调用 `RedirectRequests` 用户方法，指示请求队列服务器暂停向服务器发送请求。 具体模拟方式是，将类级别变量 `bAllocate` 设置为 `false`，这样就不会再分配对象。  
  
     接下来，调用 `FinishExistingRequests` 用户方法，完成处理挂起的服务器请求。 具体模拟方式是，清除 <xref:System.Collections.Generic.List%601> 集合。  
  
     最后，由于工作负载很轻，诱导垃圾回收。  
  
- `OnFullGCCompleteNotify`  
  
     此方法调用用户方法 `AcceptRequests` 以继续接受请求，因为服务器不再易受完全垃圾回收影响。 此操作的具体模拟方式是，将 `bAllocate` 变量设置为 `true`，以便能够继续将对象添加到 <xref:System.Collections.Generic.List%601> 集合。  
  
 下面的代码包含示例的 `Main` 方法。  
  
 [!code-cpp[GCNotification#2](../../../samples/snippets/cpp/VS_Snippets_CLR/GCNotification/cpp/program.cpp#2)]
 [!code-csharp[GCNotification#2](../../../samples/snippets/csharp/VS_Snippets_CLR/GCNotification/cs/Program.cs#2)]
 [!code-vb[GCNotification#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GCNotification/vb/program.vb#2)]  
  
 下面的代码包含 `WaitForFullGCProc` 用户方法，其中包括持续 while 循环，用于检查是否有垃圾回收通知。  
  
 [!code-cpp[GCNotification#8](../../../samples/snippets/cpp/VS_Snippets_CLR/GCNotification/cpp/program.cpp#8)]
 [!code-csharp[GCNotification#8](../../../samples/snippets/csharp/VS_Snippets_CLR/GCNotification/cs/Program.cs#8)]
 [!code-vb[GCNotification#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GCNotification/vb/program.vb#8)]  
  
 下面的代码包含 `OnFullGCApproachNotify` 方法，调用自  
  
 `WaitForFullGCProc` 方法。  
  
 [!code-cpp[GCNotification#5](../../../samples/snippets/cpp/VS_Snippets_CLR/GCNotification/cpp/program.cpp#5)]
 [!code-csharp[GCNotification#5](../../../samples/snippets/csharp/VS_Snippets_CLR/GCNotification/cs/Program.cs#5)]
 [!code-vb[GCNotification#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GCNotification/vb/program.vb#5)]  
  
 下面的代码包含 `OnFullGCApproachComplete` 方法，调用自  
  
 `WaitForFullGCProc` 方法。  
  
 [!code-cpp[GCNotification#6](../../../samples/snippets/cpp/VS_Snippets_CLR/GCNotification/cpp/program.cpp#6)]
 [!code-csharp[GCNotification#6](../../../samples/snippets/csharp/VS_Snippets_CLR/GCNotification/cs/Program.cs#6)]
 [!code-vb[GCNotification#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GCNotification/vb/program.vb#6)]  
  
 下面的代码包含调用自 `OnFullGCApproachNotify` 和 `OnFullGCCompleteNotify` 方法的用户方法。 用户方法重定向请求，完成现有请求，再在发生完全垃圾回收后继续执行请求。  
  
 [!code-cpp[GCNotification#9](../../../samples/snippets/cpp/VS_Snippets_CLR/GCNotification/cpp/program.cpp#9)]
 [!code-csharp[GCNotification#9](../../../samples/snippets/csharp/VS_Snippets_CLR/GCNotification/cs/Program.cs#9)]
 [!code-vb[GCNotification#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GCNotification/vb/program.vb#9)]  
  
 整个代码示例如下所示：  
  
 [!code-cpp[GCNotification#1](../../../samples/snippets/cpp/VS_Snippets_CLR/GCNotification/cpp/program.cpp#1)]
 [!code-csharp[GCNotification#1](../../../samples/snippets/csharp/VS_Snippets_CLR/GCNotification/cs/Program.cs#1)]
 [!code-vb[GCNotification#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GCNotification/vb/program.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [垃圾回收](index.md)
