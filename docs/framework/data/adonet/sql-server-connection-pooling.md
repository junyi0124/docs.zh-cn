---
title: SQL Server 连接池
description: 了解 ADO.NET 如何通过使用 SQL Server 连接池来最大程度地降低打开连接的成本，从而减少新连接的开销。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 7e51d44e-7c4e-4040-9332-f0190fe36f07
ms.openlocfilehash: 96d9e9a7e43f71dc30bd2d7a7f1902238941d471
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91156350"
---
# <a name="sql-server-connection-pooling-adonet"></a>SQL Server 连接池 (ADO.NET)

连接到数据库服务器通常由几个需要很长时间的步骤组成。 必须建立物理通道（例如套接字或命名管道），必须与服务器进行初次握手，必须分析连接字符串信息，必须由服务器对连接进行身份验证，必须运行检查以便在当前事务中登记，等等。  
  
 实际上，大多数应用程序仅使用一个或几个不同的连接配置。 这意味着在执行应用程序期间，许多相同的连接将反复地打开和关闭。 为了最大程度地降低打开连接的成本，ADO.NET 使用一种称为 *连接池*的优化技术。  
  
 连接池使新连接必须打开的次数得以减少。 *池池*维护物理连接的所有权。 通过为每个给定的连接配置保留一组活动连接来管理连接。 每当用户在连接上调用 `Open` 时，池进程就会查找池中可用的连接。 如果某个池连接可用，会将该连接返回给调用者，而不是打开新连接。 应用程序在该连接上调用 `Close` 时，池进程会将连接返回到活动连接池集中，而不是关闭连接。 连接返回到池中之后，即可在下一个 `Open` 调用中重复使用。  
  
 只有配置相同的连接可以建立池连接。 ADO.NET 会同时保留多个池，每种配置各有一个。 在使用集成的安全性时，连接按照连接字符串以及 Windows 标识分到多个池中。 还根据连接是否已在事务中登记来建立池连接。 在使用 <xref:System.Data.SqlClient.SqlConnection.ChangePassword%2A> 时，<xref:System.Data.SqlClient.SqlCredential> 实例影响连接池。 <xref:System.Data.SqlClient.SqlCredential> 的不同实例将使用不同的连接池，即使用户 ID 和密码相，也是如此。  
  
 池连接可以显著提高应用程序的性能和可缩放性。 默认情况下，在 ADO.NET 中启用连接池。 除非显式禁用，否则，在应用程序中打开和关闭连接时，池进程会对连接进行优化。 还可以提供几个连接字符串修饰符来控制连接池的行为。 有关更多信息，请参见本主题后面的“使用连接字符串关键字控制连接池”。  
  
> [!NOTE]
> 启用连接池后，如果发生超时错误或其他登录错误，则将引发异常，并且在接下来的五秒内进行的后续连接尝试将失败，此段时间称为“阻塞期”。 如果应用程序尝试在阻塞期内进行连接，则将再次引发第一个异常。 阻塞期结束后的后续失败将导致新的阻塞期，该阻塞期的持续时间是上一个阻塞期的两倍，最长为一分钟。  
  
## <a name="pool-creation-and-assignment"></a>池的创建和分配  

 在初次打开连接时，将根据完全匹配算法创建连接池，该算法将池与连接中的连接字符串关联。 每个连接池都与一个不同的连接字符串相关联。 打开新连接时，如果连接字符串并非与现有池完全匹配，将创建一个新池。 按进程、应用程序域、连接字符串以及 Windows 标识（在使用集成的安全性时）来建立池连接。 连接字符串还必须是完全匹配的；按不同顺序为同一连接提供的关键字将分到单独的池中。  
  
 在以下 C# 示例中创建了三个新的 <xref:System.Data.SqlClient.SqlConnection> 对象，但是管理时只需要两个连接池。 注意，根据为 `Initial Catalog` 分配的值，第一个和第二个连接字符串有所不同。  
  
```csharp
using (SqlConnection connection = new SqlConnection(  
  "Integrated Security=SSPI;Initial Catalog=Northwind"))  
    {  
        connection.Open();
        // Pool A is created.  
    }  
  
using (SqlConnection connection = new SqlConnection(  
  "Integrated Security=SSPI;Initial Catalog=pubs"))  
    {  
        connection.Open();
        // Pool B is created because the connection strings differ.  
    }  
  
using (SqlConnection connection = new SqlConnection(  
  "Integrated Security=SSPI;Initial Catalog=Northwind"))  
    {  
        connection.Open();
        // The connection string matches pool A.  
    }  
```  
  
 如果 `MinPoolSize` 在连接字符串中未指定或指定为零，池中的连接将在一段时间不活动后关闭。 但是，如果指定的 `MinPoolSize` 大于零，在 `AppDomain` 被卸载并且进程结束之前，连接池不会被破坏。 非活动或空池的维护只需要最少的系统开销。  
  
