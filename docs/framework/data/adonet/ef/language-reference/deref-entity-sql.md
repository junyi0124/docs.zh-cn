---
title: DEREF (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 4c78e833-b260-453d-9bf4-eb39857dd0fa
ms.openlocfilehash: c0c975ab5cf2761496db6efa1f88f409aa1b1abd
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91148212"
---
# <a name="deref-entity-sql"></a>DEREF (Entity SQL)

取消引用一个引用值，并生成该取消引用的结果。  
  
## <a name="syntax"></a>语法  
  
```sql  
SELECT DEREF ( o.expression ) FROM Table AS o;
```  
  
## <a name="arguments"></a>参数  

 `expression`  
 任何返回集合的有效查询表达式。  
  
## <a name="return-value"></a>返回值  

 引用的实体的值。  
  
## <a name="remarks"></a>备注  

 DEREF 运算符取消引用一个引用值，并生成该取消引用的结果。 例如，如果 `r` 是 ref 类型的引用 \<T> ， `Deref(r)` 则是类型的表达式， `T` 该表达式生成由引用的实体 `r` 。 如果引用值为 Null，或无关联（即，引用的目标不存在），则 DEREF 运算符的结果为 Null。  
  
## <a name="example"></a>示例  

 下面的 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 查询使用 DEREF 运算符取消引用一个引用值，并生成该取消引用的结果。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 按照 [如何：执行返回 PrimitiveType 结果的查询](../how-to-execute-a-query-that-returns-primitivetype-results.md)中的过程进行操作。  
  
2. 将以下查询作为参数传递给 ExecutePrimitiveTypeQuery 方法：  
  
 [!code-sql[DP EntityServices Concepts#DEREF](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#deref)]  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
- [REF](ref-entity-sql.md)
- [CREATEREF](createref-entity-sql.md)
- [KEY](key-entity-sql.md)
- [可以为 NULL 的结构化类型](nullable-structured-types-entity-sql.md)
