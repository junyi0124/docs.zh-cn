---
title: '-  (除以)  (实体 SQL) '
ms.date: 03/30/2017
ms.assetid: ef48c368-f3ed-4275-8ada-4e9649781262
ms.openlocfilehash: d7d25d8c5b91850b21e36ba8f6f668af7a7d0045
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91148156"
---
# <a name="-divide-entity-sql"></a>/（除）(Entity SQL)

用一个数除以另一个数。  
  
## <a name="syntax"></a>语法  
  
```sql  
dividend / divisor  
```  
  
## <a name="arguments"></a>参数  

 `dividend`  
 被除数的数值表达式。 `dividend` 为任何一种数值数据类型的任何有效表达式。  
  
 `divisor`  
 除数的数值表达式。 `divisor` 为任何一种数值数据类型的任何有效表达式。  
  
## <a name="result-types"></a>结果类型  

 对这两个参数进行隐式类型提升而产生的数据类型。 有关隐式类型升级的详细信息，请参阅 [类型系统](type-system-entity-sql.md)。  
  
## <a name="example"></a>示例  

 以下实体 SQL 查询使用/算术运算符将一个数除以另一个数。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 执行 [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md)中的过程。  
  
2. 将以下查询作为参数传递给 `ExecuteStructuralTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts#DIVIDE](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#divide)]  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
