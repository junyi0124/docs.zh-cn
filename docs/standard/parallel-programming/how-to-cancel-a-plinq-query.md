---
title: 如何：取消 PLINQ 查询
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, how to cancel
- cancellation, PLINQ
ms.assetid: 80b14640-edfa-4153-be1b-3e003d3e9c1a
ms.openlocfilehash: f8ca723ca3b9b9428c95651ebe2fa7d92e48ebbb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95713283"
---
# <a name="how-to-cancel-a-plinq-query"></a>如何：取消 PLINQ 查询

下面的示例展示了取消 PLINQ 查询的两种方法。 第一个示例展示了如何取消主要由数据遍历组成的查询。 第二个示例展示了如何取消包含计算成本很高的用户函数的查询。

> [!NOTE]
> 选中“仅我的代码”后，Visual Studio 会在抛出异常的代码行处中断，并显示错误消息“用户代码未处理异常”。 此错误是良性的。 可以按 F5 继续运行，并请参阅下面示例中所示的异常处理行为。 为了阻止 Visual Studio 在第一个错误出现时中断，只需依次转到“工具”、“选项”、“调试”、“常规”  下，取消选中“仅我的代码”复选框即可。
>
> 本示例旨在演示用法，运行速度可能不如等效的顺序 LINQ to Objects 查询快。 若要详细了解加速，请参阅[了解 PLINQ 中的加速](understanding-speedup-in-plinq.md)。

## <a name="example"></a>示例

[!code-csharp[PLINQ#16](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#16)]
[!code-vb[PLINQ#16](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinqsnippets1.vb#16)]

PLINQ 框架不会将一个 <xref:System.OperationCanceledException> 滚动到 <xref:System.AggregateException?displayProperty=nameWithType> 中；必须在单独的 catch 块中处理 <xref:System.OperationCanceledException>。 如果一个或多个用户委托抛出 OperationCanceledException(externalCT)（通过使用外部 <xref:System.Threading.CancellationToken?displayProperty=nameWithType>），但没有其他任何异常，且查询被定义为 `AsParallel().WithCancellation(externalCT)`，那么 PLINQ 会抛出 <xref:System.OperationCanceledException> (externalCT)，而不是 <xref:System.AggregateException?displayProperty=nameWithType>。 不过，如果一个用户委托抛出 <xref:System.OperationCanceledException>，另一个委托抛出另一种类型的异常，那么这两个异常都会滚动到 <xref:System.AggregateException> 中。

关于取消的一般性指南如下：

1. 如果执行用户委托取消，应将外部 <xref:System.Threading.CancellationToken> 告知给 PLINQ，并引发 <xref:System.OperationCanceledException>(externalCT)。

2. 如果发生取消且没有引发其他任何异常，则处理 <xref:System.OperationCanceledException>，而不是 <xref:System.AggregateException>。

## <a name="example"></a>示例

下面的示例展示了如何在用户代码中使用计算成本高的函数时处理取消。

[!code-csharp[PLINQ#17](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#17)]
[!code-vb[PLINQ#17](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinqsnippets1.vb#17)]

在用户代码中处理取消时，无需在查询定义中使用 <xref:System.Linq.ParallelEnumerable.WithCancellation%2A>。 不过，之所以建议使用 <xref:System.Linq.ParallelEnumerable.WithCancellation%2A> 是因为，<xref:System.Linq.ParallelEnumerable.WithCancellation%2A> 对查询性能没有影响，并让取消由查询运算符和用户代码进行处理。

为了确保系统响应速度，建议每毫秒检查一次取消；不过，只要不超过每 10 毫秒一次，任何频率都认为是可接受的。 此频率不得对代码性能产生不利影响。

如果枚举器已遭清理（例如，当代码跳出循环访问查询结果的 foreach（Visual Basic 中的 For Each）循环时），查询就会被取消，但不会引发异常。

## <a name="see-also"></a>请参阅

- <xref:System.Linq.ParallelEnumerable>
- [并行 LINQ (PLINQ)](introduction-to-plinq.md)
- [托管线程中的取消](../threading/cancellation-in-managed-threads.md)
