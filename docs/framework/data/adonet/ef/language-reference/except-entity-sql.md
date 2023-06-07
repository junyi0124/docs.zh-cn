---
title: EXCEPT (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 69cc23e5-3f8f-4b49-b20e-2f84ff11c80d
ms.openlocfilehash: 6797f8038a83533b5a6bd41ad402daec7abdc7de
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91148044"
---
# <a name="except-entity-sql"></a>EXCEPT (Entity SQL)

返回由 EXCEPT 操作数左侧的查询表达式返回而不由 EXCEPT 操作数右侧的查询表达式返回的任何非重复值的集合。 所有表达式都必须与 `expression`一样属于同一类型或属于公共基类型或派生类型。  
  
## <a name="syntax"></a>语法  
  
```sql  
expression EXCEPT expression  
```  
  
## <a name="arguments"></a>参数  

 `expression`  
 返回一个集合以与从其他查询表达式返回的集合进行比较的任何有效查询表达式。  
  
## <a name="return-value"></a>返回值  

 与 `expression`具有相同类型或属于公共基类型或派生类型的一个集合。  
  
## <a name="remarks"></a>备注  

 EXCEPT 是 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 集运算符之一。 所有 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 集运算符都是从左到右进行求值。 下表显示 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 集运算符的优先级。  
  
|优先级|运算符|  
|----------------|---------------|  
|最高|INTERSECT|  
||UNION<br /><br /> UNION ALL|  
||EXCEPT|  
|最低|EXISTS<br /><br /> OVERLAPS<br /><br /> FLATTEN<br /><br /> SET|  
  
## <a name="example"></a>示例  

 以下 Entity SQL 查询使用 EXCEPT 运算符以返回从两个查询表达式返回的任何非重复值的集合。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 执行 [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md)中的过程。  
  
2. 将以下查询作为参数传递给 `ExecuteStructuralTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts#EXCEPT](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#except)]  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
