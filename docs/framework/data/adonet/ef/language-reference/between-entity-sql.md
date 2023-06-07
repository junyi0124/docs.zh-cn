---
title: BETWEEN (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 4dcdd754-ae01-4e78-bf28-8a117fb2b73e
ms.openlocfilehash: 17e3fe97942b34232640b0326eca2c5729e86989
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91201169"
---
# <a name="between-entity-sql"></a>BETWEEN (Entity SQL)

确定表达式的结果值是否在指定范围内。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] BETWEEN 表达式的功能与 Transact-SQL BETWEEN 表达式相同。  
  
## <a name="syntax"></a>语法  
  
```csharp  
expression [ NOT ] BETWEEN begin_expression AND end_expression
```  
  
## <a name="arguments"></a>参数  

 `expression`  
 要测试是否在 `begin_expression` 和 `end_expression` 所定义的范围内的任何有效表达式。 `expression` 必须与 `begin_expression` 和 `end_expression` 的类型都相同。  
  
 `begin_expression`  
 任何有效表达式。 `begin_expression` 必须与 `expression` 和 `end_expression` 的类型都相同。 `begin_expression` 应小于 `end_expression`，否则返回值将取反。  
  
 `end_expression`  
 任何有效表达式。 `end_expression` 必须与 `expression` 和 `begin_expression` 的类型都相同。  
  
 NOT  
 指定对 BETWEEN 的结果取反。  
  
 AND  
 用作一个占位符，指示 `expression` 应该处于由 `begin_expression` 和 `end_expression` 指定的范围内。  
  
## <a name="return-value"></a>返回值  

 如果 `true` 处于由 `expression` 和 `begin_expression` 指定的范围内，则为 `end_expression`；否则为 `false`。 如果 `null` 为 `expression`，或者 `null` 或 `begin_expression` 为 `end_expression`，则返回 `null`。  
  
## <a name="remarks"></a>备注  

 若要指定排他范围，请使用大于 ( # A0) 并小于 ( # A1) 运算符而不是 BETWEEN。  
  
## <a name="example"></a>示例  

 下面的 Entity SQL 查询使用 BETWEEN 运算符确定一个表达式的结果值是否在指定范围内。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 执行 [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md)中的过程。  
  
2. 将以下查询作为参数传递给 `ExecuteStructuralTypeQuery` 方法：  
  
 [!code-csharp[DP EntityServices Concepts 2#BETWEEN](../../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts 2/cs/entitysql.cs#between)]  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
