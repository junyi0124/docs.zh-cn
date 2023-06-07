---
title: 事务
ms.date: 09/08/2020
description: 了解如何使用事务。
ms.openlocfilehash: 50c4cd1023eac892cafc3ae4395e9168bd8e9f36
ms.sourcegitcommit: aa6d8a90a4f5d8fe0f6e967980b8c98433f05a44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2020
ms.locfileid: "90678857"
---
# <a name="transactions"></a>事务

通过事务可以将多个 SQL 语句组合成一个工作单元，并将其作为一个原子单元提交到数据库。 如果事务中的任何语句失败，可以回滚前面语句所做的更改。 事务启动时的数据库初始状态会予以保留。 当一次对数据库进行大量修改时，使用事务也可以提高 SQLite 上的性能。

## <a name="concurrency"></a>并发

在 SQLite 中，数据库中一次只允许有一个事务具有挂起的更改。 因此，如果另一个事务需要很长时间才能完成，则对 <xref:Microsoft.Data.Sqlite.SqliteCommand> 调用 <xref:Microsoft.Data.Sqlite.SqliteConnection.BeginTransaction%2A> 和 `Execute` 方法可能会超时。

有关锁定、重试和超时的详细信息，请参阅[数据库错误](database-errors.md)。

## <a name="isolation-levels"></a>隔离级别

默认情况下，在 SQLite 中，事务是**可序列化**的。 此隔离级别可确保完全隔离事务中所做的任何更改。 在事务之外执行的其他语句不受事务更改的影响。

SQLite 还支持在使用共享缓存时读取未提交的内容  。 此级别允许脏读、不可重复读和幻像：

- 当事务外部的查询返回一个事务中挂起的更改，但事务中的更改被回滚时，会发生“脏读”  。 结果包含从不实际提交到数据库的数据。

- 如果某一事务两次查询相同的行，但由于在两个查询之间另一个事务进行了更改导致结果不同，这时会发生“不可重复读”  。

- “幻像”  在事务过程中为满足查询的 where 子句而更改或添加的行。 如果允许的话，同一查询在同一事务中执行两次时可能返回不同的行。

Microsoft.Data.Sqlite 将传递给 <xref:Microsoft.Data.Sqlite.SqliteConnection.BeginTransaction%2A> 的 IsolationLevel 视为最低级别。 实际隔离级别将提升为读取未提交内容或可序列化。

下面的代码模拟脏读。 请注意，连接字符串必须包含 `Cache=Shared`。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/DirtyReadSample/Program.cs?name=snippet_DirtyRead)]

## <a name="deferred-transactions"></a>延迟的事务

从 Microsoft.Data.Sqlite 版本 5.0 开始，可以延迟事务。 这会延迟在数据库中创建实际事务，直到执行第一个命令。 还会根据该事务的命令需求，使该事务逐步从读取事务升级到写入事务。 这有助于在事务期间并发访问数据库。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/DeferredTransactionSample/Program.cs?name=snippet_DeferredTransaction)]

> [!WARNING]
> 如果延迟的事务中的命令导致事务在数据库锁定时从读取事务升级到写入事务，则这些命令可能会失败。 发生这种情况时，应用程序将需要重试整个事务。
