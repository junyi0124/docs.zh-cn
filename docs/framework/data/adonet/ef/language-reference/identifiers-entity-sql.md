---
title: 标识符 (Entity SQL)
ms.date: 03/30/2017
ms.assetid: d58a5edd-7b5c-48e1-b5d7-a326ff426aa4
ms.openlocfilehash: 7e9b12ca351b021fab62988969cb98310cb55cc2
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203691"
---
# <a name="identifiers-entity-sql"></a>标识符 (Entity SQL)

在 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 中，标识符用于表示查询表达式别名、变量引用、对象的属性、函数等等。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 提供两种标识符：简单标识符和带引号的标识符。  
  
## <a name="simple-identifiers"></a>简单标识符  

 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 中的简单标识符是字母数字和下划线字符的序列。 标识符的第一个字符必须是字母字符（a-z 或 A-Z）。  
  
## <a name="quoted-identifiers"></a>带引号的标识符  

 带引号的标识符是括在方括号 ([]) 中的任何字符序列。 使用带引号的标识符可以指定含有在标识符中无效的字符的标识符。 方括号之间的所有字符都将成为标识符的一部分，包括所有空格。  
  
 带引号的标识符不能包含以下字符：  
  
- 换行符。  
  
- 回车符。  
  
- 制表符。  
  
- Backspace。  
  
- 额外的方括号（即括起标识符的方括号中的方括号）。  
  
 带引号的标识符可以包含 Unicode 字符。  
  
 使用带引号的标识符可以创建在标识符中无效的属性名称字符，如下面的示例所示：  
  
 `SELECT c.ContactName AS [Contact Name] FROM customers AS c`  
  
 使用带引号的标识符还可以指定属于 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 保留关键字的标识符。 例如，如果类型 `Email` 具有名为“From”的属性，则可以使用方括号来消除与保留关键字“FROM”的歧义，如下所示：  
  
 `SELECT e.[From] FROM emails AS e`  
  
 可以在点 (.) 运算符的右侧使用带引号的标识符。  
  
 `SELECT t FROM ts as t WHERE t.[property] == 2`  
  
 若要在标识符中使用方括号，请再添加一个方括号。 在下面的示例中，“`abc]`”为标识符：  
  
 `SELECT t from ts as t WHERE t.[abc]]] == 2`  
  
 对于带引号的标识符比较语义，请参阅 [输入字符集](input-character-set-entity-sql.md)。  
  
## <a name="aliasing-rules"></a>别名规则  

 如果需要，建议在 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 查询中指定别名，包括以下 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 构造：  
  
- 行构造函数的字段。  
  
- 查询表达式的 FROM 子句中的项。  
  
- 查询表达式的 SELECT 子句中的项。  
  
- 查询表达式的 GROUP BY 子句中的项。  
  
### <a name="valid-aliases"></a>有效别名  

 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 中的有效别名包括任何简单标识符或带引号的标识符。  
  
### <a name="alias-generation"></a>别名生成  

 如果 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 查询表达式中未指定别名，则 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 将尝试根据以下简单规则来生成别名：  
  
- 如果查询表达式（对该查询表达式未指定别名）是一个简单标识符或带引号的标识符，则该标识符用作别名。 例如，`ROW(a, [b])` 重命名为 `ROW(a AS a, [b] AS [b])`。  
  
- 如果查询表达式是较复杂的表达式，但该查询表达式的最后一部分是简单标识符，则该标识符用作别名。 例如，`ROW(a.a1, b.[b1])` 重命名为 `ROW(a.a1 AS a1, b.[b1] AS [b1])`。  
  
 如果希望以后使用该别名，建议不要使用隐式别名。 别名（隐式或显式）如果冲突，或在同一作用域内重复，都会出现编译错误。 即使存在同名的显式或隐式别名，隐式别名也会通过编译。  
  
 隐式别名是根据用户输入自动生成的。 例如，下面一行代码将生成 NAME 作为两个列的别名，从而发生冲突。  
  
```sql  
SELECT product.NAME, person.NAME  
```  
  
 下面使用显式别名的代码行也会失败。 但是，通过阅读代码，失败会更为明显。  
  
