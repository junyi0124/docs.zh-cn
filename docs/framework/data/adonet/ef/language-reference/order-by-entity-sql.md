---
title: ORDER BY (Entity SQL)
ms.date: 03/30/2017
ms.assetid: c0b61572-ecee-41eb-9d7f-74132ec8a26c
ms.openlocfilehash: 5e1c418a7f2bd40a42b259fb3784794b13098d7f
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91173674"
---
# <a name="order-by-entity-sql"></a>ORDER BY (Entity SQL)

指定用于 SELECT 语句所返回的对象的排序顺序。  
  
## <a name="syntax"></a>语法  
  
```sql  
[ ORDER BY
   {  
      order_by_expression [SKIP n] [LIMIT n]  
      [ COLLATE collation_name ]  
      [ ASC | DESC ]  
   }  
   [ ,…n ]
]  
```  
  
## <a name="arguments"></a>参数  

 `order_by_expression`  
 指定作为排序依据的属性的任何有效查询表达式。 可以指定多个排序表达式。 ORDER BY 子句中排序表达式的顺序将决定排序后结果集的结构。  
  
 COLLATE {collation_name}  
 指定应根据 `collation_name`中所指定的排序规则执行 ORDER BY 运算。 COLLATE 仅适用于字符串表达式。  
  
 ASC  
 指定所指定属性中的值应按升序排序，即从最低值往最高值排序。 这是默认设置。  
  
 DESC  
 指定所指定属性中的值应按降序排序，即从最高值往最低值排序。  
  
 LIMIT `n`  
 仅选择前 `n` 个项。  
  
 SKIP `n`  
 跳过前 `n` 个项。  
  
## <a name="remarks"></a>备注  

 ORDER BY 子句在逻辑上应用于 SELECT 子句的结果。 ORDER BY 子句可以使用选择列表中各项的别名来引用这些项。 ORDER BY 子句也可引用当前处于作用域内的其他变量。 但如果用 DISTINCT 修饰符指定了 SELECT 子句，则 ORDER BY 子句只能引用 SELECT 子句中的别名。  
  
 `SELECT c AS c1 FROM cs AS c ORDER BY c1.e1, c.e2`  
  
 ORDER BY 子句中的每个表达式的计算结果都必须是可按顺序比较是否不等（小于或大于，等等）的某一类型。 这些类型通常为标量基元类型，如数字、字符串和日期。 可比较类型的 RowTypes 也可按顺序比较。  
  
 如果代码循环访问一个有序集（顶级投影除外），则不能确保输出会保持其顺序。  

在下面的示例中，保证保留顺序：

```sql  
SELECT C1.FirstName, C1.LastName  
        FROM AdventureWorks.Contact as C1  
        ORDER BY C1.LastName  
```  

在下面的查询中，将忽略嵌套查询的顺序：  

```sql  
SELECT C2.FirstName, C2.LastName  
    FROM (SELECT C1.FirstName, C1.LastName  
        FROM AdventureWorks.Contact as C1  
        ORDER BY C1.LastName) as C2  
```  
  
 若要执行有序的 UNION、UNION ALL、EXCEPT 或 INTERSECT 运算，请使用以下模式：  
  
```sql  
SELECT ...  
FROM ( UNION/EXCEPT/INTERSECT operation )  
ORDER BY ...  
```  
  
## <a name="restricted-keywords"></a>受限制的关键字  

 下列关键字在 `ORDER BY` 子句中使用时，必须用引号括起：  
  
- CROSS  
  
- FULL  
  
- KEY  
  
- LEFT  
  
- ORDER  
  
- OUTER  
  
- RIGHT  
  
- ROW  
  
- 值  
  
## <a name="ordering-nested-queries"></a>嵌套查询排序  

 在实体框架中，嵌套表达式可以放在查询的任何位置，不保留嵌套查询的顺序。  

下面的查询将按姓氏对结果进行排序：  

```sql  
SELECT C1.FirstName, C1.LastName  
        FROM AdventureWorks.Contact as C1  
        ORDER BY C1.LastName  
```  

在下面的查询中，将忽略嵌套查询的顺序：  

```sql  
SELECT C2.FirstName, C2.LastName  
    FROM (SELECT C1.FirstName, C1.LastName  
        FROM AdventureWorks.Contact as C1  
        ORDER BY C1.LastName) as C2  
```  
  
## <a name="example"></a>示例  

 下面的 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 查询使用 ORDER BY 运算符来指定用于 SELECT 语句所返回的对象的排序顺序。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 执行 [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md)中的过程。  
  
2. 将以下查询作为参数传递给 `ExecuteStructuralTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts#ORDERBY](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#orderby)]  
  
## <a name="see-also"></a>请参阅

- [查询表达式](query-expressions-entity-sql.md)
- [实体 SQL 引用](entity-sql-reference.md)
- [SKIP](skip-entity-sql.md)
- [上限](limit-entity-sql.md)
- [TOP](top-entity-sql.md)
