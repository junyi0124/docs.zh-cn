---
title: 如何：在 DataContext 级别进行筛选
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 15505cd7-0df2-427a-9f86-e0f96f60ee2e
ms.openlocfilehash: 6fe79febd41c040e430dd772b87ca5624824bb65
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173466"
---
# <a name="how-to-filter-at-the-datacontext-level"></a>如何：在 DataContext 级别进行筛选

您可以在 `EntitySets` 级别筛选 `DataContext`。 此类筛选器适用于用此 <xref:System.Data.Linq.DataContext> 实例执行的所有查询。  
  
## <a name="example"></a>示例  

 在下面的示例中，使用 <xref:System.Data.Linq.DataLoadOptions.AssociateWith%28System.Linq.Expressions.LambdaExpression%29?displayProperty=nameWithType> 来筛选截至 `ShippedDate` 的客户的预加载订单。  
  
 [!code-csharp[DLinqQueryConcepts#10](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryConcepts/cs/Program.cs#10)]
 [!code-vb[DLinqQueryConcepts#10](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryConcepts/vb/Module1.vb#10)]  
  
## <a name="see-also"></a>请参阅

- [查询概念](query-concepts.md)
