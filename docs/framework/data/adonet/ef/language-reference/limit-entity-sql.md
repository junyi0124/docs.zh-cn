---
title: LIMIT (Entity SQL)
ms.date: 03/30/2017
ms.assetid: c22ffede-0a52-44d1-99b9-4a91e651e1b9
ms.openlocfilehash: 81d135785e567d46a105adcafbf083f48cb4868e
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91161758"
---
# <a name="limit-entity-sql"></a>LIMIT (Entity SQL)

在 ORDER BY 子句中使用 LIMIT 子子句可执行物理分页。 LIMIT 不能脱离 ORDER BY 子句单独使用。  
  
## <a name="syntax"></a>语法  
  
```sql  
[ LIMIT n ]  
```  
  
## <a name="arguments"></a>参数  

 `n`  
 将选择的项的数量。  
  
 如果 ORDER BY 子句中存在 LIMIT 表达式子子句，则将根据排序规范对查询排序，并且结果行数将受到 LIMIT 表达式限制。 例如，LIMIT 5 将结果集限制为 5 个实例或行。 LIMIT 的功能与 TOP 相当，区别之处是 LIMIT 要求 ORDER BY 子句存在。 SKIP 和 LIMIT 可独立与 ORDER BY 子句一起使用。  
  
> [!NOTE]
> 如果 TOP 修饰符和 SKIP 子子句出现在同一个查询表达式中，Entity SQL 查询将被视为无效。 应重写查询，将 TOP 表达式更改为 LIMIT 表达式。  
  
## <a name="example"></a>示例  

 下面的 Entity SQL 查询将 LIMIT 和 ORDER BY 运算符结合使用来指定用于 SELECT 语句所返回的对象的排序顺序。 此查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 执行 [How to: Execute a Query that Returns StructuralType Results](../how-to-execute-a-query-that-returns-structuraltype-results.md)中的过程。  
  
2. 将以下查询作为参数传递给 `ExecuteStructuralTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts#LIMIT](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#limit)]  
  
## <a name="see-also"></a>请参阅

- [ORDER BY](order-by-entity-sql.md)
- [如何：按页查看查询结果](/previous-versions/dotnet/netframework-4.0/bb738702(v=vs.100))
- [分页](paging-entity-sql.md)
- [TOP](top-entity-sql.md)
