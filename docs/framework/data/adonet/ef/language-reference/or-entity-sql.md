---
title: '|| (OR) (Entity SQL)'
ms.date: 03/30/2017
ms.assetid: 8e649648-eb9a-4380-9d74-36e62260628c
ms.openlocfilehash: 89c0a92030f2f067d5e5d45b58d475414a224ce4
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91150799"
---
# <a name="-or-entity-sql"></a>|| (OR) (Entity SQL)

组合两个 `Boolean` 表达式。  
  
## <a name="syntax"></a>语法  
  
```sql  
boolean_expression OR boolean_expression  
-- or
boolean_expression || boolean_expression  
```  
  
## <a name="arguments"></a>参数  

 `boolean_expression`  
 返回 `Boolean`的任何有效表达式。  
  
## <a name="return-value"></a>返回值  

 当任何一个条件为`true` 时，为 `true`；否则为 `false`。  
  
## <a name="remarks"></a>备注  

 OR 是 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 逻辑运算符。 它用于组合两个条件。 在一个语句中使用多个逻辑运算符时，在 AND 运算符之后对 OR 运算符求值。 不过，使用括号可以更改求值的顺序。  
  
 双竖线 ( # A2) 与 OR 运算符具有相同的功能。  
  
 下表显示可能的输入值和返回类型。  
  
||`TRUE`|`FALSE`|`NULL`|  
|-|------------|-------------|------------|  
|`TRUE`|TRUE|TRUE|TRUE|  
|`FALSE`|true|FALSE|Null|  
|`NULL`|TRUE|Null|Null|  
  
## <a name="example"></a>示例  

 以下 Entity SQL 查询使用 OR 运算符以组合两个 `Boolean` 表达式。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 执行 [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md)中的过程。  
  
2. 将以下查询作为参数传递给 `ExecuteStructuralTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts 2#OR](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#or)]  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
