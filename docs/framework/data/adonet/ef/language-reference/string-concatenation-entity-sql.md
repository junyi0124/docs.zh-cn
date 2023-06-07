---
title: '+  () 实体 SQL (字符串串联) '
ms.date: 03/30/2017
ms.assetid: 580130fa-6c7c-4f76-a47d-d22c27ccadf6
ms.openlocfilehash: 92591448a3707ba11ad2462161050e48e0398728
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173622"
---
# <a name="-string-concatenation-entity-sql"></a>+（字符串串联）(Entity SQL)

串联两个字符串。  
  
## <a name="syntax"></a>语法  
  
```sql  
expression + expression  
```  
  
## <a name="arguments"></a>参数  

 `expression`  
 EDM.String 数据类型的任何有效表达式。 两个表达式必须具有相同的数据类型，或者其中一个表达式必须能够隐式转换为另一个表达式的数据类型。  
  
## <a name="result-types"></a>结果类型  

 对这两个参数进行隐式类型提升而产生的数据类型。 有关隐式类型升级的详细信息，请参阅 [类型系统](type-system-entity-sql.md)。  
  
## <a name="example"></a>示例  

 以下 Entity SQL 查询使用 + 运算符以串联两个字符串。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 按照 [如何：执行返回 PrimitiveType 结果的查询](../how-to-execute-a-query-that-returns-primitivetype-results.md)中的过程进行操作。  
  
2. 将以下查询作为参数传递给 `ExecutePrimitiveTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts#CONCAT](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#concat)]  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
- [概念模型类型 (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#conceptual-model-types-csdl)
