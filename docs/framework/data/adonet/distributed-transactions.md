---
title: 分布式事务
ms.date: 03/30/2017
ms.assetid: 718b257c-bcb2-408e-b004-a7b0adb1c176
ms.openlocfilehash: b6553d50039ca0f4888f0610b187c6b419a462b5
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91153191"
---
# <a name="distributed-transactions"></a>分布式事务

事务是一组相关的任务，作为独立于其他任务的独立单元成功（提交）或失败（中止）。 *分布式事务*是影响若干资源的事务。 要提交分布式事务，所有参与者都必须保证对数据的任何更改是永久的。 即使发生系统崩溃或其他不可预见的事件，更改也必须是永久的。 即使只有一个参与者无法保证这一点，整个事务也将失败，在事务范围内对数据的任何更改均将回滚。  
  
> [!NOTE]
> 如果 `DataReader` 在事务处于活动状态时启动，此时若尝试提交或回滚事务，将会引发异常。  
  
## <a name="working-with-systemtransactions"></a>使用 System.Transactions  

 在 .NET Framework 中，分布式事务通过 <xref:System.Transactions> 命名空间中的 API 进行管理。 如果涉及多个永久资源管理器，<xref:System.Transactions> API 会将分布式事务处理委托给事务监视器，例如 Microsoft 分布式事务协调程序 (MS DTC)。 有关详细信息，请参阅 [事务基础知识](../transactions/transaction-fundamentals.md)。  
  
 ADO.NET 2.0 引入了对使用 `EnlistTransaction` 方法在分布式事务中进行登记的支持，该方法会登记 <xref:System.Transactions.Transaction> 实例中的连接。 在以前版本的 ADO.NET 中，分布式事务中的显式登记使用连接的 `EnlistDistributedTransaction` 方法执行，以登记 <xref:System.EnterpriseServices.ITransaction> 实例中的连接，为了向后兼容，也支持该方法。 有关企业服务事务的详细信息，请参阅 [与企业服务和 COM + 事务的互操作性](../transactions/interoperability-with-enterprise-services-and-com-transactions.md)。  
  
 在针对 SQL Server 数据库将 <xref:System.Transactions> 事务与用于 SQL Server 的 .NET Framework 提供程序结合使用时，将自动创建一个轻型 <xref:System.Transactions.Transaction>。 该事务可以根据需要提升为完全分布式事务。 有关详细信息，请参阅 [System. 事务与 SQL Server 的集成](system-transactions-integration-with-sql-server.md)。  
  
> [!NOTE]
> 默认情况下，Oracle 数据库可以同时参与的分布式事务的最大数目设置为 10。 第 10 个事务之后，连接到 Oracle 数据库时将会引发异常。 Oracle 不支持分布式事务内的 `DDL`。  
  
## <a name="automatically-enlisting-in-a-distributed-transaction"></a>自动在分布式事务中登记  

 自动登记是将 ADO.NET 连接与 `System.Transactions` 集成的默认（和首选）方法。 如果连接对象确定事务处于活动状态，用 `System.Transaction` 术语来说是指 `Transaction.Current` 不为 Null，则连接对象会自动在现有分布式事务中登记。 自动事务登记在连接打开时进行。 之后，即使在事务范围内执行命令，也不会进行自动事务登记。 可以在现有事务中禁用自动登记，方法是将 `Enlist=false` 指定为 <xref:System.Data.SqlClient.SqlConnection.ConnectionString%2A?displayProperty=nameWithType> 的连接字符串参数，或将 `OLE DB Services=-7` 指定为 <xref:System.Data.OleDb.OleDbConnection.ConnectionString%2A?displayProperty=nameWithType> 的连接字符串参数。 有关 Oracle 和 ODBC 连接字符串参数的更多信息，请参见 <xref:System.Data.OracleClient.OracleConnection.ConnectionString%2A?displayProperty=nameWithType> 和 <xref:System.Data.Odbc.OdbcConnection.ConnectionString%2A?displayProperty=nameWithType>。  
  
## <a name="manually-enlisting-in-a-distributed-transaction"></a>手动在分布式事务中登记  

 如果禁用了自动登记或者您需要登记在连接打开后启动的事务，则可以使用所用提供程序的 `EnlistTransaction` 对象的 <xref:System.Data.Common.DbConnection> 方法，在现有分布式事务中登记。 在现有分布式事务中登记可以确保当提交或回滚了事务时，也提交或回滚对数据源所做作的代码修改。  
  
 在分布式事务中登记尤其适用于为业务对象建立池连接。 如果业务对象使用打开的连接建立池连接，自动事务登记只有在该连接打开时才会进行。 如果使用池中的业务对象执行多个事务，则该对象的打开连接不自动登记在新启动的事务中。 在这种情况下，可以对该连接禁用自动事务登记，并使用 `EnlistTransaction` 在事务中登记连接。  
  
 `EnlistTransaction` 采用类型的单个自变量 <xref:System.Transactions.Transaction> ，该参数是对现有事务的引用。 在调用连接的 `EnlistTransaction` 方法之后，所有使用该连接在数据源上进行的修改均将加入事务中。 传递空值将取消该连接在当前分布式事务登记中的登记。 注意，在调用 `EnlistTransaction` 之前连接必须打开。  
  
> [!NOTE]
> 在某个事务中显式登记了连接之后，在该事务完成之前，连接将无法取消登记或在另一个事务中登记。  
  
> [!CAUTION]
> 如果连接已使用连接的 `EnlistTransaction` 方法开始了某个事务，<xref:System.Data.Common.DbConnection.BeginTransaction%2A> 将引发异常。 但是，如果事务是在数据源上开始的本地事务（例如使用 <xref:System.Data.SqlClient.SqlCommand> 显式执行 BEGIN TRANSACTION 语句），`EnlistTransaction` 将回滚该本地事务并根据请求在现有分布式事务中登记。 你不会接收本地事务已回滚的通知，必须管理任何未使用 <xref:System.Data.Common.DbConnection.BeginTransaction%2A> 开始的本地事务。 如果您将用于 SQL Server 的 .NET Framework 数据提供程序 (`SqlClient`) 与 SQL Server 结合使用，则尝试登记会引发异常。 所有其他情况将无法发现。  
  
## <a name="promotable-transactions-in-sql-server"></a>SQL Server 中的可提升事务  

 SQL Server 支持可提升的事务，在此类事务中，本地轻型事务可以仅在需要时自动提升为分布式事务。 可提升的事务不会调用分布式事务增加的系统开销，除非需要增加的系统开销。 有关详细信息和代码示例，请参阅 [System. 事务与 SQL Server 的集成](system-transactions-integration-with-sql-server.md)。  
  
## <a name="configuring-distributed-transactions"></a>配置分布式事务  

 为了使用分布式事务，您可能需要在网络上启用 MS DTC。 如果已启用 Windows 防火墙，则必须允许 MS DTC 服务使用网络或打开 MS DTC 端口。  
  
## <a name="see-also"></a>请参阅

- [事务和并发性](transactions-and-concurrency.md)
- [System.Transactions 与 SQL Server 的集成](system-transactions-integration-with-sql-server.md)
- [ADO.NET 概述](ado-net-overview.md)
