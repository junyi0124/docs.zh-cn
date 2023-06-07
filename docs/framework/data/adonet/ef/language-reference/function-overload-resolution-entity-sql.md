---
title: 函数重载解析 (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 9c648054-3808-4a69-9d3e-98e6a4f9c5ca
ms.openlocfilehash: d37cd9342d1fb3b60d5a2c05d373fb7e71f54b1f
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91189391"
---
# <a name="function-overload-resolution-entity-sql"></a>函数重载解析 (Entity SQL)

本主题描述如何解析 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 函数。  
  
 可以使用相同的名称定义一个以上的函数，只要这些函数具有唯一的签名即可。  
  
 在发生这种情况时，必须应用下面的条件来确定给定的表达式引用哪个函数。 这些条件是按顺序应用的。 第一个仅适用于单个函数的条件所对应的函数就是所解析的函数。  
  
1. **参数编号**。 函数在表达式中指定了相同个数的参数。  
  
2. **类型完全匹配**。 函数的每个自变量类型都严格匹配参数类型，或为 null 字面值。  
  
3. **子类型上的匹配**。 函数的每个自变量类型都严格匹配参数类型或为参数类型的子类型，或为 null 字面值。 如果多个函数的唯一区别是所需的子类型转换的个数，则子类型转换个数最小的函数就是所解析的函数。  
  
4. **子类型或类型升级的匹配项**。 函数的每个实参类型都严格匹配形参类型，或为形参类型的子类型，或可以提升到形参类型，或为 null 字面值。 同样，如果多个函数的唯一区别是子类型转换和提升的个数，则子类型转换和提升个数最小的函数就是所解析的函数。  
  
 如果上述任何条件都无法导致选择单个函数，则函数调用表达式具有多义性。  
  
 即使可以使用上述规则推断出单个函数，实参仍然可能不匹配形参。 在这种情况下，将引发错误。  
  
 对于用户定义的函数，内联查询函数的定义将优先，即使当模型定义的函数存在且具有一个更适合用户定义函数的签名也不例外。  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
- [Entity SQL 概述](entity-sql-overview.md)
- [函数](functions-entity-sql.md)
