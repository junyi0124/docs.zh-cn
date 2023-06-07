---
title: 基于方法的查询语法示例：中间件排序
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 5d21b178-d731-471a-8534-1f8184a2ef06
ms.openlocfilehash: 02889fb4172d24634cdcb1c8384a804b2ce1a0e8
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91191926"
---
# <a name="method-based-query-syntax-examples-ordering"></a>基于方法的查询语法示例：中间件排序

本主题中的示例演示如何使用 <xref:System.Linq.Enumerable.ThenBy%2A> 方法通过使用基于方法的查询语法来查询 [AdventureWorks 销售模型](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks) 。 这些示例中使用的 AdventureWorks 销售模型从 AdventureWorks 示例数据库中的 Contact、Address、Product、SalesOrderHeader 和 SalesOrderDetail 等表生成。  
  
 本主题中的示例使用以下 `using` / `Imports` 语句：  
  
 [!code-csharp[DP L2E Examples#ImportsUsing](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Examples/CS/Program.cs#importsusing)]
 [!code-vb[DP L2E Examples#ImportsUsing](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Examples/VB/Module1.vb#importsusing)]  
  
## <a name="thenby"></a>ThenBy  
  
### <a name="example"></a>示例  

 采用基于方法的查询语法的以下示例使用 <xref:System.Linq.Queryable.OrderBy%2A> 和 <xref:System.Linq.Queryable.ThenBy%2A> 以返回先按姓氏后按名字排序的联系人列表。  
  
 [!code-csharp[DP L2E Examples#OrderByThenBy_MQ](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Examples/CS/Program.cs#orderbythenby_mq)]
 [!code-vb[DP L2E Examples#OrderByThenBy_MQ](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Examples/VB/Module1.vb#orderbythenby_mq)]  
  
## <a name="thenbydescending"></a>ThenByDescending  
  
### <a name="example"></a>示例  

 以下示例使用 <xref:System.Linq.Queryable.OrderBy%2A> 和 <xref:System.Linq.Queryable.ThenByDescending%2A> 方法以首先按标价排序，然后执行产品名称的降序排序。  
  
 [!code-csharp[DP L2E Examples#ThenByDescending_MQ](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Examples/CS/Program.cs#thenbydescending_mq)]
 [!code-vb[DP L2E Examples#ThenByDescending_MQ](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Examples/VB/Module1.vb#thenbydescending_mq)]  
  
## <a name="see-also"></a>请参阅

- [LINQ to Entities 中的查询](queries-in-linq-to-entities.md)
