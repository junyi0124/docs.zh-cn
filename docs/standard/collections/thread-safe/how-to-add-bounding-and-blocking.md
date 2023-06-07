---
title: 如何：向集合添加限制和阻塞功能
ms.date: 03/30/2017
helpviewer_keywords:
- thread-safe collections, custom blocking collections
ms.assetid: 4c2492de-3876-4873-b5a1-000bb404d770
ms.openlocfilehash: ba41176d87779fe673fa162d0bf04c10414cdd92
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733472"
---
# <a name="how-to-add-bounding-and-blocking-functionality-to-a-collection"></a>如何：向集合添加限制和阻塞功能

本示例演示如何通过实现类中的 <xref:System.Collections.Concurrent.IProducerConsumerCollection%601?displayProperty=nameWithType> 接口，然后将类实例用作 <xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=nameWithType> 的内部存储机制，来向自定义集合类添加限制和阻塞功能。 有关限制和阻塞的详细信息，请参阅 [BlockingCollection 概述](blockingcollection-overview.md)。  
  
## <a name="example"></a>示例  

 自定义集合类是一个基本优先级别队列，优先级别在其中表示为 <xref:System.Collections.Concurrent.ConcurrentQueue%601?displayProperty=nameWithType> 对象的数组。 在每个队列中不进行其他排序。  
  
 客户端代码中启动了三个任务。 第一个任务仅轮询键盘键击以便在执行过程中的任意时刻启用取消。 第二个任务是制造者线程；它会向阻塞集合添加新项并根据随机值为每个项分配一个优先级。 第三个任务在项可用时将其从集合中移除。  
  
 可以通过使其中一个线程运行速度快于另一个线程来调整应用程序的行为。 如果制造者运行速度更快，则在阻塞集合阻止添加项（如果该集合已包含构造函数中所指定的项数）时，你会注意到限制功能。 如果使用者运行速度更快，则在使用者等待添加新项时，你会注意到阻塞功能。  
  
 [!code-csharp[CDS_BlockingCollection#06](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds_blockingcollection/cs/prodcon.cs#06)]  
  
 默认情况下，<xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=nameWithType> 的存储为 <xref:System.Collections.Concurrent.ConcurrentQueue%601?displayProperty=nameWithType>。  
  
## <a name="see-also"></a>另请参阅

- [线程安全集合](index.md)
