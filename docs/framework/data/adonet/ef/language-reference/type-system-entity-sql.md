---
title: 类型系统 (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 818a505b-a196-41dd-aaac-2ccd5f7a2f1a
ms.openlocfilehash: d4c8ba7a9d9b58220455b50ff99960fa132c00c7
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91200987"
---
# <a name="type-system-entity-sql"></a>类型系统 (Entity SQL)

[!INCLUDE[esql](../../../../../../includes/esql-md.md)] 支持多种类型：  
  
- 基元（简单）类型，如 `Int32` 和 `String.`。  
  
- 在架构中定义的名义类型，如 <xref:System.Data.Metadata.Edm.EntityType>、<xref:System.Data.Metadata.Edm.ComplexType> 和 <xref:System.Data.Metadata.Edm.RelationshipType>。  
  
- 架构中未显式定义的匿名类型：<xref:System.Data.Metadata.Edm.CollectionType>、<xref:System.Data.Metadata.Edm.RowType> 和 <xref:System.Data.Metadata.Edm.RefType>。  
  
 本部分讨论架构中未显式定义但实体 SQL 支持的匿名类型。 有关基元类型和名义类型的信息，请参阅 [ (CSDL) 的概念模型类型 ](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec#conceptual-model-types-csdl)。  
  
## <a name="rows"></a>“行”  

 行的结构取决于该行所包含的类型化以命名成员的序列。 行类型没有标识，不能被继承。 如果同一行类型的实例的成员分别等效，则这些实例是等效的。 行不具有超出其结构等效项的行为，在公共语言运行库中没有等效项。 查询可产生包含行或行的集合的结构。 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 查询和主机语言之间的 API 绑定定义行在产生结果的查询中的实现方式。 有关如何构造行实例的信息，请参阅 [构造类型](constructing-types-entity-sql.md)。  
  
## <a name="collections"></a>集合  

 集合类型表示其他对象的零个或零个以上的实例。 有关如何构造集合的信息，请参阅 [构造类型](constructing-types-entity-sql.md)。  
  
## <a name="references"></a>参考  

 引用是指向特定实体集中的特定实体的逻辑指针。  
  
 [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 支持使用以下运算符对引用进行构造、解构和导航：  
  
- [REF](ref-entity-sql.md)  
  
- [CREATEREF](createref-entity-sql.md)  
  
- [KEY](key-entity-sql.md)  
  
- [DEREF](deref-entity-sql.md)  
  
 可以使用成员访问（点）运算符 (`.`) 对引用进行导航。 下面的代码段通过对 r（引用）属性进行导航提取 Order 的 Id 属性。  
  
```sql  
select o2.r.Id
from (select ref(o) as r from LOB.Orders as o) as o2
```  
  
 如果引用值为 null，或引用的目标不存在，则结果为 null。  
  
## <a name="see-also"></a>请参阅

- [Entity SQL 概述](entity-sql-overview.md)
- [实体 SQL 引用](entity-sql-reference.md)
- [CAST](cast-entity-sql.md)
- [CSDL、SSDL 和 MSL 规范](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)
