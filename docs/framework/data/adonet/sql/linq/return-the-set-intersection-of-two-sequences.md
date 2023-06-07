---
title: 返回两个序列的交集
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d09c344e-3548-4944-a3ed-051880e3f5b8
ms.openlocfilehash: 421ec544345e41f910bf80c855a3a6b4de1c46a6
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91200220"
---
# <a name="return-the-set-intersection-of-two-sequences"></a>返回两个序列的交集

使用 <xref:System.Linq.Queryable.Intersect%2A> 运算符可返回两个序列的交集。  
  
## <a name="example"></a>示例  

 此示例使用 <xref:System.Linq.Queryable.Intersect%2A> 返回和居住的所有国家/地区的序列 `Customers` `Employees` 。  
  
 [!code-csharp[DLinqQueryExamples#42](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#42)]
 [!code-vb[DLinqQueryExamples#42](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#42)]  
  
 在 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 中，<xref:System.Linq.Queryable.Intersect%2A> 运算仅对集合而言是定义完善的。 针对多重集的语义尚未定义。  
  
## <a name="see-also"></a>请参阅

- [查询示例](query-examples.md)
- [标准查询运算符转换](standard-query-operator-translation.md)