```sql  
SELECT 1 AS X, 2 AS X …  
```  
  
## <a name="scoping-rules"></a>作用域规则  

 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 定义作用域规则，用来确定查询语言中的特定变量何时可见。 一些表达式或语句引入了新名称。 作用域规则确定在何处可以使用这些名称，与另一声明同名的新声明何时或在何处可以隐藏其前置任务。  
  
 在 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 查询中定义了名称，即是在作用域内定义了名称。 作用域包括查询的整个区域。 在作用域内定义的名称对该作用域的所有表达式或名称引用都可见。 作用域内定义的名称不能在作用域开始之前和结束之后进行引用。  
  
 作用域可以嵌套。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 的组成部分引入了包含整个区域的新作用域，这些区域可以包含也引入作用域的其他 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 表达式。 当作用域嵌套时，可以对包含引用的最内层作用域中定义的名称进行引用。 还可以对任一外层作用域中定义的任何名称进行引用。 在同一作用域内定义的任何两个作用域可视为同级作用域。 不能引用同级作用域内定义的名称。  
  
 如果内层作用域中声明的名称与外层作用域中声明的名称相同，则在内层作用域或其内部的作用域内声明的作用域内的引用只能引用新声明的名称。 外层作用域中的名称为隐藏状态。  
  
 即使在同一作用域内，名称也不能在定义前引用。  
  
 全局名称可以作为执行环境的一部分存在。 包括持久集合或环境变量的名称。 若要使名称成为全局名称，必须在最外层作用域中声明该名称。  
  
 参数不在一个作用域内。 由于对参数的引用包含特殊语法，参数名称不会与查询中的其他名称冲突。  
  
### <a name="query-expressions"></a>查询表达式  

 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 查询表达式引入一个新的作用域。 在 FROM 子句中定义的名称是以从左到右的显示顺序引入“from”作用域的。 在联接列表中，表达式可以引用列表前面定义的名称。 在 FROM 子句中标识的元素的公共属性（字段等）不添加到“from”作用域。 它们必须始终由别名限定名称引用。 通常，SELECT 表达式的所有部分都被视为在“from”作用域内。  
  
 GROUP BY 子句也引入一个新的同级作用域。 每个组都可以有一个引用组中元素集合的组名称。 每个分组表达式也将向“group”作用域引入一个新名称。 此外，嵌套聚合（即命名组）也添加到作用域中。 分组表达式本身在“from”作用域中。 但是，当使用 GROUP BY 子句时，选择列表（投影）、HAVING 子句和 ORDER BY 子句被视为在“group”作用域中，而不是在“from”作用域中。 聚合有特殊的处理方式，如下列内容所述。  
  
 以下是对作用域的附加说明：  
  
- 选择列表可将新名称按顺序引入作用域。 右侧的投影表达式可以引用投影在左侧的名称。  
  
- ORDER BY 子句可以引用选择列表中指定的名称（别名）。  
  
- SELECT 表达式内的子句计算顺序确定名称引入作用域的顺序。 计算的顺序首先是 FROM 子句，其次是 WHERE 子句、GROUP BY 子句、HAVING 子句、SELECT 子句，最后是 ORDER BY 子句。  
  
### <a name="aggregate-handling"></a>聚合处理  

 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 支持两种形式的聚合：基于集合的聚合和基于组的聚合。 基于集合的聚合是 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 的首选构造，支持基于组的聚合则是为了实现 SQL 兼容性。  
  
 在解析聚合时，[!INCLUDE[esql](../../../../../../includes/esql-md.md)] 首先尝试将它视为基于集合的聚合。 如果失败，[!INCLUDE[esql](../../../../../../includes/esql-md.md)] 将聚合输入转换为对嵌套聚合的引用，并尝试解析这个新的表达式，如下面的示例所示。  
  
 `AVG(t.c) becomes AVG(group..(t.c))`  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
- [Entity SQL 概述](entity-sql-overview.md)
- [输入字符集](input-character-set-entity-sql.md)
