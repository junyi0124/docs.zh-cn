---
title: WHERE (Entity SQL)
ms.date: 03/30/2017
ms.assetid: a8e1061e-0028-4a6f-8f19-b9f48e96c4b8
ms.openlocfilehash: 1907b8786622d3c8019c75916f997c830cc07cfb
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91180954"
---
# <a name="where-entity-sql"></a>WHERE (Entity SQL)

WHERE 子句直接应用于 [from](from-entity-sql.md) 子句之后。  
  
## <a name="syntax"></a>语法  
  
```sql  
[ WHERE expression ]  
```  
  
## <a name="arguments"></a>参数  

 `expression`  
 Boolean 类型。  
  
## <a name="remarks"></a>备注  

 WHERE 子句具有与 Transact-sql 所述相同的语义。 它将源集合的元素限定为传递条件的元素，以此限制查询表达式所生成的对象。  
  
```sql  
select c from cs as c where e  
```  
  
 表达式 `e` 必须为 Boolean 类型。  
  
 WHERE 子句直接应用在 FROM 子句之后，任何分组、排序或投影操作之前。 FROM 子句中定义的所有元素名称对 WHERE 子句的表达式都是可见的。  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
- [查询表达式](query-expressions-entity-sql.md)
