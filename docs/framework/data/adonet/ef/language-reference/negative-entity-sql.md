---
title: '-  (负)  (实体 SQL) '
ms.date: 03/30/2017
ms.assetid: 208e54ef-4741-4ec5-89d6-6ff700863cb0
ms.openlocfilehash: 71749dab073fade854c2a494841e3f6b408ebd1d
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204406"
---
# <a name="--negative-entity-sql"></a>-（负号）(Entity SQL)

返回数值表达式的值的负值。  
  
## <a name="syntax"></a>语法  
  
```sql  
- expression  
```  
  
## <a name="arguments"></a>参数  

 `expression`  
 任何一种数值数据类型的任何有效表达式。  
  
## <a name="result-types"></a>结果类型  

 `expression` 的数据类型。  
  
## <a name="remarks"></a>备注  

 如果 `expression` 为无符号类型，则结果类型将是与 `expression`的类型相关性最密切的有符号类型。 例如，如果 `expression` 属于 Byte 类型，则将返回类型为 Int16 的值。  
  
## <a name="example"></a>示例  

 以下 Entity SQL 查询使用 - 算数运算符以返回数值表达式的值的负值。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 执行 [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md)中的过程。  
  
2. 将以下查询作为参数传递给 `ExecuteStructuralTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts#NEGATIVE](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#negative)]  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
