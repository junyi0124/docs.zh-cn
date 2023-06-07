---
title: CREATEREF (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 489828cf-a335-4449-9360-b0d92eec5481
ms.openlocfilehash: c2a57116f56abf4db3caabcfe3acd0d8afbcf4eb
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91201052"
---
# <a name="createref-entity-sql"></a>CREATEREF (Entity SQL)

创建对实体集中的实体的引用。  
  
## <a name="syntax"></a>语法  
  
```sql  
CreateRef(entityset_identifier, row_typed_expression)  
```  
  
## <a name="arguments"></a>参数  

 `entityset_identifier`  
 实体集标识符，不是字符串文本。  
  
 `row_typed_expression`  
 对应于实体类型的键属性的行类型化表达式。  
  
## <a name="remarks"></a>备注  

 `row_typed_expression` 必须在结构上等效于实体的键类型。 即，其字段的数目和类型以及顺序必须与实体键相同。  
  
 在下面的示例中，Orders 和 BadOrders 都是类型 Order 的实体集，而假定 Id 为 Order 的单个键属性。 该示例演示如何生成对 BadOrders 中的实体的引用。 请注意，该引用可以是无关联引用。  即，该引用可以不真正标识特定实体。 在这种情况下，对该引用的 `DEREF` 操作会返回 Null。  
  
```sql  
SELECT CreateRef(LOB.BadOrders, row(o.Id))
FROM LOB.Orders AS o
```  
  
## <a name="example"></a>示例  

 下面的 Entity SQL 查询使用 CREATEREF 运算符创建对实体集中的实体的引用。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 执行 [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md)中的过程。  
  
2. 将以下查询作为参数传递给 `ExecuteStructuralTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts#CREATEREF](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#createref)]  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
- [DEREF](deref-entity-sql.md)
- [KEY](key-entity-sql.md)
- [REF](ref-entity-sql.md)
