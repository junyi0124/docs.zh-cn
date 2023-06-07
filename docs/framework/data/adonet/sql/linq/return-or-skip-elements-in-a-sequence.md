---
title: 返回或跳过序列中的元素
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 81a31acd-e0f1-4bca-9a12-fa1ad5752374
ms.openlocfilehash: 1a36d3c8457374183148599bf8160d14847cbf3e
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91200415"
---
# <a name="return-or-skip-elements-in-a-sequence"></a>返回或跳过序列中的元素

使用 <xref:System.Linq.Queryable.Take%2A> 运算符可返回序列中给定数目的元素，然后跳过其余元素。  
  
 使用 <xref:System.Linq.Queryable.Skip%2A> 运算符可跳过序列中给定数目的元素，然后返回其余元素。  
  
> [!NOTE]
> <xref:System.Linq.Enumerable.Take%2A> 和 <xref:System.Linq.Enumerable.Skip%2A> 用在针对 SQL Server 2000 的查询中时存在一定的限制。 有关详细信息，请参阅 [疑难解答](troubleshooting.md)中的 "跳过并使用 SQL Server 2000" 条目。  
  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 通过使用带有 SQL <xref:System.Linq.Queryable.Skip%2A> 子句的子查询来转换 `NOT EXISTS`。 这种转换存在以下局限性：  
  
- 自变量必须为集合。 不支持多重集，即便为有序多重集也不例外。  
  
- 生成的查询可能要比为应用了 <xref:System.Linq.Queryable.Skip%2A> 的基查询生成的查询复杂得多。 这种复杂性可能会导致性能降低，甚至发生超时。  
  
## <a name="example"></a>示例  

 下面的示例使用 `Take` 选择前五个受雇的 `Employees`。 请注意，此集合首先按 `HireDate` 排序。  
  
 [!code-csharp[DLinqQueryExamples#16](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#16)]
 [!code-vb[DLinqQueryExamples#16](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#16)]  
  
## <a name="example"></a>示例  

 下面的示例使用 <xref:System.Linq.Queryable.Skip%2A> 选择 10 种最贵的 `Products` 以外的所有产品。  
  
 [!code-csharp[DLinqQueryExamples#17](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#17)]
 [!code-vb[DLinqQueryExamples#17](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#17)]  
  
## <a name="example"></a>示例  

 下面的示例结合使用 <xref:System.Linq.Queryable.Skip%2A> 和 <xref:System.Linq.Queryable.Take%2A> 方法来跳过前 50 条记录，然后返回下 10 条记录。  
  
 [!code-csharp[DLinqQueryExamples#18](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#18)]
 [!code-vb[DLinqQueryExamples#18](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#18)]  
  
 <xref:System.Linq.Queryable.Take%2A> 和 <xref:System.Linq.Queryable.Skip%2A> 运算仅对有序集而言是定义完善的。 未定义针对无序集或多重集的语义。  
  
 由于 SQL 中的排序存在限制，因此 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 会设法将 <xref:System.Linq.Queryable.Take%2A> 或 <xref:System.Linq.Queryable.Skip%2A> 运算符的参数的排序操作移到相应运算符的结果中进行。  
  
> [!NOTE]
> SQL Server 2000 和 SQL Server 2005 的转换不同。 如果你计划将 <xref:System.Linq.Queryable.Skip%2A> 与查询结合使用，请使用 SQL Server 2005。  
  
 对于 SQL Server 2000，请考虑以下 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 查询：  
  
 [!code-csharp[DLinqQueryExamples#19](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQueryExamples/cs/Program.cs#19)]
 [!code-vb[DLinqQueryExamples#19](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQueryExamples/vb/Module1.vb#19)]  
  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 将排序操作移到 SQL 代码的结尾进行，如下所示：  
  
```sql
SELECT TOP 1 [t0].[CustomerID], [t0].[CompanyName],  
FROM [Customers] AS [t0]  
WHERE (NOT (EXISTS(  
    SELECT NULL AS [EMPTY]  
    FROM (  
        SELECT TOP 1 [t1].[CustomerID]  
        FROM [Customers] AS [t1]  
        WHERE [t1].[City] = @p0  
        ORDER BY [t1].[CustomerID]  
        ) AS [t2]  
    WHERE [t0].[CustomerID] = [t2].[CustomerID]  
    ))) AND ([t0].[City] = @p1)  
ORDER BY [t0].[CustomerID]  
```  
  
 当 <xref:System.Linq.Queryable.Take%2A> 和 <xref:System.Linq.Queryable.Skip%2A> 链接在一起时，所有指定的排序都必须一致。 否则，结果将是不确定的。  
  
 对于非负的、基于 SQL 规范的整型常量参数，<xref:System.Linq.Queryable.Take%2A> 和 <xref:System.Linq.Queryable.Skip%2A> 都是定义完善的。  
  
## <a name="see-also"></a>请参阅

- [查询示例](query-examples.md)
- [标准查询运算符转换](standard-query-operator-translation.md)
