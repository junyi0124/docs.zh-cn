---
title: Null 比较
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: ef88af8c-8dfe-4556-8b56-81df960a900b
ms.openlocfilehash: 71b7c4d86debe8cf267b1b65e3d176cbc4704e6d
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91185101"
---
# <a name="null-comparisons"></a>Null 比较

数据源中的 `null` 值指示未知的值。 在 LINQ to Entities 查询中，可以检查是否存在 null 值，以便仅对具有有效或非 null 数据的行执行某些计算或比较。 但是，CLR null 语义可能与数据源的 null 语义不同。 大多数数据库使用某个版本的三值逻辑处理 null 比较。 即，对 null 值的比较不会计算为 `true` 或 `false`，而是计算为 `unknown`。 通常这是 ANSI null 值的实现，但情况并非总是如此。  
  
 在 SQL Server 中，null 等于 null 比较默认返回 null 值。 在下面的示例中， `ShipDate` 将从结果集中排除为 null 的行，并且 transact-sql 语句将返回0行。  
  
```sql  
-- Find order details and orders with no ship date.  
SELECT h.SalesOrderID  
FROM Sales.SalesOrderHeader h  
JOIN Sales.SalesOrderDetail o ON o.SalesOrderID = h.SalesOrderID  
WHERE h.ShipDate IS Null  
```  
  
 这与 CLR null 语义有很大差异，在 CLR null 语义中 null 等于 null 比较返回 true。  
  
 下面的 LINQ 查询以 CLR 表示，但在数据源中执行。 由于无法保证在数据源中会遵循 CLR 语义，因此预期行为是不确定的。  
  
 [!code-csharp[DP L2E Conceptual Examples#JoinOnNull](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#joinonnull)]
 [!code-vb[DP L2E Conceptual Examples#JoinOnNull](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#joinonnull)]  
  
## <a name="key-selectors"></a>键选择器  

 *键选择器*是在标准查询运算符中用于从元素中提取键的函数。 在键选择器函数中，表达式可以与常量进行比较。 如果表达式与 null 常量进行比较或两个 null 常量进行比较，则会展现 CLR null 语义。 如果数据源中具有 null 值的两个列进行比较，则会展现存储 null 语义。 键选择器出现在许多分组和排序标准查询运算符（如 <xref:System.Linq.Queryable.GroupBy%2A>）中，用于选择对查询结果进行排序或分组所依据的键。  
  
## <a name="null-property-on-a-null-object"></a>Null 对象上的 Null 属性  

 在实体框架中，null 对象的属性为 null。 当尝试引用 CLR 中的 null 对象的属性时，会收到 <xref:System.NullReferenceException>。 当 LINQ 查询涉及 null 对象的属性时，可能会导致不一致的行为。  
  
 例如，下面的查询在命令树层中执行对 `NewProduct` 的强制转换，这可能导致 `Introduced` 属性为 null。 如果数据库将 null 比较定义为 <xref:System.DateTime> 比较计算为 true，则会包含该行。  
  
 [!code-csharp[DP L2E Conceptual Examples#CastResultsIsNull](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DP L2E Conceptual Examples/CS/Program.cs#castresultsisnull)]
 [!code-vb[DP L2E Conceptual Examples#CastResultsIsNull](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DP L2E Conceptual Examples/VB/Module1.vb#castresultsisnull)]  
  
## <a name="passing-null-collections-to-aggregate-functions"></a>将 Null 集合传递到聚合函数  

 在 LINQ to Entities 中，当您将支持的集合传递 `IQueryable` 到聚合函数时，将在数据库中执行聚合运算。 在内存中执行的查询的结果与在数据库中执行的查询的结果可能有差异。 对于内存中查询，如果没有匹配项，查询将返回零。 在数据库中，同一查询返回 `null`。 如果将 `null` 值传递给 LINQ 聚合函数，则会引发异常。 若要接受可能 `null` 的值，请将接收查询结果的类型的类型和属性强制转换为可以为 null 的值类型。  
  
## <a name="see-also"></a>请参阅

- [LINQ to Entities 查询中的表达式](expressions-in-linq-to-entities-queries.md)
