---
title: 命名空间 (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 83991c21-60db-4af9-aca3-b416f6cae98e
ms.openlocfilehash: 7a53f8e7e70dbc9fa505f7f8619af10a0e44c331
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91197802"
---
# <a name="namespaces-entity-sql"></a>命名空间 (Entity SQL)

[!INCLUDE[esql](../../../../../../includes/esql-md.md)] 引入命名空间以避免全局标识符（如类型名称、实体集、函数等）出现名称冲突。 中的命名空间支持 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 与 .NET Framework 中的命名空间支持类似。  
  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 提供两种形式的 USING 子句：限定命名空间（其中，提供较短的别名以表示命名空间）和非限定命名空间，如下例所示：  
  
 `USING System.Data;`  
  
 `USING tsql = System.Data;`  
  
## <a name="name-resolution-rules"></a>名称解析规则  

 如果无法在本地作用域中解析标识符，则会 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 尝试在全局作用域中查找命名空间)  (名称。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 首先尝试将标识符（前缀）与其中一个限定的命名空间匹配。 如果匹配，则 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 尝试在指定的命名空间中解析标识符的其余部分。 如果未找到匹配项，则将引发异常。  
  
 接下来，[!INCLUDE[esql](../../../../../../includes/esql-md.md)] 尝试搜索所有非限定命名空间（在序言中指定）以查找该标识符。 如果只能在一个命名空间中找到此标识符，则返回该位置。 如果多个命名空间对于该标识符具有匹配项，则引发异常。 如果对于此标识符无法确定任何命名空间，则 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 将名称传递到下一个外部范围（<xref:System.Data.Common.DbCommand> 或 <xref:System.Data.Common.DbConnection> 对象），如下例中所示：  
  
```sql  
SELECT TREAT(p AS NamespaceName.Employee)  
FROM ContainerName.Person AS p  
WHERE p IS OF (NamespaceName.Employee)  
```  
  
## <a name="differences-from-the-net-framework"></a>.NET Framework 中的不同之处  

 在 .NET Framework 中，您可以使用部分限定的命名空间。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 不允许这样做。  
  
## <a name="adonet-usage"></a>ADO.NET 使用  

 查询是通过 ADO.NET <xref:System.Data.Common.DbCommand> 对象表示的。 可以在 <xref:System.Data.Common.DbCommand> 对象之上构建 <xref:System.Data.Common.DbConnection> 对象。 也可以将命名空间指定为 <xref:System.Data.Common.DbCommand> 和 <xref:System.Data.Common.DbConnection> 对象的一部分。 如果 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 无法在查询自身中解析标识符，则将探测外部命名空间（基于类似规则）。  
  
## <a name="see-also"></a>请参阅

- [实体 SQL 引用](entity-sql-reference.md)
- [Entity SQL 概述](entity-sql-overview.md)
