---
title: 性能注意事项（实体框架）
description: 了解 ADO.NET 的性能特征实体框架和注意事项，以帮助提高实体框架应用程序的性能。
ms.date: 03/30/2017
ms.assetid: 61913f3b-4f42-4d9b-810f-2a13c2388a4a
ms.openlocfilehash: 799ceff8b0bc370e929f2a4ad99b64a4fde4226a
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91156718"
---
# <a name="performance-considerations-entity-framework"></a>性能注意事项（实体框架）

本主题介绍 ADO.NET 实体框架的性能特征，并提供一些注意事项帮助改善实体框架应用程序的性能。  
  
## <a name="stages-of-query-execution"></a>查询执行的各个阶段  

 为了更好地了解查询在实体框架中的性能，了解当查询针对概念模型执行并将数据作为对象返回时发生的操作将很有帮助。 下表描述这一系列操作。  
  
|操作|相对成本|频率|注释|  
|---------------|-------------------|---------------|--------------|  
|加载元数据|中等|在每个应用程序域中一次。|实体框架使用的模型和映射元数据加载到 <xref:System.Data.Metadata.Edm.MetadataWorkspace> 中。 此元数据全局缓存，并可用于同一个应用程序域中的其他 <xref:System.Data.Objects.ObjectContext> 实例。|  
|打开数据库连接|中等<sup>1</sup>|根据需要。|由于与数据库的开放式连接会占用宝贵的资源，因此实体框架会根据需要打开和关闭数据库连接。 还可以显式打开连接。 有关详细信息，请参阅 [管理连接和事务](/previous-versions/dotnet/netframework-4.0/bb896325(v=vs.100))。|  
|生成视图|高|在每个应用程序域中一次。 （可以预生成。）|在实体框架可以针对概念模型执行查询或将更改保存到数据源之前，它必须生成一组本地查询视图才能访问数据库。 由于生成这些视图会产生很高的成本，因此，您可以在设计时预生成视图并将它们添加到项目。 有关详细信息，请参阅 [如何：预生成视图以提高查询性能](/previous-versions/dotnet/netframework-4.0/bb896240(v=vs.100))。|  
|准备查询|中等<sup>2</sup>|每个唯一查询一次。|包括编写查询命令、基于模型和映射元数据生成命令树和定义所返回数据的形状的成本。 因为实体 SQL查询命令和 LINQ 查询现已缓存，所以，以后执行相同查询所需的时间较少。 您仍可以使用已编译的 LINQ 查询来降低后续执行中的这一开销，编译的查询比自动缓存的 LINQ 查询效率更高。 有关详细信息，请参阅 [)  (LINQ to Entities 的编译查询 ](./language-reference/compiled-queries-linq-to-entities.md)。 有关 LINQ 查询执行的常规信息，请参阅 [LINQ to Entities](./language-reference/linq-to-entities.md)。 **注意：** `Enumerable.Contains` 不自动缓存将运算符应用于内存中集合的 LINQ to Entities 查询。 此外，不允许在已编译的 LINQ 查询中参数化内存中的集合。|  
|执行查询|低<sup>2</sup>|每个查询一次。|使用 ADO.NET 数据提供程序对数据源执行命令的成本。 因为大多数数据源缓存查询计划，所以，以后执行相同查询所需的时间可能较少。|  
|加载和验证类型|低<sup>3</sup>|每个 <xref:System.Data.Objects.ObjectContext> 实例一次。|将加载类型，并针对概念模型定义的类型对其进行验证。|  
|跟踪|低<sup>3</sup>|对于查询返回的每个对象执行一次。 <sup>4</sup>|如果查询使用 <xref:System.Data.Objects.MergeOption.NoTracking> 合并选项，则此阶段不影响性能。<br /><br /> 如果查询使用 <xref:System.Data.Objects.MergeOption.AppendOnly>、<xref:System.Data.Objects.MergeOption.PreserveChanges> 或 <xref:System.Data.Objects.MergeOption.OverwriteChanges> 合并选项，则将在 <xref:System.Data.Objects.ObjectStateManager> 中跟踪查询结果。 将为查询返回的每个跟踪对象生成一个 <xref:System.Data.EntityKey>，并用于在 <xref:System.Data.Objects.ObjectStateEntry> 中创建 <xref:System.Data.Objects.ObjectStateManager>。 如果对于 <xref:System.Data.Objects.ObjectStateEntry> 可以找到现有 <xref:System.Data.EntityKey>，则返回现有对象。 如果使用 <xref:System.Data.Objects.MergeOption.PreserveChanges> 或 <xref:System.Data.Objects.MergeOption.OverwriteChanges> 选项，则首先更新对象，然后返回此对象。<br /><br /> 有关详细信息，请参阅 [标识解析、状态管理和更改跟踪](/previous-versions/dotnet/netframework-4.0/bb896269(v=vs.100))。|  
|使对象具体化|中等<sup>3</sup>|对于查询返回的每个对象执行一次。 <sup>4</sup>|读取返回的 <xref:System.Data.Common.DbDataReader> 对象、创建对象以及设置属性值（这些值基于 <xref:System.Data.Common.DbDataRecord> 类的每个实例中的值）的过程。 如果对象已存在于 <xref:System.Data.Objects.ObjectContext> 中且查询使用 <xref:System.Data.Objects.MergeOption.AppendOnly> 或 <xref:System.Data.Objects.MergeOption.PreserveChanges> 合并选项，则此阶段不影响性能。 有关详细信息，请参阅 [标识解析、状态管理和更改跟踪](/previous-versions/dotnet/netframework-4.0/bb896269(v=vs.100))。|  
  
 <sup>1</sup> 当数据源提供程序实现连接池时，打开连接的成本分布在整个池中。 SQL Server 的 .NET 提供程序支持连接池。  
  
 <sup>2</sup> 增加了查询复杂性，增加了开销。  
  
 <sup>3</sup> 总成本随着查询返回的对象数的增加而增加。  
  
 <sup>4</sup> EntityClient 查询不需要此开销，因为 EntityClient 查询返回的 <xref:System.Data.EntityClient.EntityDataReader> 是而不是对象。 有关详细信息，请参阅 [用于 Entity Framework 的 EntityClient 提供程序](entityclient-provider-for-the-entity-framework.md)。  
  
