---
title: SQL Server 中的快照隔离
description: 阅读 SQL Server 中快照隔离和行版本控制的概述，了解如何管理隔离级别的并发。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 43ae5dd3-50f5-43a8-8d01-e37a61664176
ms.openlocfilehash: 4934c031eb9dfb26d60c5233937cbc65ca60d4f7
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91183073"
---
# <a name="snapshot-isolation-in-sql-server"></a>SQL Server 中的快照隔离

快照隔离增强了 OLTP 应用程序的并发性。  
  
## <a name="understanding-snapshot-isolation-and-row-versioning"></a>了解快照隔离和行版本控制  

 启用快照隔离后，必须维护每个事务的更新行版本。  在 SQL Server 2019 之前，这些版本存储在 **tempdb**中。 SQL Server 2019 引入了一项新功能，即加速数据库恢复 (ADR) ，这需要其自己的行版本集。  因此，在 SQL Server 2019 的情况下，如果未启用 ADR，则行版本将始终保留在 **tempdb** 中。  如果启用了 ADR，则与快照隔离和 ADR 相关的所有行版本都将保留在 ADR 的持久性版本存储 (PVS) 中，该存储位于用户指定的文件组的用户数据库中。 唯一的事务序列号标识每个事务，并为每个行版本记录这些唯一的编号。 事务处理的最新行版本在事务序列号之前有一个序列号。 事务将忽略在事务开始后创建的新行版本。  
  
 术语“快照”反映的事实是，事务中的所有查询都能看到数据库的相同版本或快照，这取决于事务开始时数据库的状态。 不会在快照事务中的基础数据行或数据页上获取锁，这样可以执行其他事务，而不会被以前未完成的事务所阻止。 修改数据的事务不会阻止读取数据的事务，并且读取数据的事务不会阻止写入数据的事务，因为在 SQL Server 中，它们通常会在默认的 READ COMMITTED 隔离级别下执行这些操作。 这种不会产生阻止的行为也大大降低了复杂事务出现死锁的可能性。  
  
 快照隔离使用开放式并发模型。 如果快照事务尝试提交对事务开始后已更改的数据所做的修改，事务将回滚，并引发错误。 可以通过对访问要修改的数据的 SELECT 语句使用 UPDLOCK 提示来避免这种情况。 有关详细信息，请参阅 SQL Server 联机丛书中的“锁定提示”。  
  
 在事务中使用快照隔离之前，必须通过在数据库上设置 ALLOW_SNAPSHOT_ISOLATION 选项来启用它。 这样将激活在临时数据库 (tempdb) 中存储行版本的机制****。 必须在将快照隔离与 Transact-SQL ALTER DATABASE 语句一起使用的每个数据库中启用快照隔离。 在这方面，快照隔离与传统的隔离级别（READ COMMITTED、REPEATABLE READ、SERIALIZABLE 和 READ UNCOMMITTED）不同，这些级别不需要配置。 以下语句激活快照隔离并将默认的 READ COMMITTED 行为替换为快照：  
  
```sql  
ALTER DATABASE MyDatabase  
SET ALLOW_SNAPSHOT_ISOLATION ON  
  
ALTER DATABASE MyDatabase  
SET READ_COMMITTED_SNAPSHOT ON  
```  
  
 设置 READ_COMMITTED_SNAPSHOT ON 选项后，可以在默认的 READ COMMITTED 隔离级别下访问版本控制行。 如果 READ_COMMITTED_SNAPSHOT 选项设置为 OFF，则必须为每个会话显式设置快照隔离级别，以便访问版本控制行。  
  
## <a name="managing-concurrency-with-isolation-levels"></a>使用隔离级别管理并发性  

 执行 Transact-SQL 语句的隔离级别决定了其锁定和行版本控制行为。 隔离级别具有连接范围的作用域，一旦使用 SET TRANSACTION ISOLATION LEVEL 语句为连接设置了隔离级别，它将一直有效，直到关闭连接或设置另一个隔离级别。 当连接关闭并返回到池时，将保留上一个 SET TRANSACTION ISOLATION LEVEL 语句的隔离级别。 重新使用已入池连接的后续连接将使用在连接入池中时生效的隔离级别。  
  
 连接中发出的单个查询可以包含锁提示，这些提示可以修改单个语句或事务的隔离，但不会影响连接的隔离级别。 在存储过程或函数中设置的隔离级别或锁提示不会更改调用它们的连接的隔离级别，并且仅在存储过程或函数调用的持续时间内有效。  
  
 SQL-92 标准中定义了四个隔离级别，这四个隔离级别在早期版本的 SQL Server 中受支持：  
  
- READ UNCOMMITTED 是限制最少的隔离级别，因为它会忽略其他事务放置的锁。 在 READ UNCOMMITTED 下执行的事务可以读取其他事务尚未提交的修改数据值；这称为“脏”读。  
  
