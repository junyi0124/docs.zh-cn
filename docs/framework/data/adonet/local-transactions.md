---
title: 本地事务
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 8ae3712f-ef5e-41a1-9ea9-b3d0399439f1
ms.openlocfilehash: a0b713ab0b81cb2f0661212dae22db34b7f9f3ae
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91175390"
---
# <a name="local-transactions"></a>本地事务

如果要将多个任务绑定在一起，以便将它们作为单个工作单元执行，则可以使用 ADO.NET 中的事务。 例如，假设应用程序执行两个任务。 首先使用订单信息更新表。 然后更新包含库存信息的表，将已订购的商品记入借方。 如果有一个任务失败，则回滚两个更新。  
  
## <a name="determining-the-transaction-type"></a>确定事务类型  

 如果事务是单阶段事务，则该事务被视为本地事务，并且由数据库直接处理。 当事务由事务监视器协调并使用防故障机制 (例如两阶段提交) 进行事务解析时，该事务被视为分布式事务。  
  
 每个 .NET Framework 数据提供程序都有自己 `Transaction` 的用于执行本地事务的对象。 如果要求事务在 SQL Server 数据库中执行，则选择 <xref:System.Data.SqlClient> 事务。 对于 Oracle 事务，使用 <xref:System.Data.OracleClient> 提供程序。 此外，还提供了一个 <xref:System.Data.Common.DbTransaction> 类，该类可用于编写与提供程序无关的代码，该代码需要事务。  
  
> [!NOTE]
> 当事务在服务器上执行时，它们是最有效的。 如果使用的 SQL Server 数据库广泛使用显式事务，应考虑使用 Transact-SQL BEGIN TRANSACTION 语句以存储过程的形式编写这些事务。
  
## <a name="performing-a-transaction-using-a-single-connection"></a>使用单个连接执行事务  

 在 ADO.NET 中，可以通过对象控制事务 `Connection` 。 可以使用 `BeginTransaction` 方法启动本地事务。 开始事务后，可以使用 `Transaction` 对象的 `Command` 属性在该事务中登记一个命令。 然后，可以根据事务组件的成功或失败，提交或回滚在数据源上进行的修改。  
  
> [!NOTE]
> 不应对本地事务使用 `EnlistDistributedTransaction` 方法。  
  
 事务的作用域限于该连接。 以下示例执行显式事务，该事务由 `try` 块中两个独立的命令组成。 命令对 AdventureWorks SQL Server 示例数据库中的 ScrapReason 表执行 INSERT 语句，如果没有引发异常，则提交该数据库。 如果引发异常，`catch` 块中的代码将回滚此事务。 如果在事务完成之前事务中止或连接关闭，事务将自动回滚。  
  
## <a name="example"></a>示例  

 按照下列步骤执行事务。  
  
1. 调用 <xref:System.Data.SqlClient.SqlConnection.BeginTransaction%2A> 对象的 <xref:System.Data.SqlClient.SqlConnection> 方法，以标记事务的开始。 <xref:System.Data.SqlClient.SqlConnection.BeginTransaction%2A> 方法返回对事务的引用。 此引用分配给在事务中登记的 <xref:System.Data.SqlClient.SqlCommand> 对象。  
  
2. 将 `Transaction` 对象分配给要执行的 <xref:System.Data.SqlClient.SqlCommand.Transaction%2A> 的 <xref:System.Data.SqlClient.SqlCommand> 属性。 如果在具有活动事务的连接上执行命令，并且尚未将 `Transaction` 对象配给 `Transaction` 对象的 `Command` 属性，则会引发异常。  
  
3. 执行所需的命令。  
  
4. 调用 <xref:System.Data.SqlClient.SqlTransaction.Commit%2A> 对象的 <xref:System.Data.SqlClient.SqlTransaction> 方法完成事务，或调用 <xref:System.Data.SqlClient.SqlTransaction.Rollback%2A> 方法结束事务。 如果在 <xref:System.Data.SqlClient.SqlTransaction.Commit%2A> 或 <xref:System.Data.SqlClient.SqlTransaction.Rollback%2A> 方法执行之前连接关闭或断开，事务将回滚。  
  
 下面的代码示例演示了使用 ADO.NET 与 Microsoft SQL Server 的事务逻辑。  
  
 [!code-csharp[DataWorks SqlTransaction.Local#1](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SqlTransaction.Local/CS/source.cs#1)]
 [!code-vb[DataWorks SqlTransaction.Local#1](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SqlTransaction.Local/VB/source.vb#1)]  
  
## <a name="see-also"></a>请参阅

- [事务和并发性](transactions-and-concurrency.md)
- [分布式事务](distributed-transactions.md)
- [System.Transactions 与 SQL Server 的集成](system-transactions-integration-with-sql-server.md)
- [ADO.NET 概述](ado-net-overview.md)