## <a name="additional-considerations"></a>其他注意事项  

 下面是可能影响实体框架应用程序的性能的其他注意事项。  
  
### <a name="query-execution"></a>查询执行  

 因为查询可能消耗大量资源，所以请考虑一下在代码中的哪个点以及在哪台计算机上执行查询。  
  
#### <a name="deferred-versus-immediate-execution"></a>延迟执行与立即执行  

 当您创建 <xref:System.Data.Objects.ObjectQuery%601> 或 LINQ 查询时，查询可能并不立即执行。 查询延迟执行，直到您需要结果，例如，在 `foreach` (C#) 或 `For Each` (Visual Basic) 枚举期间或当分配此查询以填充 <xref:System.Collections.Generic.List%601> 集合时。 当您对 <xref:System.Data.Objects.ObjectQuery%601.Execute%2A> 调用 <xref:System.Data.Objects.ObjectQuery%601> 方法时，或当您调用返回单一查询（如 <xref:System.Linq.Enumerable.First%2A> 或 <xref:System.Linq.Enumerable.Any%2A>）的 LINQ 方法时，将立即开始执行查询。 有关详细信息，请参阅[ (LINQ to Entities) ](./language-reference/query-execution.md)的[对象查询](/previous-versions/dotnet/netframework-4.0/bb896241(v=vs.100))和查询执行。  
  
#### <a name="client-side-execution-of-linq-queries"></a>客户端执行 LINQ 查询  

 尽管 LINQ 查询执行发生在承载数据源的计算机上，但 LINQ 查询的某些部分可能在客户端计算机上进行计算。 有关详细信息，请参阅 " [查询执行" (LINQ to Entities) ](./language-reference/query-execution.md)的 "存储执行" 部分。  
  
### <a name="query-and-mapping-complexity"></a>查询和映射复杂性  

 实体模型中各个查询以及映射的复杂性对查询性能具有重大影响。  
  
#### <a name="mapping-complexity"></a>映射复杂性  

 比概念模型中实体之间和存储模型中表之间的简单一对一映射更复杂的模型与具有一对一映射的模型相比，前者将生成更复杂的命令。  
  
#### <a name="query-complexity"></a>查询复杂性  

 在针对数据源执行的命令或返回大量数据的命令中需要大量联接的查询可能会在以下几个方面影响性能：  
  