- SQL Server 的默认隔离级别为 READ COMMITTED。 它通过指定语句不能读取已由其他事务修改但尚未提交的数据值来防止脏读。 其他事务仍然可以在当前事务的各个执行语句之间修改、插入或删除数据，从而产生不可重复读取或“虚拟”数据。  
  
- REPEATABLE READ 是比 READ COMMITTED 更严格的隔离级别。 它包含 READ COMMITTED，另外还指定在当前事务提交之前，任何其他事务都不能修改或删除已由当前事务读取的数据。 由于读取数据上的共享锁一直保持到事务结束，而不是在每个语句结束时释放，因此并发级别低于 READ COMMITTED。  
  
- SERIALIZABLE 是限制最多的隔离级别，因为它锁定了键的整个范围，并在事务完成之前一直保持锁定状态。 它包含 REPEATABLE READ 并添加了限制，即在事务完成之前，其他事务不能将新行插入到事务已读取的范围中。  
  
 有关详细信息，请参阅[事务锁定和行版本控制指南](/sql/relational-databases/sql-server-transaction-locking-and-row-versioning-guide)。  
  
### <a name="snapshot-isolation-level-extensions"></a>快照隔离级别扩展  

 SQL Server 通过引入 SNAPSHOT 隔离级别并另外实现了 READ COMMITTED，引入对 SQL-92 隔离级别的扩展。 新的 READ_COMMITTED_SNAPSHOT 隔离级别可以透明的方式替换所有事务的 READ COMMITTED。  
  
- 快照隔离指定事务内读取的数据永远不会反映其他同步事务所做的更改。 事务使用事务开始时存在的数据行版本。 读取数据时不会对数据进行锁定，因此快照事务不会阻止其他事务写入数据。 写入数据的事务也不会阻止快照事务读取数据。 需要通过设置 ALLOW_SNAPSHOT_ISOLATION 数据库选项启用快照隔离才能使用它。  
  
- 在数据库中启用快照隔离时，READ_COMMITTED_SNAPSHOT 数据库选项确定默认的 READ COMMITTED 隔离级别的行为。 如果没有显式指定 READ_COMMITTED_SNAPSHOT ON，则 READ COMMITTED 将应用于所有隐式事务。 这会生成与设置 READ_COMMITTED_SNAPSHOT OFF（默认）相同的行为。 如果 READ_COMMITTED_SNAPSHOT OFF 有效，则数据库引擎使用共享锁来强制实施默认隔离级别。 如果将 READ_COMMITTED_SNAPSHOT 数据库选项设置为 ON，则数据库引擎将使用行版本控制和快照隔离作为默认值，而不是使用锁来保护数据。  
  
## <a name="how-snapshot-isolation-and-row-versioning-work"></a>快照隔离和行版本化的工作原理  

 启用 SNAPSHOT 隔离级别后，每次更新行时，SQL Server 数据库引擎在 tempdb 中存储原始行的副本，并为该行添加事务序列号****。 下面是事件发生的顺序：  
  
- 将启动新事务，并为其分配事务序列号。  
  
- 数据库引擎在事务中读取某行，并从 tempdb 中检索其序列号与事务序列号最接近并且小于事务序列号的行版本****。  
  
- 数据库引擎将检查事务序列号是否不在快照事务启动时处于活动状态的未提交事务的事务序列号的列表中。  
  
- 事务从 tempdb 中读取自事务开始以来最新的行版本****。 由于这些序列号值将高于事务序列号的值，因此它将看不到在启动事务后插入的新行。  
  
- 当前事务将看到事务开始后删除的行，因为 tempdb 中的行版本具有更低的序列号值****。  
  
 快照隔离的最终效果是，事务将看到事务开始时存在的所有数据，而不会对基础表执行或设置任何锁。 在存在争用的情况下，这可能导致性能改进。  
  
 快照事务始终使用乐观并发控制，这会保留阻止其他事务更新行的任何锁。 如果快照事务尝试提交对事务开始后更改的行的更新，则将回滚该事务，并引发错误。  
  
## <a name="working-with-snapshot-isolation-in-adonet"></a>在 ADO.NET 中使用快照隔离  

 <xref:System.Data.SqlClient.SqlTransaction> 类支持 ADO.NET 中的快照隔离。 如果数据库已启用了快照隔离，但是未配置 READ_COMMITTED_SNAPSHOT ON，必须在调用 <xref:System.Data.SqlClient.SqlConnection.BeginTransaction%2A> 方法时，使用 IsolationLevel.Snapshot 枚举值启动 <xref:System.Data.SqlClient.SqlTransaction>****。 此代码段假定连接是打开的 <xref:System.Data.SqlClient.SqlConnection> 对象。  
  
```vb  
Dim sqlTran As SqlTransaction = _  
  connection.BeginTransaction(IsolationLevel.Snapshot)  
```  
  
```csharp  
SqlTransaction sqlTran =
  connection.BeginTransaction(IsolationLevel.Snapshot);  
```  
  
