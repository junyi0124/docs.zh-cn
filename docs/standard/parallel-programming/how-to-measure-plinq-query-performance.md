---
title: 如何：衡量 PLINQ 查询性能
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, how to measure performance
ms.assetid: 491ba43b-2c10-473d-9aab-e2cb96446711
ms.openlocfilehash: 2cbd178d5004d28120ab701a777a474a7e78e09e
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734434"
---
# <a name="how-to-measure-plinq-query-performance"></a>如何：衡量 PLINQ 查询性能

此示例展示了如何使用 <xref:System.Diagnostics.Stopwatch> 类度量 PLINQ 查询的执行时间。  
  
## <a name="example"></a>示例  

 此示例使用空 `foreach` 循环（Visual Basic 中为 `For Each`）来衡量查询执行所用的时间。 在实际代码中，循环通常会包含其他处理步骤，这会增加查询总执行时间。 请注意，秒表会恰好在循环开始之前启动，因为此时查询开始执行。 如果需要更精细的度量，可以使用 `ElapsedTicks` 属性，而不是 `ElapsedMilliseconds`。  
  
 [!code-csharp[PLINQ#19](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/measure2.cs#19)]
 [!code-vb[PLINQ#19](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/measure2.vb#19)]  
  
 试验查询实现时，总执行时间是有用的指标，但它并不总是能说明一切。 为了更深入全面地了解查询线程相互之间以及查询线程和其他运行进程之前的交互，请使用[并发可视化工具](/visualstudio/profiling/concurrency-visualizer)。  
  
## <a name="see-also"></a>请参阅

- [并行 LINQ (PLINQ)](introduction-to-plinq.md)