- 看似简单的针对概念模型的查询可能导致对数据源执行更复杂的查询。 因为实体框架将针对概念模型的查询转换为针对数据源的等效查询，所以可能发生上述情况。 当概念模型中的单个实体集映射到数据源中的多个表时，或当实体之间的关系映射到联接表时，针对数据源查询执行的查询命令可能需要一个或多个联接。  
  
    > [!NOTE]
    > 使用 <xref:System.Data.Objects.ObjectQuery.ToTraceString%2A> 或 <xref:System.Data.Objects.ObjectQuery%601> 类的 <xref:System.Data.EntityClient.EntityCommand> 方法可以查看针对给定查询的数据源执行的命令。 有关详细信息，请参阅 [如何：查看存储命令](/previous-versions/dotnet/netframework-4.0/bb896348(v=vs.100))。  
  
- 嵌套的 Entity SQL 查询可能在服务器上创建联接，并可能返回大量的行。  
  
     下面是投影子句中嵌套查询的示例：  
  
    ```sql  
    SELECT c, (SELECT c, (SELECT c FROM AdventureWorksModel.Vendor AS c  ) As Inner2
        FROM AdventureWorksModel.JobCandidate AS c  ) As Inner1
        FROM AdventureWorksModel.EmployeeDepartmentHistory AS c  
    ```  
  
     此外，此类查询还会导致查询管道生成单个查询并在各嵌套查询间复制对象。 因此，单一列可能会复制多次。 在某些数据库（包括 SQL Server）中，这可能导致 TempDB 表增长过大，而降低服务器性能。 当执行嵌套查询时，应务必小心。  
  
- 如果客户端执行的操作所消耗的资源与结果集的大小成正比，则返回大量数据的任何查询都可能导致性能下降。 在此类情况下，应考虑限制查询返回的数据量。 有关详细信息，请参阅 [如何：对查询结果进行分页](/previous-versions/dotnet/netframework-4.0/bb738702(v=vs.100))。  
  
 实体框架自动生成的任何命令都可能比数据库开发人员显式编写的类似命令更复杂。 如果您需要对针对数据源执行的命令进行显式控制，请考虑对表值函数或存储过程定义映射。  
  
#### <a name="relationships"></a>关系  

 若要获得最佳查询性能，必须将实体之间的关系同时定义为实体模型中的关联和数据源中的逻辑关系。  
  
### <a name="query-paths"></a>查询路径  

 默认情况下，当您执行 <xref:System.Data.Objects.ObjectQuery%601> 时，并不返回相关对象（但返回表示关系自身的对象）。 可以采用以下三种方法之一加载相关对象：  
  
1. 在执行 <xref:System.Data.Objects.ObjectQuery%601> 前设置查询路径。  
  
2. 对于对象公开的导航属性调用 `Load` 方法。  
  
3. 将 <xref:System.Data.Objects.ObjectContextOptions.LazyLoadingEnabled%2A> 上的 <xref:System.Data.Objects.ObjectContext> 选项设置为 `true`。 请注意，当你通过 [实体数据模型设计器](/previous-versions/dotnet/netframework-4.0/cc716685(v=vs.100))生成对象层代码时，会自动执行此操作。 有关详细信息，请参阅 [生成的代码概述](/previous-versions/dotnet/netframework-4.0/cc982041(v=vs.100))。  
  
 在考虑要使用哪个选项时，请注意在对数据库的请求数与单个查询中返回的数据量之间进行权衡。 有关详细信息，请参阅 [加载相关对象](/previous-versions/dotnet/netframework-4.0/bb896272(v=vs.100))。  
  
#### <a name="using-query-paths"></a>使用查询路径  

 查询路径定义查询返回的对象的图。 当定义查询路径时，仅需对数据库请求一次，即可返回该路径定义的所有对象。 如果使用查询路径，看似简单的对象查询也可能需要对数据源执行复杂的命令。 这是因为，在单个查询中返回相关对象需要一个或多个联接。 对复杂实体模型（如具有继承关系的实体或包含多对多关系的路径）进行的查询的复杂性将进一步加大。  
  
