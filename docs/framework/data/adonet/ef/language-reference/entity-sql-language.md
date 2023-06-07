---
title: Entity SQL 语言
ms.date: 03/30/2017
ms.assetid: 9e7d8837-28c5-429d-a824-7bafb59724cf
ms.openlocfilehash: 721a4cd9d4e5618c083392bbe1ae203f285f8feb
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91148108"
---
# <a name="entity-sql-language"></a>Entity SQL 语言

Entity SQL 是类似于 SQL 的与存储无关的查询语言。 通过 Entity SQL，可以将实体数据作为对象或以表格形式进行查询。 在以下情况下，应考虑使用 Entity SQL：  
  
- 当查询必须在运行时动态构造时。 在这种情况下，还应考虑使用 <xref:System.Data.Objects.ObjectQuery%601> 的查询生成器方法，而不是在运行时构造 Entity SQL 查询字符串。  
  
- 当您要将查询定义为模型定义的一部分时。 在数据模型中只支持 Entity SQL。 有关详细信息，请参阅 [QueryView 元素 (MSL) ](/ef/ef6/modeling/designer/advanced/edmx/msl-spec#queryview-element-msl)  
  
- 当使用 EntityClient，通过 <xref:System.Data.EntityClient.EntityDataReader> 将只读实体数据返回为行集时。 有关详细信息，请参阅 [用于 Entity Framework 的 EntityClient 提供程序](../entityclient-provider-for-the-entity-framework.md)。  
  
- 如果您已经是基于 SQL 的查询语言的专家，Entity SQL 可能对您而言是最简单不过了。  
  
## <a name="using-entity-sql-with-the-entityclient-provider"></a>将 Entity SQL 与 EntityClient 提供程序结合使用  

 如果您要将 Entity SQL 与 EntityClient 提供程序结合使用，有关更多信息请参见下列主题：  
  
 [用于实体框架的 EntityClient 提供程序](../entityclient-provider-for-the-entity-framework.md)  
  
 [如何：生成 EntityConnection 连接字符串](../how-to-build-an-entityconnection-connection-string.md)  
  
 [如何：执行返回 PrimitiveType 结果的查询](../how-to-execute-a-query-that-returns-primitivetype-results.md)  
  
 [如何：执行返回 StructuralType 结果的查询](../how-to-execute-a-query-that-returns-structuraltype-results.md)  
  
 [如何：执行返回 RefType 结果的查询](../how-to-execute-a-query-that-returns-reftype-results.md)  
  
 [如何：执行返回复杂类型的查询](../how-to-execute-a-query-that-returns-complex-types.md)  
  
 [如何：执行返回嵌套集合的查询](../how-to-execute-a-query-that-returns-nested-collections.md)  
  
 [如何：使用 EntityCommand 执行参数化实体 SQL 查询](../how-to-execute-a-parameterized-entity-sql-query-using-entitycommand.md)  
  
 [如何：使用 EntityCommand 执行参数化存储过程](../how-to-execute-a-parameterized-stored-procedure-using-entitycommand.md)  
  
 [如何：执行多态查询](../how-to-execute-a-polymorphic-query.md)  
  
 [如何：使用导航运算符导航关系](../how-to-navigate-relationships-with-the-navigate-operator.md)  
  
## <a name="using-entity-sql-with-object-queries"></a>将 Entity SQL 与对象查询结合使用  

 如果您要将 Entity SQL 与对象查询结合使用，有关更多信息请参见下列主题：  
  
 [如何：执行返回实体类型对象的查询](/previous-versions/dotnet/netframework-4.0/bb738694(v=vs.100))  
  
 [如何：执行参数化查询](/previous-versions/dotnet/netframework-4.0/bb738521(v=vs.100))  
  
 [如何：使用导航属性导航关系](/previous-versions/dotnet/netframework-4.0/bb896321(v=vs.100))  
  
 [如何：调用用户定义的函数](/previous-versions/dotnet/netframework-4.0/dd490951(v=vs.100))  
  
 [如何：筛选数据](/previous-versions/dotnet/netframework-4.0/cc716755(v=vs.100))  
  
 [如何：对数据排序](/previous-versions/dotnet/netframework-4.0/cc716784(v=vs.100))  
  
 [如何：对数据分组](/previous-versions/dotnet/netframework-4.0/bb896341(v=vs.100))  
  
 [如何：聚合数据](/previous-versions/dotnet/netframework-4.0/cc716738(v=vs.100))  
  
 [如何：执行返回匿名类型对象的查询](/previous-versions/dotnet/netframework-4.0/bb738512(v=vs.100))  
  
 [如何：执行返回基元类型集合的查询](/previous-versions/dotnet/netframework-4.0/bb738451(v=vs.100))  
  
 [如何：在 EntityCollection 中查询相关对象](/previous-versions/dotnet/netframework-4.0/cc716708(v=vs.100))  
  
 [如何：对两个查询的联合排序](/previous-versions/dotnet/netframework-4.0/bb896299(v=vs.100))  
  
 [如何：按页查看查询结果](/previous-versions/dotnet/netframework-4.0/bb738702(v=vs.100))  
  
## <a name="in-this-section"></a>本节内容  

 [Entity SQL 概述](entity-sql-overview.md)  
  
 [实体 SQL 引用](entity-sql-reference.md)  
  
## <a name="see-also"></a>请参阅

- [ADO.NET 实体框架](../index.md)
- [语言参考](index.md)
