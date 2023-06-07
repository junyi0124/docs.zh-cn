---
title: 变量 (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 3eed222a-f8f6-46b6-9cd5-220cc0e4e5d8
ms.openlocfilehash: af6d586a22f14a04bfc7ec339d0aa8e9ba7c66c7
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91180993"
---
# <a name="variables-entity-sql"></a>变量 (Entity SQL)

## <a name="variable"></a>变量  

 变量表达式是对当前作用域中定义的命名表达式的引用。 变量引用必须是 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] [标识符](identifiers-entity-sql.md)中定义的有效标识符。  
  
 以下示例演示如何在表达式中使用变量。 FROM 子句中的 `c` 是变量定义。 在 SELECT 子句中使用的 `c` 表示变量引用。  
  
```sql  
select c
from LOB.customers as c  
```  
  
## <a name="see-also"></a>请参阅

- [标识符](identifiers-entity-sql.md)
- [参数](parameters-entity-sql.md)
- [Entity SQL 概述](entity-sql-overview.md)
