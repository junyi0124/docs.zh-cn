---
title: KEY (Entity SQL)
ms.date: 03/30/2017
ms.assetid: cbaa97a8-c89c-4460-8c74-00474695789f
ms.openlocfilehash: 07160467dcee60377e3ef448fdc66092da4e06e7
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91161966"
---
# <a name="key-entity-sql"></a>KEY (Entity SQL)

提取引用或实体表达式的键。  
  
## <a name="syntax"></a>语法  
  
```sql  
KEY(createref_expression)  
```  
  
## <a name="remarks"></a>备注  

 实体键按指定实体或实体引用的正确顺序包含键值。 因为多个实体集可以基于相同的类型，所以同一个键可能出现在每个实体集中。 若要获取唯一引用，请使用 `REF`。 KEY 运算符的返回类型为行类型，该行类型按相同顺序为实体的每个键包含一个字段。  
  
 在下面的示例中，键运算符传递对 BadOrder 实体的引用，并返回该引用的键部分。 在此示例中，一个记录类型恰好包含对应于 `Id` 属性的一个字段。  
  
```sql  
select Key( CreateRef(LOB.BadOrders, row(o.Id)) )
from LOB.Orders as o  
```  
  
## <a name="example"></a>示例  

 下面的 Entity SQL 查询使用 KEY 运算符提取具有类型引用的表达式的键部分。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 执行 [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md)中的过程。  
  
2. 将以下查询作为参数传递给 `ExecuteStructuralTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts#KEY](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#key)]  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
- [CREATEREF](createref-entity-sql.md)
- [REF](ref-entity-sql.md)
- [DEREF](deref-entity-sql.md)
