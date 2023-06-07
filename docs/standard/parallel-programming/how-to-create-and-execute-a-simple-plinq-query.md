---
title: 如何：创建并执行简单的 PLINQ 查询
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, how to create
ms.assetid: 983b4213-bddd-4a44-9262-cbe59186df4c
ms.openlocfilehash: 228a94323c42d7c7a5ecbd295a0db5d73f4f1f49
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95703689"
---
# <a name="how-to-create-and-execute-a-simple-plinq-query"></a>如何：创建并执行简单的 PLINQ 查询

本文中的示例演示如何通过对源序列使用 <xref:System.Linq.ParallelEnumerable.AsParallel%2A?displayProperty=nameWithType> 扩展方法来创建一个简单的并行语言集成查询 (LINQ) 查询，并使用 <xref:System.Linq.ParallelEnumerable.ForAll%2A?displayProperty=nameWithType> 方法执行该查询。  
  
> [!NOTE]
> 本文档使用 lambda 表达式在 PLINQ 中定义委托。 如果不熟悉 C# 或 Visual Basic 中的 lambda 表达式，请参阅 [PLINQ 和 TPL 中的 Lambda 表达式](lambda-expressions-in-plinq-and-tpl.md)。  
  
## <a name="example"></a>示例  

 [!code-csharp[PLINQ#11](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/create1.cs#11)]
 [!code-vb[PLINQ#11](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/create1.vb#11)]  
  
 此示例演示用于在结果序列的排序不重要的情况下创建和执行任何并行 LINQ 查询的基本模式. 未排序的查询通常比已排序的查询快。 查询将源分区为多个任务，这些任务将在多个线程上异步执行。 每个任务的完成顺序不仅取决于处理分区中的元素所涉及的工作量，还取决于诸如操作系统如何调度每个线程之类的外部因素。 本示例旨在演示用法，运行速度可能不如等效的顺序 LINQ to Objects 查询快。 若要详细了解加速，请参阅[了解 PLINQ 中的加速](understanding-speedup-in-plinq.md)。 有关如何在查询中保留元素排序的详细信息，请参阅[如何：在 PLINQ 查询中控制排序](how-to-control-ordering-in-a-plinq-query.md)。  
  
## <a name="see-also"></a>请参阅

- [并行 LINQ (PLINQ)](introduction-to-plinq.md)
