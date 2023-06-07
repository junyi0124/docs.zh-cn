---
title: 返回两个序列的并集
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 8b8bd3cb-86d4-4a3b-9906-61f68726dd1f
ms.openlocfilehash: 0fe32d8c3c37e99a50ca03262dc6184337b4450e
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91200194"
---
# <a name="return-the-set-union-of-two-sequences"></a>返回两个序列的并集

使用 <xref:System.Linq.Queryable.Union%2A> 运算符可返回两个序列的并集。  
  
## <a name="example"></a>示例  

 此示例使用 <xref:System.Linq.Queryable.Union%2A> 返回有或的所有国家/地区的序列 `Customers` `Employees` 。  
  
 [!code-csharp[DLinqQueryExamples#43](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#43)]
 [!code-vb[DLinqQueryExamples#43](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#43)]  
  
 在中 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] ， <xref:System.Linq.Queryable.Union%2A> 运算符是为多重集定义的，因为多重集的无序串联 (实际上是 [`UNION ALL`](/sql/t-sql/language-elements/set-operators-union-transact-sql) SQL) 中子句的结果。

有关详细信息和示例，请参阅 <xref:System.Linq.Queryable.Union%2A?displayProperty=nameWithType> 。
  
## <a name="see-also"></a>请参阅

- [查询示例](query-examples.md)
- [标准查询运算符转换](standard-query-operator-translation.md)
