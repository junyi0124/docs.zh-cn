---
title: 用于实体框架的 EntityClient 提供程序
ms.date: 03/30/2017
ms.assetid: 8c5db787-78e6-4a34-8dc1-188bca0aca5e
ms.openlocfilehash: 9b3194e4a85eda6f405a3ab85723ac2cb9d00090
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91181071"
---
# <a name="entityclient-provider-for-the-entity-framework"></a>用于实体框架的 EntityClient 提供程序

EntityClient 提供程序是一种数据提供程序，实体框架应用程序使用该提供程序访问在概念模型中描述的数据。 有关概念模型的信息，请参阅 [建模和映射](modeling-and-mapping.md)。 EntityClient 使用其他 .NET Framework 数据提供程序访问数据源。 例如，EntityClient 在访问 SQL Server 数据库时使用 SQL Server .NET Framework 数据提供程序 (SqlClient)。 有关 SqlClient 提供程序的信息，请参阅 [SqlClient for the 实体框架](sqlclient-for-the-entity-framework.md)。 EntityClient 提供程序是在 <xref:System.Data.EntityClient> 命名空间中实现的。  
  
## <a name="managing-connections"></a>管理连接  

 实体框架通过 <xref:System.Data.EntityClient.EntityConnection> 向基础数据提供程序和关系数据库提供，在存储特定的 ADO.NET 数据提供程序的基础上建立。 若要构造 <xref:System.Data.EntityClient.EntityConnection> 对象，你必须引用一组包含所需模型和映射的元数据，以及特定于存储的数据提供程序名称和连接字符串。 <xref:System.Data.EntityClient.EntityConnection>准备就绪后，可以通过从概念模型生成的类访问实体。  
  
 可以在 app.config 文件中指定连接字符串。  
  
 <xref:System.Data.EntityClient> 还包含 <xref:System.Data.EntityClient.EntityConnectionStringBuilder> 类。 通过使用此类的属性和方法，开发人员可以使用此类以编程方式创建语法正确的连接字符串，并可以分析和重新生成现有的连接字符串。 有关详细信息，请参阅 [如何：生成 EntityConnection 连接字符串](how-to-build-an-entityconnection-connection-string.md)。  
  
## <a name="creating-queries"></a>创建查询  

 [!INCLUDE[esql](../../../../../includes/esql-md.md)]语言是一种独立于存储的 SQL 方言，可直接处理概念实体架构，并支持实体数据模型的概念（例如继承和关系）。 <xref:System.Data.EntityClient.EntityCommand> 类用于对实体模型执行 [!INCLUDE[esql](../../../../../includes/esql-md.md)] 命令。 构造 <xref:System.Data.EntityClient.EntityCommand> 对象时，可以指定一个存储过程名称或查询文本。 实体框架与存储特定的数据提供程序一起使用，以将泛型转换为 [!INCLUDE[esql](../../../../../includes/esql-md.md)] 特定于存储的查询。 有关编写查询的详细信息 [!INCLUDE[esql](../../../../../includes/esql-md.md)] ，请参阅 [实体 SQL 语言](./language-reference/entity-sql-language.md)。  
  
 下面的示例创建一个 <xref:System.Data.EntityClient.EntityCommand> 对象，并 [!INCLUDE[esql](../../../../../includes/esql-md.md)] 将查询文本分配给其 <xref:System.Data.EntityClient.EntityCommand.CommandText%2A?displayProperty=nameWithType> 属性。 此 [!INCLUDE[esql](../../../../../includes/esql-md.md)] 查询请求从概念模型中按标价排序的产品。 下面的代码完全不识别存储模型。  
  
 ```csharp
EntityCommand cmd = conn.CreateCommand();
cmd.CommandText = @"SELECT VALUE p
  FROM AdventureWorksEntities.Product AS p
  ORDER BY p.ListPrice";
```
  
## <a name="executing-queries"></a>执行查询  

 执行查询时，查询将经过解析并转换为规范命令目录树。 所有后续处理都在该命令目录树上执行。 命令目录树是 <xref:System.Data.EntityClient> 与基础 .NET Framework 数据访问接口（如）之间的通信方式 <xref:System.Data.SqlClient> 。  
  
 <xref:System.Data.EntityClient.EntityDataReader> 公开对概念模型执行 <xref:System.Data.EntityClient.EntityCommand> 的结果。 若要执行返回 <xref:System.Data.EntityClient.EntityDataReader> 的命令，请调用 <xref:System.Data.EntityClient.EntityCommand.ExecuteReader%2A>。 <xref:System.Data.EntityClient.EntityDataReader> 实现 <xref:System.Data.IExtendedDataRecord> 以描述丰富结构化的结果。  
  
## <a name="managing-transactions"></a>管理事务  

 在实体框架中，有两种使用事务的方法：自动和显式。 自动事务使用 <xref:System.Transactions> 命名空间，而显式事务使用 <xref:System.Data.EntityClient.EntityTransaction> 类。  
  
 若要更新通过概念模型公开的数据，请参阅 [如何：在实体框架中管理事务](/previous-versions/dotnet/netframework-4.0/bb738523(v=vs.100))。  
  
## <a name="in-this-section"></a>本节内容  

 [如何：生成 EntityConnection 连接字符串](how-to-build-an-entityconnection-connection-string.md)  
  
 [如何：执行返回 PrimitiveType 结果的查询](how-to-execute-a-query-that-returns-primitivetype-results.md)  
  
 [如何：执行返回 StructuralType 结果的查询](how-to-execute-a-query-that-returns-structuraltype-results.md)  
  
 [如何：执行返回 RefType 结果的查询](how-to-execute-a-query-that-returns-reftype-results.md)  
  
 [如何：执行返回复杂类型的查询](how-to-execute-a-query-that-returns-complex-types.md)  
  
 [如何：执行返回嵌套集合的查询](how-to-execute-a-query-that-returns-nested-collections.md)  
  
 [如何：使用 EntityCommand 执行参数化实体 SQL 查询](how-to-execute-a-parameterized-entity-sql-query-using-entitycommand.md)  
  
 [如何：使用 EntityCommand 执行参数化存储过程](how-to-execute-a-parameterized-stored-procedure-using-entitycommand.md)  
  
 [如何：执行多态查询](how-to-execute-a-polymorphic-query.md)  
  
 [如何：使用导航运算符导航关系](how-to-navigate-relationships-with-the-navigate-operator.md)  
  
## <a name="see-also"></a>请参阅

- [管理连接和事务](/previous-versions/dotnet/netframework-4.0/bb896325(v=vs.100))
- [ADO.NET 实体框架](index.md)
- [语言参考](./language-reference/index.md)
