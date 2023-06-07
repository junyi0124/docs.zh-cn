---
title: USING (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 20f58b8f-6070-4456-b7e8-5ff3d6269273
ms.openlocfilehash: bdab81ed8acae5e757de0a669922dc79d0303ee4
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91203587"
---
# <a name="using-entity-sql"></a>USING (Entity SQL)

指定查询表达式中使用的命名空间。  
  
## <a name="syntax"></a>语法  
  
```sql  
USING [ alias = ] namespace  
```  
  
## <a name="arguments"></a>参数  

 `alias`  
 指定用于限定命名空间的较短别名。  
  
 `namespace`  
 任何有效的命名空间。  
  
## <a name="example"></a>示例  

 以下 Entity SQL 查询使用 USING 运算符以指定查询表达式中使用的命名空间。 若要编译并运行此查询，请执行下列步骤：  
  
1. 按照 [如何：执行返回 PrimitiveType 结果的查询](../how-to-execute-a-query-that-returns-primitivetype-results.md)中的过程进行操作。  
  
2. 将以下查询作为参数传递给 `ExecutePrimitiveTypeQuery` 方法：  
  
```csharp
using SqlServer; RAND()  
```  
  
## <a name="see-also"></a>请参阅

- [命名空间](namespaces-entity-sql.md)
- [实体 SQL 引用](entity-sql-reference.md)
