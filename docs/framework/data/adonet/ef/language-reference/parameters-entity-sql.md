---
title: 参数 (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 8d618edd-0988-4ff2-8263-ce59448af7a5
ms.openlocfilehash: 759452902461e1a460b69774bb33f92bbd532ed0
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91177509"
---
# <a name="parameters-entity-sql"></a>参数 (Entity SQL)

参数是在 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 之外（通常通过由主机语言使用的绑定 API）定义的变量。 每个参数都具有名称和类型。 参数名称在查询表达式中定义，并以 (@) 符号作为前缀。 这可以将它们与属性的名称或在查询中定义的其他名称区分开来。  
  
 主机语言绑定 API 提供用于绑定参数的 API。  
  
## <a name="example"></a>示例  
  
```sql  
SELECT c
      FROM LOB.Customers AS c
      WHERE c.Name = @name  
```  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
- [Entity SQL 概述](entity-sql-overview.md)
