---
title: 如何：处理 PLINQ 查询中的异常
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, how to handle exceptions
ms.assetid: 8d56ff9b-a571-4d31-b41f-80c0b51b70a5
ms.openlocfilehash: aa583d8c31381d23e22f271db3d42d35e1fa14b2
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94826939"
---
# <a name="how-to-handle-exceptions-in-a-plinq-query"></a>如何：处理 PLINQ 查询中的异常

本主题中的第一个示例展示了如何处理 PLINQ 查询在执行时可能会抛出的 <xref:System.AggregateException?displayProperty=nameWithType>。 第二个示例展示了如何将 try-catch 块置于委托中，并尽可能靠近抛出异常的代码位置。 这样一来，可以在异常发生时尽快捕获它们，并继续执行查询。 如果允许异常向上冒泡回到联接线程，则查询也许可以在引发异常后继续处理一些项。

如果 PLINQ 回退到顺序执行且发生异常，异常可能会直接传播，而不会被包装在 <xref:System.AggregateException> 中。 此外，<xref:System.Threading.ThreadAbortException> 始终都会直接传播。

> [!NOTE]
> 选中“仅我的代码”后，Visual Studio 会在抛出异常的代码行处中断，并显示错误消息“用户代码未处理异常”。 此错误是良性的。 可以按 F5 继续运行，并请参阅下面示例中所示的异常处理行为。 为了阻止 Visual Studio 在第一个错误出现时中断，只需依次转到“工具”、“选项”、“调试”、“常规”  下，取消选中“仅我的代码”复选框即可。
>
> 本示例旨在演示用法，运行速度可能不如等效的顺序 LINQ to Objects 查询快。 若要详细了解加速，请参阅[了解 PLINQ 中的加速](understanding-speedup-in-plinq.md)。

## <a name="example"></a>示例

此示例展示了如何将 try-catch 块置于执行查询以捕获抛出的任何 <xref:System.AggregateException?displayProperty=nameWithType> 的代码附近。

[!code-csharp[PLINQ#41](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#41)]
[!code-vb[PLINQ#41](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinqsnippets1.vb#41)]

在此示例中，查询无法在异常抛出后继续运行。 在应用代码捕获到异常时，PLINQ 已停止对所有线程运行查询。

## <a name="example"></a>示例

下面的示例展示了如何将 try-catch 块置于委托中，以便可以捕获异常并继续执行查询。

[!code-csharp[PLINQ#42](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#42)]
[!code-vb[PLINQ#42](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinqsnippets1.vb#42)]

## <a name="compiling-the-code"></a>编译代码

- 若要编译和运行这些示例，请将它们复制到“PLINQ 数据样本”示例中，并通过 Main 调用此方法。

## <a name="robust-programming"></a>可靠编程

仅在确定如何处理异常时，才捕获异常，以免破坏程序状态。

## <a name="see-also"></a>另请参阅

- <xref:System.Linq.ParallelEnumerable>
- [并行 LINQ (PLINQ)](introduction-to-plinq.md)