> [!NOTE]
> 当出现故障转移等错误时，会自动清除池。  
  
## <a name="adding-connections"></a>添加连接  

 连接池是为每个唯一的连接字符串创建的。 当创建一个池后，将创建多个连接对象并将其添加到该池中，以满足最小池大小的需求。 连接根据需要添加到池中，但是不能超过指定的最大池大小（默认值为 100）。 连接在关闭或断开时释放回池中。  
  
 在请求 <xref:System.Data.SqlClient.SqlConnection> 对象时，如果存在可用的连接，将从池中获取该对象。 连接要可用，必须未使用，具有匹配的事务上下文或未与任何事务上下文关联，并且具有与服务器的有效链接。  
  
 连接池进程通过在连接释放回池中时重新分配连接，来满足这些连接请求。 如果已达到最大池大小且不存在可用的连接，则该请求将会排队。 然后，池进程尝试重新建立任何连接，直至到达超时时间（默认值为 15 秒）。 如果池进程在连接超时之前无法满足请求，将引发异常。  
  
> [!CAUTION]
> 我们强烈建议您在使用完连接时一定要关闭连接，以便连接可以返回池。 您可以使用 `Close` 对象的或方法或 `Dispose` `Connection` 通过在 c # 中的语句内打开所有连接 `using` 或 `Using` Visual Basic 中的语句来执行此操作。 不是显式关闭的连接可能不会添加或返回到池中。 有关详细信息，请参阅 [Using 语句](../../../csharp/language-reference/keywords/using-statement.md) 或 [如何：处置 Visual Basic 的系统资源](../../../visual-basic/programming-guide/language-features/control-flow/how-to-dispose-of-a-system-resource.md) 。  
  
> [!NOTE]
> 不要在类的 `Close` 方法中对 `Dispose`、`Connection` 或任何其他托管对象调用 `DataReader` 或 `Finalize`。 在终结器中，仅释放类直接拥有的非托管资源。 如果类不拥有任何非托管资源，则不要在类定义中包含 `Finalize` 方法。 有关详细信息，请参阅 [垃圾回收](../../../standard/garbage-collection/index.md)。  
  
有关与打开和关闭连接相关联的事件的详细信息，请参阅 SQL Server 文档中的 [Audit Login 事件类](/sql/relational-databases/event-classes/audit-login-event-class) 和 [audit 注销事件类](/sql/relational-databases/event-classes/audit-logout-event-class) 。  
  
## <a name="removing-connections"></a>移除连接  

 如果空闲时间达到大约 4-8 分钟，或池进程检测到与服务器的连接已断开，连接池进程会将该连接从池中移除。 注意，只有在尝试与服务器进行通信之后才能检测到断开的连接。 如果发现某连接不再连接到服务器，则会将其标记为无效。 无效连接只有在关闭或重新建立后，才会从连接池中移除。  
  
 如果存在一个与已消失的服务器的连接，即使连接池进程尚未检测到断开的连接，也可以从池中取出此连接并将连接标记为无效。 这种情况是因为检查连接是否仍有效的系统开销将造成与服务器的另一次往返，从而抵消了池进程的优势。 发生此情况时，初次尝试使用该连接将检测连接是否曾断开，并引发异常。  
  
## <a name="clearing-the-pool"></a>清除池  

 ADO.NET 2.0 引入了两种新的方法来清除池： <xref:System.Data.SqlClient.SqlConnection.ClearAllPools%2A> 和 <xref:System.Data.SqlClient.SqlConnection.ClearPool%2A> 。 `ClearAllPools` 清除指定提供程序的连接池，`ClearPool` 清除与特定连接关联的连接池。 如果在调用时连接正在使用，将对它们进行相应的标记。 连接关闭时，将被丢弃，而不是返回池中。  
  
