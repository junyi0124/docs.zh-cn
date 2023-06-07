---
title: 使用 foreach 删除 BlockingCollection 中的项
ms.date: 05/04/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- thread-safe collections, how to enumerate blocking collection
ms.assetid: 2096103c-22f7-420d-b631-f102bc33a6dd
ms.openlocfilehash: 9346ead4bf0aef91a224e0ab11ffd30a7c205294
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94830995"
---
# <a name="use-foreach-to-remove-items-in-a-blockingcollection"></a>使用 foreach 删除 BlockingCollection 中的项

除了使用 <xref:System.Collections.Concurrent.BlockingCollection%601.Take%2A> 和 <xref:System.Collections.Concurrent.BlockingCollection%601.TryTake%2A> 方法从 <xref:System.Collections.Concurrent.BlockingCollection%601> 中提取项之外，还可结合使用 [foreach](../../../csharp/language-reference/keywords/foreach-in.md)（在 Visual Basic 中为 [For Each](../../../visual-basic/language-reference/statements/for-each-next-statement.md)）和 <xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A?displayProperty=nameWithType> 删除项，直至添加完成且集合为空。 由于与典型的 `foreach` (`For Each`) 循环不同，此枚举器通过删除项来修改源集合，因此将其称作 *转变枚举* 或 *耗用枚举*。

## <a name="example"></a>示例

以下示例演示如何使用 `foreach` (`For Each`) 循环删除 <xref:System.Collections.Concurrent.BlockingCollection%601> 中的所有项。

[!code-csharp[CDS_BlockingCollection#03](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds_blockingcollection/cs/example03.cs#03)]
[!code-vb[CDS_BlockingCollection#03](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_blockingcollection/vb/enumeratebc.vb#03)]

此示例将 `foreach` 循环与耗用线程中的 <xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A?displayProperty=nameWithType> 方法结合使用，这会导致在枚举每个项时将其从集合中删除。 <xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=nameWithType> 随时限制集合中所包含的最大项数。 按照此方式枚举集合会在没有项可用或集合为空时阻止使用者线程。 在此示例中，由于制造者线程添加项的速度快于消耗项的速度，因此不需要考虑阻止问题。

<xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A?displayProperty=nameWithType> 返回了 `IEnumerable<T>`，因此无法保证顺序。 但是，在内部将 <xref:System.Collections.Concurrent.ConcurrentQueue%601?displayProperty=nameWithType> 用作基础集合类型，这将按照先进先出 (FIFO) 的顺序取消对象的排队。 如果对 <xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A?displayProperty=nameWithType> 进行了并发调用，这些调用会争用。 无法在一个枚举中观察到在另一个枚举中使用（已取消排队）的项目。

若要枚举集合而不对其进行修改，只需使用 `foreach` (`For Each`) 即可，无需使用 <xref:System.Collections.Concurrent.BlockingCollection%601.GetConsumingEnumerable%2A> 方法。 但是，务必要了解此类枚举表示的是某个精确时间点的集合快照。 如果其他线程在你执行循环的同时添加或删除项，则循环可能不会表示集合的实际状态。

## <a name="see-also"></a>请参阅

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- [并行编程](../../parallel-programming/index.md)
