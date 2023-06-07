---
title: 事务和并发性
ms.date: 03/30/2017
ms.assetid: f46570de-9e50-4fe6-8710-a8c31fa8569b
ms.openlocfilehash: 049e402345e1abbb46739e48c89101207a43bb27
ms.sourcegitcommit: 5b475c1855b32cf78d2d1bbb4295e4c236f39464
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/24/2020
ms.locfileid: "91191666"
---
# <a name="transactions-and-concurrency"></a>事务和并发性

事务由作为包执行的单个命令或一组命令组成。 通过事务可以将多个操合并为单个工作单元。 如果在事务中的某一点发生故障，则所有更新都可以回滚到其事务前状态。  
  
 事务必须符合 ACID 属性（原子性、一致性、隔离和持久性）才能保证数据的一致性。 大多数关系数据库系统（例如 Microsoft SQL Server）都可在客户端应用程序执行更新、插入或删除操作时为事务提供锁定、日志记录和事务管理功能，以此来支持事务。  
  
> [!NOTE]
> 如果锁定持续时间过长，则涉及多个资源的事务可能会降低并发性。 因此，事务应尽量保持简短。  
  
 如果一个事务涉及同一个数据库或服务器中的多个表，则存储过程中的显式事务通常可以更好地执行。 您可以通过使用 Transact-SQL `BEGIN TRANSACTION`、`COMMIT TRANSACTION` 和 `ROLLBACK TRANSACTION` 语句在 SQL Server 存储过程中创建事务。 有关详细信息，请参阅 SQL Server 联机丛书。  
  
 涉及不同资源管理器的事务（例如 SQL Server 和 Oracle 之间的事务）需要分布式事务。  
  
## <a name="in-this-section"></a>本节内容  

 [本地事务](local-transactions.md)  
 演示如何对数据库执行事务。  
  
 [分布式事务](distributed-transactions.md)  
 描述如何在 ADO.NET 中执行分布式事务。  
  
 [System.Transactions 与 SQL Server 的集成](system-transactions-integration-with-sql-server.md)  
 描述 <xref:System.Transactions> 与使用分布式事务的 SQL Server 的集成。  
  
 [开放式并发](optimistic-concurrency.md)  
 描述开放式并发和保守式并发，以及如何测试并发冲突。  
  
## <a name="see-also"></a>请参阅

- [事务基础知识](../transactions/transaction-fundamentals.md)
- [连接数据源](connecting-to-a-data-source.md)
- [命令和参数](commands-and-parameters.md)
- [DataAdapter 和 DataReader](dataadapters-and-datareaders.md)
- [DbProviderFactories](dbproviderfactories.md)
- [ADO.NET 概述](ado-net-overview.md)
