---
title: 安全注意事项（实体框架）
ms.date: 03/30/2017
ms.assetid: 84758642-9b72-4447-86f9-f831fef46962
ms.openlocfilehash: 5a2d96679c43528b86d3b1d7301b4520a38289aa
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91156675"
---
# <a name="security-considerations-entity-framework"></a>安全注意事项（实体框架）

本主题介绍特定于开发、部署和运行实体框架应用程序的安全注意事项。 还应遵循创建安全 .NET Framework 应用程序的建议。 有关详细信息，请参阅[安全性概述](../security-overview.md)。  
  
## <a name="general-security-considerations"></a>一般安全注意事项  

 以下安全注意事项适用于所有使用实体框架的应用程序。  
  
#### <a name="use-only-trusted-data-source-providers"></a>仅使用可信的数据源提供程序。  

 若要与数据源通信，提供程序必须执行下列操作：  
  
- 接收来自实体框架的连接字符串。  
  
- 将命令目录树转换为数据源的本机查询语言。  
  
- 组合并返回结果集。  
  
 在登录操作过程中，将通过基础数据源的网络库将基于用户密码的信息传递给服务器。 恶意提供程序可能窃取用户凭据，生成恶意查询或篡改结果集。  
  
#### <a name="encrypt-your-connection-to-protect-sensitive-data"></a>对连接进行加密以保护敏感数据。  

 实体框架不会直接处理数据加密。 如果用户通过公共网络访问数据，应用程序应建立到数据源的加密连接以提高安全性。 有关详细信息，请参见关于您所用数据源的安全性方面的文档。 有关 SQL Server 数据源，请参阅 [加密到 SQL Server 的连接](/previous-versions/sql/sql-server-2008-r2/ms189067(v=sql.105))。  
  
#### <a name="secure-the-connection-string"></a>保护连接字符串。  

 保护应用程序时，最重要的目标之一是保护对数据源的访问。 不受保护或构造不当的连接字符串会构成潜在的安全漏洞。 如果以纯文本形式存储连接信息或者将其保留在内存中，则可能危及整个系统的安全。 建议采用以下方法保护连接字符串：  
  
- 对 SQL Server 数据源使用 Windows 身份验证。  
  
     如果使用 Windows 身份验证连接到 SQL Server 数据源，则连接字符串不包含登录和密码信息。  
  
- 使用受保护的配置加密配置文件节。  
  
     ASP.NET 提供了一项称为“受保护配置”的功能，您可以使用此功能对配置文件中的敏感信息进行加密。 虽然受保护配置主要是为 ASP.NET 设计的，但您也可以使用该功能对 Windows 应用程序中的配置文件节进行加密。 有关新的受保护配置功能的详细说明，请参阅 [使用受保护的配置加密配置信息](/previous-versions/aspnet/53tyfkaw(v=vs.100))。  
  
- 将连接字符串存储在受保护的配置文件中。  
  
     绝不应在源代码中嵌入连接字符串。 你可以在配置文件中存储连接字符串，从而不必将其嵌入到应用程序的代码中。 默认情况下，实体数据模型向导将连接字符串存储在应用程序配置文件中。 必须保护此文件，避免未经授权的访问。  
  
- 动态创建连接时使用连接字符串生成器。  
  
     如果必须在运行时构造连接字符串，请使用 <xref:System.Data.EntityClient.EntityConnectionStringBuilder> 类。 此字符串生成器类可以验证并转义无效的输入信息，从而有助于防止连接字符串注入式攻击。 有关详细信息，请参阅 [如何：生成 EntityConnection 连接字符串](how-to-build-an-entityconnection-connection-string.md)。 还应使用适当的字符串生成器类构造作为实体框架连接字符串一部分的数据源连接字符串。 有关 ADO.NET 提供程序的连接字符串生成器的信息，请参阅 [连接字符串生成器](../connection-string-builders.md)。  
  
 有关详细信息，请参阅 [保护连接信息](../protecting-connection-information.md)。  
  
#### <a name="do-not-expose-an-entityconnection-to-untrusted-users"></a>不要向不可信用户公开 EntityConnection。  

 <xref:System.Data.EntityClient.EntityConnection> 对象公开基础连接的连接字符串。 有权访问 <xref:System.Data.EntityClient.EntityConnection> 对象的用户也可以更改基础连接的 <xref:System.Data.ConnectionState>。 <xref:System.Data.EntityClient.EntityConnection> 类不是线程安全的。  
  