### <a name="example"></a>示例  

 下面的示例演示如何通过尝试访问锁定的数据来表现不同的隔离级别，并且不应在生产代码中使用。  
  
 该代码连接到 SQL Server 中的 AdventureWorks 示例数据库上，并创建一个名为 TestSnapshot 的表，然后插入一行数据********。 此代码使用 ALTER DATABASE Transact-SQL 语句为数据库启用快照隔离，但不会设置 READ_COMMITTED_SNAPSHOT 选项，而是保留默认的 READ COMMITTED 隔离级别行为。 然后，此代码执行以下操作：  
  
- 它开始 sqlTransaction1（但未完成），后者使用 SERIALIZABLE 隔离级别来启动更新事务。 这具有锁定表的效果。  
  
- 打开第二个连接，并使用 SNAPSHOT 隔离级别开始第二个事务，读取 TestSnapshot 表中的数据****。 由于启用了快照隔离，因此此事务可以读取 sqlTransaction1 开始之前已存在的数据。  
  
- 打开第三个连接，并使用 READ COMMITTED 隔离级别启动一个事务，尝试读取表中的数据。 在这种情况下，代码无法读取数据，因为它不能读取在第一个事务的表中放置的锁和超时。如果使用了可重复读取和可序列化隔离级别，则会出现相同的结果，因为这些隔离级别也不能读取在第一个事务中放置的锁之后。  
  
- 打开第四个连接，并使用 READ UNCOMMITTED 隔离级别启动一个事务，该隔离级别对 sqlTransaction1 中未提交的值执行脏读。 如果未提交第一个事务，则此值可能在数据库中实际上不存在。  
  
- 回滚第一个事务，并通过删除 TestSnapshot 表以及禁用 AdventureWorks 数据库的快照隔离来进行清理********。  
  
> [!NOTE]
> 下面的示例使用与关闭连接池相同的连接字符串。 如果连接已入池，则重置其隔离级别不会在服务器上重置隔离级别。 因此，使用同一个入池的内部连接的后续连接将会启动，并将其隔离级别设置为已入池连接的隔离级别。 关闭连接池的另一种方法是为每个连接显式设置隔离级别。  
  
 [!code-csharp[DataWorks SnapshotIsolation.Demo#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SnapshotIsolation.Demo/CS/source.cs#1)]
 [!code-vb[DataWorks SnapshotIsolation.Demo#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SnapshotIsolation.Demo/VB/source.vb#1)]  
  
### <a name="example"></a>示例  

 下面的示例演示了在修改数据时快照隔离的行为。 此代码执行以下操作：  
  
- 连接到 AdventureWorks 示例数据库并启用 SNAPSHOT 隔离****。  
  
- 创建一个名为 TestSnapshotUpdate 的表并插入三行示例数据****。  
  
- 使用快照隔离开始 sqlTransaction1，但不完成。 在事务中选择三行数据。  
  
- 创建第二个通向 AdventureWorks 的 SqlConnection，并使用 READ COMMITTED 隔离级别创建第二个事务，更新在 sqlTransaction1 中选择的其中一行的值********。  
  
- 提交 sqlTransaction2。  
  
- 返回到 sqlTransaction1 并尝试更新 sqlTransaction1 已提交的同一行。 引发错误 3960，并且自动回滚 sqlTransaction1。 控制台窗口中将显示 SqlException.Number 和 SqlException.Message********。  
  
- 执行清理代码以关闭 AdventureWorks 中的快照隔离并删除 TestSnapshotUpdate 表********。  
  
 [!code-csharp[DataWorks SnapshotIsolation.DemoUpdate#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks SnapshotIsolation.DemoUpdate/CS/source.cs#1)]
 [!code-vb[DataWorks SnapshotIsolation.DemoUpdate#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks SnapshotIsolation.DemoUpdate/VB/source.vb#1)]  
  
### <a name="using-lock-hints-with-snapshot-isolation"></a>对快照隔离使用锁提示  

 在上面的示例中，第一个事务选择数据，第二个事务更新第一个事务完成之前的数据，这将在第一个事务尝试更新同一行时导致更新冲突。 通过在事务开始时提供锁提示，可以减少长时间运行的快照事务中的更新冲突的可能性。 以下 SELECT 语句使用 UPDLOCK 提示来锁定所选行：  
  
```sql  
SELECT * FROM TestSnapshotUpdate WITH (UPDLOCK)
  WHERE PriKey BETWEEN 1 AND 3  
```  
  
 使用 UPDLOCK 锁提示可以阻止任何行尝试在第一个事务完成之前更新行。 这可确保所选行稍后在事务中更新时不会发生冲突。 请参阅 SQL Server 联机丛书中的“锁定提示”。  
  
 如果应用程序有很多冲突，则快照隔离可能不是最佳选择。 只应在真正需要时使用提示。 应用程序的设计不应使其操作经常依赖于锁提示。  
  
## <a name="see-also"></a>请参阅

- [SQL Server 和 ADO.NET](index.md)
- [ADO.NET 概述](../ado-net-overview.md)
- [事务锁定和行版本控制指南](/sql/relational-databases/sql-server-transaction-locking-and-row-versioning-guide)