> [!NOTE]
> 使用 <xref:System.Data.Objects.ObjectQuery.ToTraceString%2A> 方法可以查看将由 <xref:System.Data.Objects.ObjectQuery%601> 生成的命令。 有关详细信息，请参阅 [如何：查看存储命令](/previous-versions/dotnet/netframework-4.0/bb896348(v=vs.100))。  
  
 如果查询路径包含过多相关对象，或对象包含过多行数据，数据源可能无法完成查询。 如果查询所需的中间临时存储区超过数据源的容量，则会出现这种情况。 出现这种情况时，通过显式加载相关对象可以降低数据源查询的复杂性。  
  
#### <a name="explicitly-loading-related-objects"></a>显式加载相关对象  

 通过对返回 `Load` 或 <xref:System.Data.Objects.DataClasses.EntityCollection%601> 的导航属性调用 <xref:System.Data.Objects.DataClasses.EntityReference%601> 方法，可以显式加载相关对象。 显式加载对象要求每次调用 `Load` 时都往返一次数据库。  
  
> [!NOTE]
> 如果在循环访问所返回对象的集合时调用 `Load`，例如，当您使用 `foreach` 语句（在 Visual Basic 中为 `For Each`）时，数据源特定的提供程序必须在单个连接上支持多个活动的结果集。 对于 SQL Server 数据库，必须在提供程序连接字符串中指定 `MultipleActiveResultSets = true` 值。  
  
 当实体没有 <xref:System.Data.Objects.ObjectContext.LoadProperty%2A> 或 <xref:System.Data.Objects.DataClasses.EntityCollection%601> 属性时，还可以使用 <xref:System.Data.Objects.DataClasses.EntityReference%601> 方法。 这在使用 POCO 实体时非常有用。  
  
 尽管显式加载相关对象将减少联接数并减少冗余数据量，但 `Load` 要求与数据库之间进行重复的连接，这在显式加载大量对象时可能会带来很高的成本。  
  
### <a name="saving-changes"></a>保存更改  

 当您对 <xref:System.Data.Objects.ObjectContext.SaveChanges%2A> 调用 <xref:System.Data.Objects.ObjectContext> 方法时，将为上下文中每个添加、更新或删除的对象生成一个单独的创建、更新或删除命令。 这些命令在单个事务中针对数据源执行。 对于查询，创建、更新和删除操作的性能取决于概念模型中映射的复杂性。  
  
### <a name="distributed-transactions"></a>分布式事务  

 在显式事务中执行的、需要由分布式事务处理协调器 (DTC) 管理的资源的操作与不需要 DTC 的相似操作相比，前者的成本要高得多。 在以下情况下，将需要提升到 DTC：  
  
- 具有对 SQL Server 2000 数据库执行的操作的显式事务，或始终将显式事务提升到 DTC 的其他数据源。  
  
- 当通过实体框架管理连接时，具有针对 SQL Server 2005 的操作的显式事务。 出现这种情况的原因是，每当关闭并在单个事务中重新打开连接时，SQL Server 2005 升级到 DTC，这是实体框架的默认行为。 当使用 SQL Server 2008 时，不会发生这种 DTC 提升的现象。 为了避免在使用 SQL Server 2005 时出现此提升，必须在事务中显式打开和关闭连接。 有关详细信息，请参阅 [管理连接和事务](/previous-versions/dotnet/netframework-4.0/bb896325(v=vs.100))。  
  
 当在 <xref:System.Transactions> 事务内执行了一个或多个操作时，将使用显式事务。 有关详细信息，请参阅 [管理连接和事务](/previous-versions/dotnet/netframework-4.0/bb896325(v=vs.100))。  
  
## <a name="strategies-for-improving-performance"></a>改善性能的策略  

 您可以通过以下策略改进实体框架中查询的总体性能：  
  
#### <a name="pre-generate-views"></a>预生成视图  

 当应用程序首次执行查询时，基于实体模型生成视图需要很高的成本。 使用 EdmGen.exe 实用工具可以将视图预生成为 Visual Basic 或 C# 代码文件，此文件可以在设计期间添加到项目中。 还可以使用文本模板转换工具包生成预编译的视图。 预生成的视图在运行时进行验证，以确保这些视图与指定实体模型的当前版本保持一致。 有关详细信息，请参阅 [如何：预生成视图以提高查询性能](/previous-versions/dotnet/netframework-4.0/bb896240(v=vs.100))。
  
 在使用非常大的模型时，适用以下注意事项：  
  
 .NET 元数据格式将一个给定的二进制文件中的用户字符串字符数限定为 16,777,215 (0xFFFFFF)。 如果你正在为非常大的模型生成视图，并且该视图文件达到此大小限制，则你将收到 "没有剩余的逻辑空间来创建更多用户字符串。" 编译错误。 此大小限制适用于所有托管二进制文件。 有关详细信息，请参阅 [博客](/archive/blogs/appfabriccat/solving-the-no-logical-space-left-to-create-more-user-strings-error-and-improving-performance-of-pre-generated-views-in-visual-studio-net4-entity-framework) ，其中演示了在处理大型和复杂模型时如何避免此错误。  
  
