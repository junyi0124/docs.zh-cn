---
title: IN (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 51662950-ee01-4857-b7b9-311dd8515966
ms.openlocfilehash: 582a3b988247f1484197c0905fecf7f4407f88b0
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203665"
---
# <a name="in-entity-sql"></a>IN (Entity SQL)

确定某个值是否与某个集合中的任何值匹配。  
  
## <a name="syntax"></a>语法  
  
```sql  
value [ NOT ] IN expression  
```  
  
## <a name="arguments"></a>参数  

 `value`  
 返回匹配值的任何有效表达式。  
  
 [ NOT ]  
 指定对 IN 的 `Boolean` 结果取反。  
  
 `expression`  
 返回集合以测试是否具有匹配的任何有效表达式。 所有表达式都必须与 `value`一样属于同一类型或属于公共基类型或派生类型。  
  
## <a name="return-value"></a>返回值  

 如果在集合中找到此值，则为 `true`；如果值为空或集合为空，则为空；否则为 `false`。 使用 NOT IN 可对 IN 的结果取反。  
  
## <a name="example"></a>示例  

 以下 Entity SQL 查询使用 IN 运算符以确定某个值是否与集合中的任何值匹配。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 执行 [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md)中的过程。  
  
2. 将以下查询作为参数传递给 `ExecuteStructuralTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts#IN](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#in)]  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
