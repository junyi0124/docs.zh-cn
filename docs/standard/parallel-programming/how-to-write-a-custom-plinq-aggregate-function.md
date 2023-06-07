---
title: 如何：编写自定义 PLINQ 聚合函数
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- PLINQ queries, how to create aggregate function
ms.assetid: 5a70dd49-ab2a-4798-b551-196ee7042b1a
ms.openlocfilehash: dc03802c960c0926380d7b7fa44fdf436b8fea89
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95734252"
---
# <a name="how-to-write-a-custom-plinq-aggregate-function"></a>如何：编写自定义 PLINQ 聚合函数

此示例展示了如何使用 <xref:System.Linq.ParallelEnumerable.Aggregate%2A> 方法，将自定义聚合函数应用于源序列。  
  
> [!WARNING]
> 本示例旨在演示用法，运行速度可能不如等效的顺序 LINQ to Objects 查询快。 若要详细了解加速，请参阅[了解 PLINQ 中的加速](understanding-speedup-in-plinq.md)。  
  
## <a name="example"></a>示例  

 下面的示例计算整数序列的标准偏差。  
  
 [!code-csharp[PLINQ#31](../../../samples/snippets/csharp/VS_Snippets_Misc/plinq/cs/plinqsamples.cs#31)]
 [!code-vb[PLINQ#31](../../../samples/snippets/visualbasic/VS_Snippets_Misc/plinq/vb/plinqsnippets1.vb#31)]  
  
 此示例使用 PLINQ 独有的聚合标准查询运算符的重载。 此重载需要使用额外的 <xref:System.Func%603?displayProperty=nameWithType> 作为第三个输入参数。 此委托先合并所有线程的结果，再对聚合结果执行最终计算。 此示例将所有线程的总和结果相加到一起。  
  
 请注意，如果 Lambda 表达式主体由一个表达式组成，<xref:System.Func%602?displayProperty=nameWithType> 委托的返回值就是表达式值。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Linq.ParallelEnumerable>
- [并行 LINQ (PLINQ)](introduction-to-plinq.md)
