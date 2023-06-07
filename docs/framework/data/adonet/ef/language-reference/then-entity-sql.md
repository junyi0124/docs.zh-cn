---
title: THEN (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 54222642-23c6-4f61-9861-67caca53ac5f
ms.openlocfilehash: f038f242bc0873df3d72700aa05fca2f76f777ff
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91161667"
---
# <a name="then-entity-sql"></a>THEN (Entity SQL)

当 WHEN 子句取值为 `true`时的结果。  
  
## <a name="syntax"></a>语法  
  
```sql  
WHEN when_expression THEN then_expression  
```  
  
## <a name="arguments"></a>参数  

 `when_expression`  
 任何有效的 Boolean 表达式。  
  
 `then_expression`  
 任何返回集合的有效查询表达式。  
  
## <a name="remarks"></a>备注  

 如果 `when_expression` 取值为 `true`，则结果为对应的 `then-expression`。 如果任何 WHEN 条件均未得以满足，将求出 `else-expression` 的值。 然而，如果没有 `else-expression`，则结果为空值。  
  
 有关示例，请参阅 [CASE](case-entity-sql.md)。  
  
## <a name="example"></a>示例  

 以下 Entity SQL 查询使用 CASE 表达式计算一组 `Boolean` 表达式的值。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 按照 [如何：执行返回 PrimitiveType 结果的查询](../how-to-execute-a-query-that-returns-primitivetype-results.md)中的过程进行操作。  
  
2. 将以下查询作为参数传递给 `ExecutePrimitiveTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts#CASE_WHEN_THEN_ELSE](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#case_when_then_else)]  
  
## <a name="see-also"></a>请参阅

- [CASE](case-entity-sql.md)
- [实体 SQL 引用](entity-sql-reference.md)
