---
title: 如何：在 PLINQ 查询中控制排序
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, how to control ordering
ms.assetid: c67eccc7-004d-4b2f-987e-919cbbd62ef7
ms.openlocfilehash: 945a1e882c473ec9926e60a9e3e513d4949a6717
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95700907"
---
# <a name="how-to-control-ordering-in-a-plinq-query"></a>如何：在 PLINQ 查询中控制排序

这些示例展示了如何使用 <xref:System.Linq.ParallelEnumerable.AsOrdered%2A> 扩展方法控制 PLINQ 查询中的顺序。  
  
> [!WARNING]
> 这些示例主要用于演示用法，可能会或可能不会比相当的顺序 LINQ to Objects 查询快。  
  
## <a name="example"></a>示例  

 下面的示例暂留了源序列的顺序。 有时，这样做是有必要的；例如，一些查询运算符需要有序的源序列，才能生成正确结果。  
  
 [!code-csharp[PLINQ#12](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#12)]
 [!code-vb[PLINQ#12](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinqsnippets1.vb#12)]  
  
## <a name="example"></a>示例  

 下面的示例展示了一些可能要求源序列应为有序的查询运算符。 虽然这些运算符可以处理无序序列，但可能会生成意外结果。  
  
 [!code-csharp[PLINQ#14](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#14)]
 [!code-vb[PLINQ#14](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinqsnippets1.vb#14)]  
  
 若要运行此方法，请将它粘贴到 [PLINQ 数据样本](plinq-data-sample.md)项目的 PLINQDataSample 类中，再按 F5。  
  
## <a name="example"></a>示例  

 下面的示例展示了如何暂留查询第一部分的顺序，再删除顺序以提升 join 子句的性能，并对最终结果序列重新应用顺序。  
  
 [!code-csharp[PLINQ#15](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#15)]
 [!code-vb[PLINQ#15](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinqsnippets1.vb#15)]  
  
 若要运行此方法，请将它粘贴到 [PLINQ 数据样本](plinq-data-sample.md)项目的 PLINQDataSample 类中，再按 F5。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Linq.ParallelEnumerable>
- [并行 LINQ (PLINQ)](introduction-to-plinq.md)