#### <a name="do-not-pass-connections-outside-the-security-context"></a>不要在安全上下文以外传递连接。  

 连接建立后，切勿在安全上下文以外传递它。 例如，有权打开连接的线程不应将连接存储在全局位置。 如果连接位于全局位置，则其他恶意线程无需显式授权便可使用该打开的连接。  
  
#### <a name="be-aware-that-logon-information-and-passwords-may-be-visible-in-a-memory-dump"></a>请注意，登录信息和密码可能在内存转储中是可见的。  

 如果在连接字符串中提供数据源登录和密码信息，那么在垃圾回收机制回收这些资源之前，这些信息将一直保留在内存中。 这样，就无法确定密码字符串何时从内存中消失。 如果应用程序崩溃，内存转储文件中有可能包含敏感的安全信息，而运行该应用程序的用户以及对计算机拥有管理员权限的任何用户都可以查看内存转储文件。 请为到 Microsoft SQL Server 的连接使用 Windows 身份验证。  
  
#### <a name="grant-users-only-the-necessary-permissions-in-the-data-source"></a>仅授予用户数据源的必要权限。  

 数据源管理员应仅向用户授予必要的权限。 尽管 [!INCLUDE[esql](../../../../../includes/esql-md.md)] 不支持修改数据的 DML 语句（如 INSERT、UPDATE 或 DELETE），但用户仍可以访问到数据源的连接。 恶意用户可使用此连接通过数据源的本机语言执行 DML 语句。  
  
#### <a name="run-applications-with-the-minimum-permissions"></a>以最低权限运行应用程序。  

 如果允许托管应用程序以完全信任权限运行，则 .NET Framework 不会限制应用程序访问您的计算机。 这会在应用程序中造成安全漏洞而威胁整个系统。 若要在 .NET Framework 中使用代码访问安全性和其他安全机制，你应使用部分信任权限以及允许应用程序正常运行所需的最小权限集来运行应用程序。 以下代码访问权限是实体框架应用程序所需的最低权限：  
  
- <xref:System.Security.Permissions.FileIOPermission>：使用 <xref:System.Security.Permissions.FileIOPermissionAccess.Write> 打开指定的元数据文件，或者使用 <xref:System.Security.Permissions.FileIOPermissionAccess.PathDiscovery> 在目录中搜索元数据文件。  
  
- <xref:System.Security.Permissions.ReflectionPermission>：使用 <xref:System.Security.Permissions.ReflectionPermissionFlag.RestrictedMemberAccess> 支持 LINQ to Entities 查询。  
  
- <xref:System.Transactions.DistributedTransactionPermission>：使用 <xref:System.Security.Permissions.PermissionState.Unrestricted> 在 <xref:System.Transactions><xref:System.Transactions.Transaction> 中登记。  
  
- <xref:System.Security.Permissions.SecurityPermission>：使用 <xref:System.Security.Permissions.SecurityPermissionFlag.SerializationFormatter> 以通过 <xref:System.Runtime.Serialization.ISerializable> 接口对异常进行序列化。  
  
- 打开数据库连接和对数据库执行命令的权限，例如， <xref:System.Data.SqlClient.SqlClientPermission> 对于 SQL Server 数据库。  
  
 有关更多信息，请参见 [Code Access Security and ADO.NET](../code-access-security.md)。  
  
#### <a name="do-not-install-untrusted-applications"></a>不要安装不可信的应用程序。  

 实体框架不会强制实施任何安全权限，并将在进程中调用任何用户提供的数据对象代码，而不管它是否受信任。 确保数据存储和应用程序执行客户端身份验证和授权。  
  
#### <a name="restrict-access-to-all-configuration-files"></a>限制对所有配置文件的访问。  

 管理员必须限制对指定应用程序配置的所有文件的写访问权限，包括 enterprisesec.config、security.config、machine.config 和应用程序配置文件 \<*application*>.exe.config。  
  
 可在 app.config 中修改提供程序固定名称。客户端应用程序必须负责使用强名称通过标准提供程序工厂模型访问基础提供程序。  
  
