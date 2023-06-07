---
title: BlockingCollection 概述
description: 阅读有关 .NET 中线程安全集合类 BlockingCollection<T> 的信息。 此类提供一些功能，例如并发添加项或从多个线程中获取项。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- BlockingCollection, overview
ms.assetid: 987ea3d7-0ad5-4238-8b64-331ce4eb3f0b
ms.openlocfilehash: 550649ae8d5527b96e3a44edf44731c9c0586150
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733498"
---
# <a name="blockingcollection-overview"></a>BlockingCollection 概述

<xref:System.Collections.Concurrent.BlockingCollection%601> 是一个线程安全集合类，可提供以下功能：  
  
- 实现制造者-使用者模式。  
  
- 通过多线程并发添加和获取项。  
  
- 可选最大容量。  
  
- 集合为空或已满时通过插入和移除操作进行阻塞。  
  
- 插入和移除“尝试”操作不发生阻塞，或在指定时间段内发生阻塞。  
  
- 封装实现 <xref:System.Collections.Concurrent.IProducerConsumerCollection%601> 的任何集合类型  
  
- 使用取消标记执行取消操作。  
  
- 支持使用 `foreach`（在 Visual Basic 中，使用 `For Each`）的两种枚举：  
  
    1. 只读枚举。  
  
    2. 在枚举项时将项移除的枚举。  
  
