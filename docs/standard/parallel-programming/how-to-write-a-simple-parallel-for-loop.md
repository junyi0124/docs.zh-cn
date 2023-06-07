---
title: 如何：编写简单的 Parallel.For 循环
description: 了解如何编写 .NET 中的 Parallel.For 循环，在该循环中无需取消循环、中断循环迭代或维护任何线程本地状态。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Parallel.For, How to Write
- for loop, parallel construction in .NET
- parallel for loops, how to use
ms.assetid: 9029ba7f-a9d1-4526-8c84-c88716dba5d4
ms.openlocfilehash: 506d6dd725f5d42c6c445a14c5f450c815bfdde1
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94826628"
---
# <a name="how-to-write-a-simple-parallelfor-loop"></a>如何：编写简单的 Parallel.For 循环

本主题包含两个示例，这两个示例阐释了 <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> 方法。 第一个示例使用 <xref:System.Threading.Tasks.Parallel.For%28System.Int64%2CSystem.Int64%2CSystem.Action%7BSystem.Int64%7D%29?displayProperty=nameWithType> 方法重载，而第二个示例使用 <xref:System.Threading.Tasks.Parallel.For%28System.Int32%2CSystem.Int32%2CSystem.Action%7BSystem.Int32%7D%29?displayProperty=nameWithType> 重载，它们是 <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> 方法最简单的两个重载。 如果不需要取消循环、中断循环迭代或保持任何线程本地状态，则可以使用 <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> 方法的这两个重载。

> [!NOTE]
> 本文档使用 lambda 表达式在 TPL 中定义委托。 如果不熟悉 C# 或 Visual Basic 中的 lambda 表达式，请参阅 [PLINQ 和 TPL 中的 Lambda 表达式](lambda-expressions-in-plinq-and-tpl.md)。

第一个示例计算单个目录中文件的大小。 第二个示例计算两个矩阵的乘积。

## <a name="directory-size-example"></a>目录大小示例

本示例是一个简单的命令行实用工具，用于计算一个目录中的文件总大小。 它需要将单个目录路径作为参数，并报告该目录中文件的数量和总大小。 在验证目录存在后，它会使用 <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> 方法来枚举目录中的文件并确定其文件大小。 然后，将每个文件大小添加到 `totalSize` 变量。 请注意，此加法操作是通过调用 <xref:System.Threading.Interlocked.Add%2A?displayProperty=nameWithType> 来执行的，因此它是作为原子操作来执行的。 否则，多个任务可能会同时尝试更新 `totalSize` 变量。

[!code-csharp[Conceptual.Parallel.For#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.parallel.for/cs/for1.cs#1)]
[!code-vb[Conceptual.Parallel.For#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.parallel.for/vb/for1.vb#1)]

## <a name="matrix-and-stopwatch-example"></a>矩阵和秒表示例

本示例使用 <xref:System.Threading.Tasks.Parallel.For%2A?displayProperty=nameWithType> 方法来计算两个矩阵的乘积。 它还演示如何使用 <xref:System.Diagnostics.Stopwatch?displayProperty=nameWithType> 类来比较并行循环和非并行循环的性能。 请注意，由于本示例可能会生成大量输出，因此它允许将输出重定向到一个文件。

[!code-csharp[TPL_Parallel#01](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/simpleparallelfor.cs#01)]
[!code-vb[TPL_Parallel#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/simpleparallelfor.vb#01)]

在对任何代码（包括循环）进行并行化时，一个重要的目标是利用尽可能多的处理器，而不会过度并行化到并行处理的开销使任何性能优势消耗殆尽的程度。 在本特定示例中，只会对外部循环进行并行化，原因是不会在内部循环中执行太多工作。 少量工作和不良缓存影响的组合可能会导致嵌套并行循环的性能降低。 因此，仅并行化外部循环是在大多数系统上最大程度地发挥并发优势的最佳方式。

## <a name="the-delegate"></a>委托

<xref:System.Threading.Tasks.Parallel.For%2A> 的此重载的第三个参数是类型为 `Action<int>`（C# 中）或 `Action(Of Integer)`（Visual Basic 中）的委托。 不管 `Action` 委托具有零个、一个或十六个类型参数，它都始终返回 void。 在 Visual Basic 中，`Action` 的行为是用 `Sub` 定义的。 示例使用 lambda 表达式来创建委托，但也可以用其他方式创建委托。 有关详细信息，请参阅 [PLINQ 和 TPL 中的 Lambda 表达式](lambda-expressions-in-plinq-and-tpl.md)。

## <a name="the-iteration-value"></a>迭代值

委托采用其值为当前迭代的单一输入参数。 此迭代值由运行时提供，并且其起始值为正在当前线程上处理的源的片段（分区）上第一个元素的索引。

如果需要更好地控制并发级别，请使用采用 <xref:System.Threading.Tasks.ParallelOptions?displayProperty=nameWithType> 输入参数的重载之一，例如：<xref:System.Threading.Tasks.Parallel.For%28System.Int32%2CSystem.Int32%2CSystem.Threading.Tasks.ParallelOptions%2CSystem.Action%7BSystem.Int32%2CSystem.Threading.Tasks.ParallelLoopState%7D%29?displayProperty=nameWithType>。

## <a name="return-value-and-exception-handling"></a>返回值和异常处理

当所有线程均已完成时，<xref:System.Threading.Tasks.Parallel.For%2A> 会返回一个 <xref:System.Threading.Tasks.ParallelLoopResult?displayProperty=nameWithType> 对象。 当手动停止或中断循环迭代时，此返回值特别有用，因为 <xref:System.Threading.Tasks.ParallelLoopResult> 存储诸如完成运行的最后一个迭代等信息。 如果某个线程上出现一个或多个异常，则将会引发 <xref:System.AggregateException?displayProperty=nameWithType>。

在本示例的代码中，未使用 <xref:System.Threading.Tasks.Parallel.For%2A> 的返回值。

## <a name="analysis-and-performance"></a>分析和性能

可以使用性能向导来查看计算机上的 CPU 使用情况。 进行试验，增加矩阵中的列数和行数。 矩阵越大，并行计算和顺序计算之间的性能差异就越大。 当矩阵很小时，由于设置并行循环时会产生开销，因此顺序计算将运行更快。

同步调用共享资源（如控制台或文件系统）将大幅降低并行循环的性能。 在衡量性能时，请尝试避免在循环内进行诸如 <xref:System.Console.WriteLine%2A?displayProperty=nameWithType> 等调用。

## <a name="compile-the-code"></a>编译代码

将此代码复制并粘贴到 Visual Studio 项目中。

## <a name="see-also"></a>请参阅

- <xref:System.Threading.Tasks.Parallel.For%2A>
- <xref:System.Threading.Tasks.Parallel.ForEach%2A>
- [数据并行](data-parallelism-task-parallel-library.md)
- [并行编程](index.md)
