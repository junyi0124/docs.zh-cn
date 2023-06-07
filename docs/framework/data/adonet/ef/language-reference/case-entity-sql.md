---
title: CASE (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 26a47873-e87d-4ba2-9e2c-3787c21efe89
ms.openlocfilehash: 65efedd36401db402a32748afaebff0f2af9f2a7
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91185218"
---
# <a name="case-entity-sql"></a>CASE (Entity SQL)

求出一组 `Boolean` 表达式的值以确定结果。  
  
## <a name="syntax"></a>语法  
  
```csharp  
CASE  
     WHEN Boolean_expression THEN result_expression
    [ ...n ]
     [
    ELSE else_result_expression
     ]
END  
```  
  
## <a name="arguments"></a>参数  

 `n`  
 一个占位符，表明可以使用多个 WHEN `Boolean_expression` THEN `result_expression` 子句。  
  
 THEN `result_expression`  
 作为在 `Boolean_expression` 的计算结果为 `true`时返回的表达式。 `result expression` 是任何有效的表达式。  
  
 ELSE `else_result_expression`  
 比较运算的结果都不为 `true`时返回的表达式。 如果忽略此参数且比较运算计算的结果不为 `true`，CASE 将返回空值。 `else_result_expression` 是任何有效的表达式。 `else_result_expression` 及任何 `result_expression` 的数据类型必须相同或必须是隐式转换的数据类型。  
  
 WHEN `Boolean_expression`  
 使用 CASE 搜索格式时所计算的 `Boolean` 表达式。 `Boolean_expression` 是任何有效的 `Boolean` 表达式。  
  
## <a name="return-value"></a>返回值  

 从 `result_expression` 和可选 `else_result_expression`的类型集中返回优先级最高的类型。  
  
## <a name="remarks"></a>备注  

 [!INCLUDE[esql](../../../../../../includes/esql-md.md)]Case 表达式类似于 transact-sql case 表达式。 可以使用 case 表达式进行一系列条件测试，以确定哪个表达式将产生正确的结果。 这种格式的 case 表达式应用于由一个或多个 `Boolean` 表达式组成的一组表达式，以确定正确的结果表达式。  
  
 CASE 函数以指定的顺序为每个 WHEN 子句计算 `Boolean_expression` 的值，然后返回首个满足 `result_expression` = `Boolean_expression` 的 `true`。 而不对剩下的表达式求值。 如果没有任何 `Boolean_expression` 的计算结果为 `true`，则当指定了 ELSE 子句时，数据库引擎将返回 `else_result_expression` ；如果未指定 ELSE 子句，则返回空值。  
  
 CASE 语句不会返回 multiset 类型。  
  
## <a name="example"></a>示例  

 以下 Entity SQL 查询使用 CASE 表达式计算一组 `Boolean` 表达式以确定结果。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 按照 [如何：执行返回 PrimitiveType 结果的查询](../how-to-execute-a-query-that-returns-primitivetype-results.md)中的过程进行操作。  
  
2. 将以下查询作为参数传递给 `ExecutePrimitiveTypeQuery` 方法：  
  
 [!code-csharp[DP EntityServices Concepts 2#CASE_WHEN_THEN_ELSE](../../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts 2/cs/entitysql.cs#case_when_then_else)]  
  
## <a name="see-also"></a>请参阅

- [THEN](then-entity-sql.md)
- [SELECT](select-entity-sql.md)
- [实体 SQL 引用](entity-sql-reference.md)