## <a name="transaction-support"></a>事务支持  

 连接是根据事务上下文来从池中取出并进行分配的。 除非在连接字符串中指定了 `Enlist=false`，否则连接池将确保连接在 <xref:System.Transactions.Transaction.Current%2A> 上下文中登记。 如果连接使用登记的 `System.Transactions` 事务关闭并返回到池中，连接将保留在池中，以便使用相同 `System.Transactions` 事务对该连接池的下一次请求将返回相同的连接（如果可用）。 如果发出这样的请求，而没有可用的池连接，则会从池的非事务性部分取出一个连接并登记。 如果在池的每个区域都没有可用的连接，则会创建一个新的连接并登记。  
  
 当连接关闭时，它将被释放回池中，并根据其事务上下文放入相应的子部分。 因此，即使分布式事务仍然挂起，仍可以关闭该连接而不会生成错误。 这样，你就可以在之后提交或中止分布式事务。  
  
## <a name="controlling-connection-pooling-with-connection-string-keywords"></a>使用连接字符串关键字控制连接池  

 `ConnectionString` 对象的 <xref:System.Data.SqlClient.SqlConnection> 属性支持连接字符串键/值对，可以用于调整连接池逻辑的行为。 有关详细信息，请参阅 <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A>。  
  
## <a name="pool-fragmentation"></a>池碎片  

 池碎片是许多 Web 应用程序中的一个常见问题，应用程序可能会创建大量在进程退出后才会释放的池。 这样，将打开大量的连接，占用许多内存，从而导致性能降低。  
  
### <a name="pool-fragmentation-due-to-integrated-security"></a>因为集成安全性产生的池碎片  

 连接根据连接字符串以及用户标识来建立池连接。 因此，如果使用网站上的基本身份验证或 Windows 身份验证以及集成的安全登录，每个用户将获得一个池。 尽管这样可以提高单个用户的后续数据库请求的性能，但是该用户无法利用其他用户建立的连接。 这样还使每个用户至少产生一个与数据库服务器的连接。 这对特定 Web 应用程序结构会产生副作用，因为开发人员必须衡量安全性和审计要求。  
  
### <a name="pool-fragmentation-due-to-many-databases"></a>因为许多数据库产生的池碎片  

 许多 Internet 服务提供商在一台服务器上托管多个网站。 他们可能使用单个数据库确认窗体身份验证登录，然后为该用户或用户组打开与特定数据库的连接。 与身份验证数据库的连接将建立池连接，供每个用户使用。 但是，每个数据库的连接存在一个独立的池，这会增加与服务器的连接数。  
  
 这也会对应用程序设计产生副作用。 但是，可以通过一个相对简单的方式避免此副作用，而又不会影响连接 SQL Server 时的安全性。 不是为每个用户或组连接独立的数据库，而是连接到服务器上的相同数据库，然后执行 Transact-SQL USE 语句来切换为所需的数据库。 以下代码段演示如何创建与 `master` 数据库的初始连接，然后切换到 `databaseName` 字符串变量中指定的所需数据库。  
  
```vb  
' Assumes that command is a valid SqlCommand object and that  
' connectionString connects to master.  
    command.Text = "USE DatabaseName"  
Using connection As New SqlConnection(connectionString)  
    connection.Open()  
    command.ExecuteNonQuery()  
End Using  
```  
  
```csharp  
// Assumes that command is a SqlCommand object and that  
// connectionString connects to master.  
command.Text = "USE DatabaseName";  
using (SqlConnection connection = new SqlConnection(  
  connectionString))  
  {  
    connection.Open();  
    command.ExecuteNonQuery();  
  }  
```  
  
## <a name="application-roles-and-connection-pooling"></a>应用程序角色和连接池  

 通过调用 `sp_setapprole` 系统存储过程激活了 SQL Server 应用程序角色之后，该连接的安全上下文无法重置。 但是，如果启用了池，连接将返回池，在重复使用池连接时会出错。 有关详细信息，请参阅知识库文章 "[OLE DB 资源池的 SQL 应用程序角色错误](https://support.microsoft.com/default.aspx?scid=KB;EN-US;Q229564)"。  
  
### <a name="application-role-alternatives"></a>应用程序角色替代项  

 建议您利用可以使用的安全机制，而不使用应用程序角色。 有关详细信息，请参阅 [在 SQL Server 中创建应用程序角色](./sql/creating-application-roles-in-sql-server.md)。  
  
## <a name="see-also"></a>请参阅

- [连接池](connection-pooling.md)
- [SQL Server 和 ADO.NET](./sql/index.md)
- [性能计数器](performance-counters.md)
- [ADO.NET 概述](ado-net-overview.md)
