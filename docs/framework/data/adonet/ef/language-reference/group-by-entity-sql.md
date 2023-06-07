---
title: GROUP BY (Entity SQL)
ms.date: 03/30/2017
ms.assetid: cf4f4972-4724-4945-ba44-943a08549139
ms.openlocfilehash: 281d5d2df389f0952f0552747fa12b67b14d470c
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91204484"
---
# <a name="group-by-entity-sql"></a>GROUP BY (Entity SQL)

指定由查询 ([SELECT](select-entity-sql.md)) 表达式返回的对象要分入的组。  
  
## <a name="syntax"></a>语法  
  
```sql  
[ GROUP BY aliasedExpression [ ,...n ] ]  
```  
  
## <a name="arguments"></a>参数  

 `aliasedExpression`  
 要对其执行分组的任何有效查询表达式。 `expression` 可以是属性或者是引用 FROM 子句所返回的属性的非聚合表达式。 GROUP BY 子句中的每一个表达式的求值结果必须为可比较相等性的类型。 这些类型通常为标量基元类型，如数字、字符串和日期。 不可按集合分组。  
  
## <a name="remarks"></a>备注  

 如果 SELECT 子句中包含聚合函数，则 \<select list> GROUP by 将计算每个组的汇总值。 指定 GROUP BY 时，选择列表中任何非聚合表达式内的每个属性名都应包含在 GROUP BY 列表中，或者 GROUP BY 表达式必须与选择列表表达式完全匹配。  
  
> [!NOTE]
> 如果未指定 ORDER BY 子句，则使用 GROUP BY 子句返回的组没有任何特定的顺序。 若要指定特定的数据排序，建议始终使用 ORDER BY 子句。  
  
 指定 GROUP BY 子句时，无论是显式指定还是隐式指定（例如，通过查询中的 HAVING 子句指定），当前作用域都将隐藏，并且将引入新的作用域。  
  
 SELECT 子句、HAVING 子句和 ORDER BY 子句将无法再引用 FROM 子句中指定的元素名。 您只能引用分组表达式本身。 为此，可以为每个分组表达式指定新的名称（别名）。 如果没有为分组表达式指定别名， [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 将尝试使用别名生成规则来生成别名，如下例中所示。  
  
 `SELECT g1, g2, ...gn FROM c as c1`  
  
 `GROUP BY e1 as g1, e2 as g2, ...en as gn`  
  
 GROUP BY 子句中的表达式无法引用先前在相同 GROUP BY 子句中定义的名称。  
  
 除了分组名称外，还可以在 SELECT 子句、HAVING 子句和 ORDER BY 子句中指定聚合。 聚合中包含的表达式针对组中的每一个元素进行求值。 聚合运算符缩减所有这些表达式的值（通常，但非总是，缩减为单个值）。 聚合表达式可以引用父作用域中可见的原始元素名，或者引用 GROUP BY 子句本身引入的任何新名称。 虽然聚合出现在 SELECT 子句、HAVING 子句和 ORDER BY 子句中，但是实际上它们是在与分组表达式相同的作用域中求值的，如下面的示例中所示。  
  
 `SELECT name, sum(o.Price * o.Quantity) as total`  
  
 `FROM orderLines as o`  
  
 `GROUP BY o.Product as name`  
  
 此查询使用 GROUP BY 子句生成所有已订购产品的成本报表，并按产品细分。 它在分组表达式中为产品指定名称 `name` ，然后在 SELECT 列表中引用该名称。 它还在 SELECT 列表中指定聚合 `sum` ，该聚合在内部引用订单行的价格和数量。  
  
 每一个 GROUP By 键表达式必须至少有一个对输入作用域的引用：  
  
```sql  
SELECT FROM Persons as P  
GROUP BY Q + P   -- GOOD  
GROUP BY Q   -- BAD  
GROUP BY 1   -- BAD, a constant is not allowed  
```  
  
 有关使用 GROUP BY 的示例，请参阅 [HAVING](having-entity-sql.md)。  
  
## <a name="example"></a>示例  

 下面的 Entity SQL 查询使用 GROUP BY 运算符来指定查询所返回的对象的分组。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 按照 [如何：执行返回 PrimitiveType 结果的查询](../how-to-execute-a-query-that-returns-primitivetype-results.md)中的过程进行操作。  
  
2. 将以下查询作为参数传递给 `ExecutePrimitiveTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts#GROUPBY](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#groupby)]  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
- [查询表达式](query-expressions-entity-sql.md)
