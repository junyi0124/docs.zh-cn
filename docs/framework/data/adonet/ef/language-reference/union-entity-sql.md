---
title: UNION (Entity SQL)
ms.date: 03/30/2017
ms.assetid: df98a4db-b00d-4c8b-bd74-0d285f27e1df
ms.openlocfilehash: 9c4106d26fb73219d7b5f0c6763736aaf9163d4b
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91200974"
---
# <a name="union-entity-sql"></a>UNION (Entity SQL)

将两个或更多查询的结果组合成单个集合。  
  
## <a name="syntax"></a>语法  
  
```sql  
expression  
UNION [ ALL ]  
expression  
```  
  
## <a name="arguments"></a>参数  

 `expression`  
 返回一个集合以与该集合进行组合的任何有效查询表达式。所有表达式都必须与 `expression`一样属于同一类型或属于公共基类型或派生类型。  
  
 UNION  
 指定组合多个集合并将其作为单个集合返回。  
  
 ALL  
 指定组合多个集合并将其作为单个集合返回（包括重复项）。 如果未指定，则从结果集合中删除重复项。  
  
## <a name="return-value"></a>返回值  

 与 `expression`具有相同类型或属于公共基类型或派生类型的一个集合。  
  
## <a name="remarks"></a>备注  

 UNION 是 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 集运算符之一。 所有 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 集运算符都是从左到右进行求值。 有关集运算符的优先级信息 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] ，请参阅 [EXCEPT](except-entity-sql.md)。  
  
## <a name="example"></a>示例  

 以下 Entity SQL 查询使用 UNION ALL 运算符以将两个查询的结果组合成单个集合。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 执行 [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md)中的过程。  
  
2. 将以下查询作为参数传递给 `ExecuteStructuralTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts#UNION](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#union)]  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