#### <a name="consider-using-the-notracking-merge-option-for-queries"></a>考虑对查询使用 NoTracking 合并选项  

 跟踪对象上下文中返回的对象会引发成本。 检测对象更改以及确保对于同一个逻辑实体的多个请求返回相同的对象实例均要求将对象附加到 <xref:System.Data.Objects.ObjectContext> 实例。 如果不打算对对象进行更新或删除操作，并且不需要标识管理，请考虑在 <xref:System.Data.Objects.MergeOption.NoTracking> 执行查询时使用合并选项。  
  
#### <a name="return-the-correct-amount-of-data"></a>返回正确的数据量  

 在某些方案下，使用 <xref:System.Data.Objects.ObjectQuery%601.Include%2A> 方法指定查询路径要快得多，因为它要求与数据库之间的往返次数较少。 然而，在其他方案中，加载相关对象时增加与数据库之间的往返次数可能会更快，因为联接较少的较为简单的查询会使数据冗余程度下降。 因此，我们建议你测试用于检索相关对象的不同方法的性能。 有关详细信息，请参阅 [加载相关对象](/previous-versions/dotnet/netframework-4.0/bb896272(v=vs.100))。  
  
 为了避免在单个查询中返回过多的数据，请考虑将查询结果分页为多个可管理的组。 有关详细信息，请参阅 [如何：对查询结果进行分页](/previous-versions/dotnet/netframework-4.0/bb738702(v=vs.100))。  
  
#### <a name="limit-the-scope-of-the-objectcontext"></a>限制 ObjectContext 的作用域  

 在大多数情况下，应在 <xref:System.Data.Objects.ObjectContext> 语句（在 Visual Basic 中为 `using`）内创建一个 `Using…End Using` 实例。 这样，通过确保当代码退出语句块时自动释放与对象上下文关联的资源，可以提高性能。 但是，当控制权绑定到由对象上下文管理的对象时，只要需要绑定并手动释放它，就应维护 <xref:System.Data.Objects.ObjectContext> 实例。 有关详细信息，请参阅 [管理连接和事务](/previous-versions/dotnet/netframework-4.0/bb896325(v=vs.100))。  
  
#### <a name="consider-opening-the-database-connection-manually"></a>考虑手动打开数据库连接  

 当应用程序执行一系列对象查询或频繁调用 <xref:System.Data.Objects.ObjectContext.SaveChanges%2A> 来持续对数据源执行创建、更新和删除操作时，实体框架必须连续打开并关闭与数据源的连接。 在这类情况下，请考虑在开始这些操作时手动打开连接，并在操作完成后关闭或释放连接。 有关详细信息，请参阅 [管理连接和事务](/previous-versions/dotnet/netframework-4.0/bb896325(v=vs.100))。  
  
## <a name="performance-data"></a>性能数据  

 [ADO.NET 团队博客](/archive/blogs/adonet/)上的以下文章中发布了实体框架的一些性能数据：  
  
- [Exploring the Performance of the ADO.NET Entity Framework - Part 1（探索 ADO.NET 实体框架的性能 - 第 1 部分）](/archive/blogs/adonet/exploring-the-performance-of-the-ado-net-entity-framework-part-1)  
  
- [Exploring the Performance of the ADO.NET Entity Framework – Part 2（探索 ADO.NET 实体框架的性能 - 第 2 部分）](/archive/blogs/adonet/exploring-the-performance-of-the-ado-net-entity-framework-part-2)  
  
- [ADO.NET Entity Framework Performance Comparison（ADO.NET 实体框架性能比较）](/archive/blogs/adonet/ado-net-entity-framework-performance-comparison)  
  
## <a name="see-also"></a>请参阅

- [开发和部署注意事项](development-and-deployment-considerations.md)
