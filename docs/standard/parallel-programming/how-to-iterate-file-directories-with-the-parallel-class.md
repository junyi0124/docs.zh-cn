---
title: 如何：使用并行类循环访问文件目录
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- parallel loops, how to iterate directories
ms.assetid: 555e9f48-f53d-4774-9bcf-3e965c732ec5
ms.openlocfilehash: 75097485c78e9ded67f41d9632f5399c081b3a16
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734460"
---
# <a name="how-to-iterate-file-directories-with-the-parallel-class"></a>如何：使用并行类循环访问文件目录

在许多情况下，文件迭代是可以轻松并行执行的操作。 主题[如何：使用 PLINQ 循环访问文件目录](how-to-iterate-file-directories-with-plinq.md)介绍了如何在许多情况下以最简单的方式执行此任务。 不过，如果代码必须处理访问文件系统时可能会出现的多种异常，可能会带来麻烦。 下面的示例展示了一种解决此问题的方法。 它使用基于堆栈的迭代遍历指定目录下的所有文件和文件夹，并让代码能够捕获和处理各种异常。 当然，如何处理异常还是取决于自己的选择。  
  
## <a name="example"></a>示例  

 下面的示例按顺序循环访问目录，但会并行处理文件。 这可能是文件与目录比很大时的最佳方法。 也可以并行执行目录迭代，并顺序访问每个文件。 并行执行两个循环的效率可能并不高，除非专门定目标到有大量处理器的计算机。 不过，与所有情况一样，应彻底测试应用，以确定最佳方法。  
  
 [!code-csharp[TPL_Parallel#08](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_parallel/cs/parallel_file.cs#08)]
 [!code-vb[TPL_Parallel#08](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_parallel/vb/fileiteration08.vb#08)]  
  
 在此示例中，文件 I/O 是同步执行。 若要处理大文件或网络连接速度慢，最好异步访问文件。 可以将异步 I/O 技术与并行迭代结合使用。 有关详细信息，请参阅 [TPL 和传统 .NET 异步编程](tpl-and-traditional-async-programming.md)。  
  
 此示例使用局部 `fileCount` 变量维护已处理的总文件数的计数。 由于多个任务可能并发访问此变量，可以调用 <xref:System.Threading.Interlocked.Add%2A?displayProperty=nameWithType> 方法来同步对它的访问。  
  
 请注意，如果主线程抛出异常，<xref:System.Threading.Tasks.Parallel.ForEach%2A> 方法启动的线程可能会继续运行。 若要停止这些线程，可以在异常处理程序中设置布尔变量，并在并行循环每次迭代时检查此变量的值。 如果此值指明异常已抛出，请使用 <xref:System.Threading.Tasks.ParallelLoopState> 变量停止或中断循环。 有关详细信息，请参阅[如何：停止或中断 Parallel.For 循环](/previous-versions/dotnet/netframework-4.0/dd460721(v=vs.100))。  
  
## <a name="see-also"></a>另请参阅

- [数据并行](data-parallelism-task-parallel-library.md)
