---
title: MULTISET (Entity SQL)
ms.date: 03/30/2017
ms.assetid: eb90a377-e47a-43a5-b308-e993b6d611e6
ms.openlocfilehash: a81787da9ee1af084a903dcb50b024f3d26d18fc
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91175689"
---
# <a name="multiset-entity-sql"></a>MULTISET (Entity SQL)

根据值列表创建多集的实例。 MULTISET 构造函数中的所有值都必须具有兼容类型 `T`。 不允许使用空的多集构造函数。  
  
## <a name="syntax"></a>语法  
  
```sql  
MULTISET ( expression [{, expression }] )  
-- or  
{ expression [{, expression }] }  
```  
  
## <a name="arguments"></a>参数  

 `expression`  
 任何有效的值列表。  
  
## <a name="return-value"></a>返回值  

 类型集的集合 \<T> 。  
  
## <a name="remarks"></a>备注  

 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 提供了三种构造函数：行构造函数、对象构造函数和多集（或集合）构造函数。 有关详细信息，请参阅 [构造类型](constructing-types-entity-sql.md)。  
  
 多集构造函数根据值列表创建多集的实例。 该构造函数中的所有值都必须具有兼容类型。  
  
 例如，下面的表达式创建整数的多集。  
  
 `MULTISET(1, 2, 3)`  
  
 `{1, 2, 3}`  
  
> [!NOTE]
> 仅当包装多集具有单个多重集元素时才支持嵌套的多集文本;例如， `{{1, 2, 3}}` 。 当包装多集具有多个多集元素（如 `{{1, 2}, {3, 4}}`）时，不支持嵌套多集文本。  
  
## <a name="example"></a>示例  

 下面的 Entity SQL 查询使用 MULTISET 运算符根据值列表创建多集的实例。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 执行 [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md)中的过程。  
  
2. 将以下查询作为参数传递给 `ExecuteStructuralTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts#MULTISET](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#multiset)]  
  
## <a name="see-also"></a>请参阅

- [构造类型](constructing-types-entity-sql.md)
- [实体 SQL 引用](entity-sql-reference.md)
