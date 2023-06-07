---
title: HAVING (Entity SQL)
ms.date: 03/30/2017
ms.assetid: b5d52d97-8372-4335-beac-2d0b79dc3707
ms.openlocfilehash: a117f377b3f03b6a1a12e39426a24f3141aa40ff
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204458"
---
# <a name="having-entity-sql"></a>HAVING (Entity SQL)

指定组或聚合的搜索条件。  
  
## <a name="syntax"></a>语法  
  
```sql  
[ HAVING search_condition ]  
```  
  
## <a name="arguments"></a>参数  

 `search_condition`  
 指定组或聚合应满足的搜索条件。 当 HAVING 与 GROUP BY ALL 一起使用时，HAVING 子句优先于 ALL。  
  
## <a name="remarks"></a>备注  

 HAVING 子句用于对分组结果指定附加筛选条件。 如果在查询表达式中未指定 GROUP BY 子句，则将使用隐式单集组。  
  
> [!NOTE]
> HAVING 只能与 [SELECT](select-entity-sql.md) 语句一起使用。 如果不使用 [GROUP BY](group-by-entity-sql.md) ，则其行为与 WHERE 子句类似。  
  
HAVING 子句与 WHERE 子句的工作方式类似，只是它应用在 GROUP BY 操作之后。 这意味着 HAVING 子句只能引用分组别名和聚合，如以下示例中所示：
  
```sql  
SELECT Name, SUM(o.Price * o.Quantity) AS Total FROM orderLines AS o GROUP BY o.Product AS Name  
HAVING SUM(o.Quantity) > 1  
```  
  
 上例将组限定为只包含多个产品的组。  
  
## <a name="example"></a>示例  

 下面的 Entity SQL 查询使用 HAVING 和 GROUP BY 运算符指定组或聚合的搜索条件。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 按照 [如何：执行返回 PrimitiveType 结果的查询](../how-to-execute-a-query-that-returns-primitivetype-results.md)中的过程进行操作。  
  
2. 将以下查询作为参数传递给 `ExecutePrimitiveTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts#HAVING](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#having)]  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
- [查询表达式](query-expressions-entity-sql.md)
