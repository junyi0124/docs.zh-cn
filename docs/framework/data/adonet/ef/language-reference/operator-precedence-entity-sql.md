---
title: 运算符优先级 (Entity SQL)
ms.date: 03/30/2017
ms.assetid: e92e4ca5-2889-4266-9625-47f0eb01a948
ms.openlocfilehash: f8aa0f213a24d6431d8910af849571a67fbd9f57
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91175637"
---
# <a name="operator-precedence-entity-sql"></a>运算符优先级 (Entity SQL)

当 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 查询具有多个运算符时，运算符优先级确定执行操作的顺序。 执行顺序可能对查询结果有明显的影响。  
  
 运算符的优先级别如下表中所示。 在较低级别的运算符之前先对较高级别的运算符进行求值。  
  
|级别|操作类型|操作员|  
|-----------|--------------------|--------------|  
|1|主要|`. , [] ()`|  
|2|一元|`! not`|  
|3|乘法|`* / %`|  
|4|加法|`+ -`|  
|5|中间件排序|`< > <= >=`|  
|6|等式|`= != <>`|  
|7|条件“与”|`and &&`|  
|8|条件“或”|`or &#124;&#124;`|  
  
 当一个表达式中的两个运算符有相同的运算符优先级别时，将按照它们在查询中的位置对其从左到右进行求值。 例如，`x+y-z` 将计算为 `(x+y)-z`。  
  
 在查询中可以使用括号替代所定义的运算符的优先级。 首先对括号中的内容进行求值，从而产生一个结果，然后括号外的运算符才可以使用这个结果。 例如，将 `x+y*z` 乘以 `y` ， `z` 然后添加 `x` ，但 `(x+y)*z` 将添加 `x` 到，然后将结果与 `y` 相乘 `z` 。  
  
## <a name="see-also"></a>请参阅

- [Entity SQL 概述](entity-sql-overview.md)