#### <a name="restrict-permissions-to-the-model-and-mapping-files"></a>限制对模型和映射文件的访问权限。  

 管理员必须将对模型和映射文件（.edmx、.csdl、.ssdl 和 .msl）的写访问权限仅分配给负责修改模型或映射的用户。 实体框架只需要在运行时对这些文件具有读取权限。 管理员还应限制对由实体数据模型工具生成的对象层和预编译的查看源代码文件的访问权限。  
  
## <a name="security-considerations-for-queries"></a>有关查询的安全注意事项  

 查询概念模型时需要考虑下列安全注意事项。 这些注意事项适用于使用 EntityClient 的 [!INCLUDE[esql](../../../../../includes/esql-md.md)] 查询以及使用 LINQ、[!INCLUDE[esql](../../../../../includes/esql-md.md)] 和查询生成器方法的对象查询。  
  
#### <a name="prevent-sql-injection-attacks"></a>防范 SQL 注入式攻击。  

 应用程序经常接受外部输入（来自用户或其他外部代理），并根据该输入执行操作。 任何直接或间接从用户或外部代理派生的输入都可能包含使用目标语言的语法来执行未授权操作的内容。 当目标语言是结构化查询语言 (SQL) ，例如 Transact-sql 时，此操作称为 SQL 注入攻击。 恶意用户可直接向查询中注入命令并删除数据库表、引起拒绝服务或者更改所执行操作的性质。  
  
- [!INCLUDE[esql](../../../../../includes/esql-md.md)] 注入式攻击：  
  
     SQL 注入式攻击在 [!INCLUDE[esql](../../../../../includes/esql-md.md)] 中的实施方法是向查询谓词和参数名称中使用的值提供恶意输入。 若要避免 SQL 注入风险，切勿组合用户输入与 [!INCLUDE[esql](../../../../../includes/esql-md.md)] 命令文本。  
  
     [!INCLUDE[esql](../../../../../includes/esql-md.md)] 查询可在任何接受文本的位置接受参数。 应使用参数化查询，而不是将来自外部代理的文本直接注入查询。 还应考虑使用 [查询生成器方法](/previous-versions/dotnet/netframework-4.0/bb896238(v=vs.100)) 安全地构造实体 SQL。  
  
- LINQ to Entities 注入攻击：  
  
     尽管可以在 LINQ to Entities 中进行查询撰写，但它是通过对象模型 API 执行的。 与 [!INCLUDE[esql](../../../../../includes/esql-md.md)] 查询不同，LINQ to Entities 查询不是通过使用字符串操作或串联来撰写的，它们不容易受到传统的 SQL 注入式攻击。  
  
#### <a name="prevent-very-large-result-sets"></a>避免结果集过大。  

 如果客户端执行的操作所消耗的资源与结果集的大小成正比，则过大的结果集会导致客户端系统关闭。 在下列情况下可能发生意外的过大结果集：  
  
- 针对大型数据库的查询未包含适当的筛选条件。  
  
- 查询在服务器上创建笛卡尔联接。  
  
- 嵌套的 [!INCLUDE[esql](../../../../../includes/esql-md.md)] 查询。  
  
 如果接受用户输入，则必须确保输入内容不会导致结果集过大以致超出系统的处理能力。 你还可以使用 LINQ to Entities 中的 <xref:System.Linq.Queryable.Take%2A> 方法或中的 [LIMIT](./language-reference/limit-entity-sql.md) 运算符 [!INCLUDE[esql](../../../../../includes/esql-md.md)] 来限制结果集的大小。  
  
#### <a name="avoid-returning-iqueryable-results-when-exposing-methods-to-potentially-untrusted-callers"></a>避免在将方法公开给可能不受信任的调用方时返回 IQueryable 结果。  

 避免公开给可能不受信任的调用方的方法返回 <xref:System.Linq.IQueryable%601> 类型的原因如下：  
  
- 公开 <xref:System.Linq.IQueryable%601> 类型的查询的使用方可以对公开安全数据或增大结果集大小的结果调用方法。 例如，请考虑使用以下方法签名：  

    ```csharp
    public IQueryable<Customer> GetCustomer(int customerId)
    ```

    此查询的使用方可以对返回的 `.Include("Orders")` 调用 `IQueryable<Customer>`，以检索查询不打算公开的数据。 通过将方法的返回类型更改为 <xref:System.Collections.Generic.IEnumerable%601> 并调用具体化结果的方法（如 `.ToList()`），可以避免此问题。  
  
