---
title: 将某一类型转换为泛型 IEnumerable
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 64774fb5-7447-4296-ad3b-8a94346f99a1
ms.openlocfilehash: c2d34ae708f79d9f920679b3714a100fe8943c38
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91164410"
---
# <a name="convert-a-type-to-a-generic-ienumerable"></a>将某一类型转换为泛型 IEnumerable

使用 <xref:System.Linq.Enumerable.AsEnumerable%2A> 可返回类型化为泛型 `IEnumerable` 的参数。  
  
## <a name="example"></a>示例  

 在此示例中，[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]（使用默认泛型 `Query`）会尝试将查询转换为 SQL 并在服务器上执行。 但 `where` 子句引用用户定义的客户端方法 (`isValidProduct`)，此方法无法转换为 SQL。  
  
 解决方法是指定 <xref:System.Collections.Generic.IEnumerable%601> 的客户端泛型 `where` 实现以替换泛型 <xref:System.Linq.IQueryable%601>。 可通过调用 <xref:System.Linq.Enumerable.AsEnumerable%2A> 运算符来执行此操作。  
  
 [!code-csharp[DLinqQueryExamples#46](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#46)]
 [!code-vb[DLinqQueryExamples#46](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#46)]  
  
## <a name="see-also"></a>请参阅

- [查询示例](query-examples.md)
