---
title: INTERSECT (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 93c6fe33-f341-4b52-911e-adf503891951
ms.openlocfilehash: 217cd9b2a428c890d83d2b55d45321a04488398e
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203639"
---
# <a name="intersect-entity-sql"></a>INTERSECT (Entity SQL)

返回 INTERSECT 操作数左右两边的两个查询表达式均返回的所有非重复值的集合。 所有表达式都必须与 `expression`一样属于同一类型或属于公共基类型或派生类型。  
  
## <a name="syntax"></a>语法  
  
```sql  
expression INTERSECT expression  
```  
  
## <a name="arguments"></a>参数  

 `expression`  
 返回一个集合以与从其他查询表达式返回的集合进行比较的任何有效查询表达式。  
  
## <a name="return-value"></a>返回值  

 与 `expression`具有相同类型或属于公共基类型或派生类型的一个集合。  
  
## <a name="remarks"></a>备注  

 INTERSECT 是 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 集运算符之一。 所有 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 集运算符都是从左到右进行求值。 有关集运算符的优先级信息 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] ，请参阅 [EXCEPT](except-entity-sql.md)。  
  
## <a name="example"></a>示例  

 以下 Entity SQL 查询使用 INTERSECT 运算符以返回 INTERSECT 操作数左右两边的两个查询表达式均返回的所有非重复值的集合。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 执行 [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md)中的过程。  
  
2. 将以下查询作为参数传递给 `ExecuteStructuralTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts#INTERSECT](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#intersect)]  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
