---
title: FUNCTION (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 0bb88992-37ed-4991-ace5-55be612a2c4d
ms.openlocfilehash: 4e06b5bf8a2ca62630666ab3e8ba35f0425e3988
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91148030"
---
# <a name="function-entity-sql"></a>FUNCTION (Entity SQL)

在 Entity SQL 查询命令的范围内定义函数。  
  
## <a name="syntax"></a>语法  
  
```sql  
FUNCTION function-name  
( [ { parameter_name <type_definition>
        [ ,...n ]  
  ]  
) AS ( function_expression )
  
<type_definition>::=  
    { data_type | COLLECTION ( <type_definition> )
                | REF ( data_type )
                | ROW ( row_expression )
        }
```  
  
## <a name="arguments"></a>参数  

 `function-name`  
 函数的名称。  
  
 `parameter-name`  
 函数中参数的名称。  
  
 `function_expression`  
 属于函数的有效 Entity SQL 表达式。 函数中的命令，可以作用于传递给函数的 `parameter_name` 参数。  
  
 `data_type`  
 受支持类型的名称。  
  
 集合 ( <type_definition `>` )   
 一个表达式，返回受支持类型、行或引用的集合。  
  
 引用** (** `data_type` **) **  
 一个表达式，返回对实体类型的引用。  
  
 行** (** `row_expression` **) **  
 一个表达式，从一个或多个值返回结构上类型化的匿名记录。 有关更多信息，请参见 [ROW](row-entity-sql.md)。  
  
## <a name="remarks"></a>备注  

 可以内联声明具有相同名称的多个函数，只要这些函数的签名不同即可。 有关详细信息，请参阅 [Function Overload Resolution](function-overload-resolution-entity-sql.md)。  
  
 只有在 Entity SQL 命令中定义了内联函数后，才可以在该命令中调用内联函数。 但是，在定义被调函数之前或之后，都可以在一个内联函数中调用另一个内联函数。 在下面的示例中，函数 A 在定义函数 B 之前调用函数 B：  
  
 `Function A() as ('A calls B. ' + B())`  
  
 `Function B() as ('B was called.')`  
  
 `A()`  
  
 有关详细信息，请参阅 [如何：调用用户定义的函数](/previous-versions/dotnet/netframework-4.0/dd490951(v=vs.100))。  
  
 还可以在模型本身中声明函数。 在模型中声明的函数的执行方式与在命令中内联声明的函数的执行方式相同。 有关详细信息，请参阅 [用户定义函数](user-defined-functions-entity-sql.md)。  
  
## <a name="example"></a>示例  

 下面的 Entity SQL 命令定义一个函数 `Products` ，该函数采用整数值来筛选返回的产品。  
  
 [!code-sql[DP EntityServices Concepts#FUNCTION1](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#function1)]  
  
## <a name="example"></a>示例  

 下面的 Entity SQL 命令定义一个函数 `StringReturnsCollection` ，该函数采用字符串集合来筛选返回的联系人。  
  
 [!code-sql[DP EntityServices Concepts#FUNCTION2](~/samples/snippets/tsql/VS_Snippets_Data/dp entityservices concepts/tsql/entitysql.sql#function2)]  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
- [Entity SQL 语言](entity-sql-language.md)