- 由于 <xref:System.Linq.IQueryable%601> 查询在循环访问结果时执行，因此公开 <xref:System.Linq.IQueryable%601> 类型的查询的使用方可能会捕捉引发的异常。 这些异常可能包含不适用于使用方的信息。  
  
## <a name="security-considerations-for-entities"></a>有关实体的安全注意事项  

 生成和处理实体类型时应考虑下列安全注意事项。  
  
#### <a name="do-not-share-an-objectcontext-across-application-domains"></a>不要在应用程序域之间共享 ObjectContext。  

 在多个应用程序域之间共享 <xref:System.Data.Objects.ObjectContext> 可能会公开连接字符串中的信息。 正确的做法是将序列化的对象或对象图传递给其他应用程序域，并将这些对象附加到该应用程序域中的 <xref:System.Data.Objects.ObjectContext>。 有关详细信息，请参阅 [序列化对象](/previous-versions/dotnet/netframework-4.0/bb738446(v=vs.100))。  
  
#### <a name="prevent-type-safety-violations"></a>避免类型安全冲突。  

 如果违反类型安全，则实体框架无法保证对象中数据的完整性。 如果使用完全信任代码访问安全性运行不可信的应用程序，可能导致类型安全冲突。  
  
#### <a name="handle-exceptions"></a>处理异常。  

 在 try-catch 块中访问 <xref:System.Data.Objects.ObjectContext> 的方法和属性。 捕获异常可防止未经处理的异常向应用程序的用户公开 <xref:System.Data.Objects.ObjectStateManager> 中的条目或模型信息（如，表名称）。  
  
## <a name="security-considerations-for-aspnet-applications"></a>有关 ASP.NET 应用程序的安全注意事项  

在 ASP.NET 应用程序中使用路径时，应考虑以下事项。  
  
#### <a name="verify-whether-your-host-performs-path-checks"></a>检验主机是否执行路径检查。  

 当 `|DataDirectory|` 使用 (包含在管道符号) 替换字符串中时，ADO.NET 将验证是否支持解析的路径。 例如，`DataDirectory` 后面不允许出现“..”。 解决 Web 应用程序根运算符 (的相同检查 `~`) 由托管 ASP.NET 的进程执行。 IIS 执行此检查；但是非 IIS 的主机可能不检验是否支持解析的路径。 你应该知道要在其上部署实体框架应用程序的主机的行为。  
  
#### <a name="do-not-make-assumptions-about-resolved-path-names"></a>不要对解析的路径名称做任何假定。  

 尽管根运算符 () 的值 `~` 和 `DataDirectory` 替换字符串解析应在应用程序运行时保持不变，但实体框架不会限制主机修改这些值。  
  
#### <a name="verify-the-path-length-before-deployment"></a>部署前检查路径长度。  

 在部署实体框架应用程序之前，应确保根运算符 (~) 和 `DataDirectory` 替换字符串的值不超过操作系统中路径长度的限制。 ADO.NET 数据访问接口不确保路径长度超出有效限制。  
  
## <a name="security-considerations-for-adonet-metadata"></a>有关 ADO.NET 元数据的安全注意事项  

 生成和处理模型和映射文件时应考虑下列安全注意事项。  
  
#### <a name="do-not-expose-sensitive-information-through-logging"></a>不要通过日志公开敏感信息。  

ADO.NET 元数据服务组件不记录任何专用信息。 如果由于访问限制而无法返回某些结果，则数据库管理系统和文件系统应返回零结果，而不是引发可能包含敏感信息的异常。  
  
#### <a name="do-not-accept-metadataworkspace-objects-from-untrusted-sources"></a>不要接受来自不可信源的 MetadataWorkspace 对象。  

 应用程序不应接受来自不可信源的 <xref:System.Data.Metadata.Edm.MetadataWorkspace> 类的实例。 正确的做法是从这样的源显式构造并填充工作区。  
  
## <a name="see-also"></a>请参阅

- [保证 ADO.NET 应用程序的安全](../securing-ado-net-applications.md)
- [部署注意事项](deployment-considerations.md)
- [迁移注意事项](migration-considerations.md)