## <a name="bounding-and-blocking-support"></a>限制和阻塞支持  

 <xref:System.Collections.Concurrent.BlockingCollection%601> 支持限制和阻塞。 限制意味着可以设置集合的最大容量。 限制在某些情况中很重要，因为它使你能够控制内存中的集合的最大大小，并可阻止制造线程移动到离使用线程前方太远的位置。  
  
 多个线程或任务可同时向集合添加项，如果集合达到其指定最大容量，则制造线程将发生阻塞，直到移除集合中的某个项。 多个使用者可以同时移除项，如果集合变空，则使用线程将发生阻塞，直到制造者添加某个项。 制造线程可调用 <xref:System.Collections.Concurrent.BlockingCollection%601.CompleteAdding%2A> 来指示不再添加项。 使用者将监视 <xref:System.Collections.Concurrent.BlockingCollection%601.IsCompleted%2A> 属性以了解集合何时为空且不再添加项。 下面的示例展示了容量上限为 100 的简单 BlockingCollection。 只要满足某些外部条件为 true，制造者任务就会向集合添加项，然后调用 <xref:System.Collections.Concurrent.BlockingCollection%601.CompleteAdding%2A>。 使用者任务获取项，直到 <xref:System.Collections.Concurrent.BlockingCollection%601.IsCompleted%2A> 属性为 true。  
  
 [!code-csharp[CDS_BlockingCollection#04](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds_blockingcollection/cs/blockingcollection.cs#04)]
 [!code-vb[CDS_BlockingCollection#04](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_blockingcollection/vb/introsnippetsbc.vb#04)]  
  
 有关完整的示例，请参阅 [如何：在 BlockingCollection 中逐个添加和取出项](how-to-add-and-take-items.md)。  
  
## <a name="timed-blocking-operations"></a>计时阻塞操作  

 在针对有限集合的计时阻塞 <xref:System.Collections.Concurrent.BlockingCollection%601.TryAdd%2A> 和 <xref:System.Collections.Concurrent.BlockingCollection%601.TryTake%2A> 操作中，此方法将尝试添加或取出某个项。 如果项可用，项会被置于通过引用传入的变量中，然后方法返回 true。 如果在指定的超时期限过后未检索到任何项，方法返回 false。 相应线程可以任意执行一些其他有用的工作，然后再重新尝试访问该集合。 有关计时阻塞访问的示例，请参阅[如何：在 BlockingCollection 中逐个添加和取出项](how-to-add-and-take-items.md)中的第二个示例。  
  
## <a name="cancelling-add-and-take-operations"></a>取消添加和取出操作  

 添加和取出操作通常会在一个循环内执行。 可以通过以下方法来取消循环：向 <xref:System.Collections.Concurrent.BlockingCollection%601.TryAdd%2A> 或 <xref:System.Collections.Concurrent.BlockingCollection%601.TryTake%2A> 方法传入 <xref:System.Threading.CancellationToken>，然后在每次迭代时检查该标记的 <xref:System.Threading.CancellationToken.IsCancellationRequested%2A> 属性的值。 如果值为 true，由你自行决定是否通过清理所有资源并退出循环来响应取消请求。 下面的示例演示获取取消标记和使用该标记的代码的 <xref:System.Collections.Concurrent.BlockingCollection%601.TryAdd%2A> 重载：  
  
 [!code-csharp[CDS_BlockingCollection#05](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds_blockingcollection/cs/blockingcollection.cs#05)]
 [!code-vb[CDS_BlockingCollection#05](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_blockingcollection/vb/introsnippetsbc.vb#05)]  
  
 有关如何添加取消支持的示例，请参阅[如何：在 BlockingCollection 中逐个添加和取出项](how-to-add-and-take-items.md)中的第二个示例。  
  
## <a name="specifying-the-collection-type"></a>指定集合类型  

 创建 <xref:System.Collections.Concurrent.BlockingCollection%601> 时，不仅可以指定上限容量，而且可以指定要使用的集合类型。 例如，可为先进先出 (FIFO) 行为指定 <xref:System.Collections.Concurrent.ConcurrentQueue%601>，也可为后进先出 (LIFO) 行为指定 <xref:System.Collections.Concurrent.ConcurrentStack%601>。 可使用实现 <xref:System.Collections.Concurrent.IProducerConsumerCollection%601> 接口的任何集合类。 <xref:System.Collections.Concurrent.BlockingCollection%601> 的默认集合类型为 <xref:System.Collections.Concurrent.ConcurrentQueue%601>。 下面的代码示例演示如何创建字符串的 <xref:System.Collections.Concurrent.BlockingCollection%601>，其容量为 1000 并使用 <xref:System.Collections.Concurrent.ConcurrentBag%601>：  
  
```vb  
Dim bc = New BlockingCollection(Of String)(New ConcurrentBag(Of String()), 1000)  
```  
  
```csharp  
BlockingCollection<string> bc = new BlockingCollection<string>(new ConcurrentBag<string>(), 1000 );  
```  
  
 有关详细信息，请参阅[如何：向集合添加限制和阻塞功能](how-to-add-bounding-and-blocking.md)。  
  
## <a name="ienumerable-support"></a>IEnumerable 支持  

 <xref:System.Collections.Concurrent.BlockingCollection%601> 提供 <xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A> 方法，该方法允许使用者使用 `foreach`（在 Visual Basic 中为 `For Each`）删除项直到完成集合，也就是说，集合为空且不再添加项。 有关详细信息，请参阅[如何：使用 ForEach 移除 BlockingCollection 中的项](how-to-use-foreach-to-remove.md)。  
  
## <a name="using-many-blockingcollections-as-one"></a>将多个 BlockingCollection 作为整体使用  

 在使用者需要同时取出多个集合中的项的情况下，可以创建 <xref:System.Collections.Concurrent.BlockingCollection%601> 的数组并使用静态方法，如 <xref:System.Collections.Concurrent.BlockingCollection%601.TakeFromAny%2A> 和 <xref:System.Collections.Concurrent.BlockingCollection%601.AddToAny%2A> 方法，这两个方法可以在该数组的任意集合中添加或取出项。 如果一个集合发生阻塞，此方法会立即尝试其他集合，直到找到能够执行该操作的集合。 有关详细信息，请参阅[如何：在管道中使用阻塞集合的数组](how-to-use-arrays-of-blockingcollections.md)。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- [集合和数据结构](../index.md)
- [线程安全集合](index.md)
