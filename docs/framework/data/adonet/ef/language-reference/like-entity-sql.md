---
title: LIKE (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 8300e6d2-875b-481e-9ef4-e1e7c12d46fa
ms.openlocfilehash: c4c2d6020e5355930dfa8880b0966dfe015baa51
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91161832"
---
# <a name="like-entity-sql"></a>LIKE (Entity SQL)

确定特定字符 `String` 是否与指定模式相匹配。  
  
## <a name="syntax"></a>语法  
  
```sql  
match [NOT] LIKE pattern [ESCAPE escape]  
```  
  
## <a name="arguments"></a>参数  

 `match`  
 计算结果为 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 的 `String` 表达式。  
  
 `pattern`  
 要与指定 `String` 匹配的模式。  
  
 `escape`  
 一个转义符。  
  
 NOT  
 指定对 LIKE 的结果取反。  
  
## <a name="return-value"></a>返回值  

 如果 `true` 与模式相匹配，则为 `string`；否则为 `false`。  
  
## <a name="remarks"></a>备注  

 使用 LIKE 运算符的 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 表达式的计算方式十分类似于将相等性用作筛选条件的表达式。 但是，使用 LIKE 运算符的 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 表达式可以包含文本和通配符。  
  
 下表说明模式 `string` 的语法。  
  
|通配符|描述|示例|  
|------------------------|-----------------|-------------|  
|%|包含零个或零个以上字符的任何 `string`。|`title like '%computer%'``"computer"`在标题中的任意位置查找所有标题。|  
|_（下划线）|任何单个字符。|`firstname like '_ean'` 查找以开头的所有四个字母的名字 `"ean` ，如 "Dean" 或小红。|  
|[ ]|指定范围 ([a-f]) 或集合 ([abcdef]) 中的任意单个字符。|`lastname like '[C-P]arsen'` 查找以 "arsen" 结尾并以 C 和 P 之间的任何单个字符开头的姓氏（如 Carsen 或 Larsen）。|  
|[^]|不在指定范围 ([^a-f]) 或集合 ([^abcdef]) 中的任意单个字符。|`lastname like 'de[^l]%'` 查找所有以 "de" 开头的名称，并且不包含 "l" 作为下一字母。|  
  
> [!NOTE]
> [!INCLUDE[esql](../../../../../../includes/esql-md.md)] LIKE 运算符和 ESCAPE 子句不适用于 `System.DateTime` 或 `System.Guid` 值。  
  
 LIKE 支持 ASCII 模式匹配和 Unicode 模式匹配。 当所有参数都为 ASCII 字符时，将执行 ASCII 模式匹配。 如果一个或多个参数为 Unicode，则所有参数都会转换为 Unicode，并执行 Unicode 模式匹配。 在将 Unicode 与 LIKE 一起使用时，尾随空格有意义；但对非 Unicode，尾随空格则没有意义。 的模式字符串语法与 Transact-sql 的模式字符串语法 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 相同。  
  
 模式可以包含常规字符和通配符。 模式匹配过程中，常规字符必须与在字符 `string` 中指定的字符完全匹配。 但是，通配符可以与字符串的任意部分相匹配。 在与通配符一起使用时，LIKE 运算符比 = 和 != 字符串比较运算符更为灵活。  
  
> [!NOTE]
> 如果是针对特定的提供程序，则可以使用特定于提供程序的扩展。 但是，其他提供程序可能以不同方式对待这类构造。 SqlServer 支持 [first-last] 和 [^first-last] 模式，前一个模式与介于“first”和“last”之间的一个字符完全匹配，而后一个模式与不在“first”和“last”之间的一个字符完全匹配。  
  
### <a name="escape"></a>Escape  

 使用 ESCAPE 子句可以搜索包含一个或多个特殊通配符（在上一部分的表中进行了介绍）的字符串。 例如，假定有几个文档在标题中包含文本“100%”，而您希望搜索所有这些文档。 由于百分号 (%) 字符是通配符，因此若要成功执行搜索，必须使用 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] ESCAPE 子句对百分号字符进行转义。 下面是一个此筛选的示例。  
  
```sql  
"title like '%100!%%' escape '!'"  
```  
  
 在此搜索表达式中，紧跟在惊叹号字符 (!) 之后的百分号通配符 (%) 除了 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 通配符和方括号 (`[ ]`) 字符之外，可以使用任何字符作为转义符。 在前一个示例中，惊叹号 (!) 字符是转义字符。  
  
## <a name="example"></a>示例  

 下面两个 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 查询使用 LIKE 和 ESCAPE 运算符确定特定字符串是否与指定模式匹配。 第一个查询搜索以字符 `Name` 开头的 `Down_`。 此查询使用了 ESCAPE 选项，因为下划线 (`_`) 为通配符。 如果不指定 ESCAPE 选项，则该查询将搜索所有以单词 `Name` 开头、后跟任意单个字符（下划线字符除外）的 `Down` 值。 这些查询基于 AdventureWorks 销售模型。 若要编译并运行此查询，请执行下列步骤：  
  
1. 按照 [如何：执行返回 PrimitiveType 结果的查询](../how-to-execute-a-query-that-returns-primitivetype-results.md)中的过程进行操作。  
  
2. 将以下查询作为参数传递给 `ExecutePrimitiveTypeQuery` 方法：  
  
 [!code-sql[DP EntityServices Concepts#LIKE](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#like)]  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
