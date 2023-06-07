---
title: 如何：在 BlockingCollection 中逐个添加和取出项
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- thread-safe collections, blocking dictionary
ms.assetid: 38f2f3d8-15e5-4bf4-9c83-2b5b6f22bad1
ms.openlocfilehash: de65dbdcc3239016a16bc12f9254bb99637e45b4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733485"
---
# <a name="how-to-add-and-take-items-individually-from-a-blockingcollection"></a>如何：在 BlockingCollection 中逐个添加和取出项

此示例展示了如何以阻止性和非阻止性方式在 <xref:System.Collections.Concurrent.BlockingCollection%601> 中添加和删除项。 有关 <xref:System.Collections.Concurrent.BlockingCollection%601> 的详细信息，请参阅 [BlockingCollection 概述](blockingcollection-overview.md)。  
  
 有关如何枚举 <xref:System.Collections.Concurrent.BlockingCollection%601> 直至其为空且不再添加更多元素的示例，请参阅[如何：使用 ForEach 移除 BlockingCollection 中的项](how-to-use-foreach-to-remove.md)。
  
## <a name="example"></a>示例  

 第一个示例展示了如何添加和取出项，以便在集合暂时为空（取出时）或达到最大容量（添加时），或超过指定超时期限时，阻止相应操作。 注意，仅当已创建 BlockingCollection 且构造函数中指定了最大容量时，才会启用在达到最大容量时进行阻止的功能。  
  
 [!code-csharp[CDS_BlockingCollection#01](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds_blockingcollection/cs/example01.cs#01)]
 [!code-vb[CDS_BlockingCollection#01](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_blockingcollection/vb/simpleblocking.vb#01)]  
  
## <a name="example"></a>示例  

 第二个示例演示如何添加和取出项以便不会阻止操作。 如果没有任何项、已达到绑定集合的最大容量或已超过超时期限，<xref:System.Collections.Concurrent.BlockingCollection%601.TryAdd%2A> 或 <xref:System.Collections.Concurrent.BlockingCollection%601.TryTake%2A> 操作返回 false。 这样一来，线程可以暂时执行其他一些有用的工作，并在稍后再次尝试检索新项，或尝试添加先前无法添加的相同项。 该程序还演示如何在访问 <xref:System.Collections.Concurrent.BlockingCollection%601> 时实现取消。  
  
 [!code-csharp[CDS_BlockingCollection#02](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds_blockingcollection/cs/example02.cs#02)]
 [!code-vb[CDS_BlockingCollection#02](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_blockingcollection/vb/nonblockingbc.vb#02)]  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- [BlockingCollection 概述](blockingcollection-overview.md)
