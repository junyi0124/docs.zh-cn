---
title: 数据库错误
ms.date: 12/13/2019
description: 介绍库如何处理数据库错误和停用。
ms.openlocfilehash: 9a613b6f94a89dc40e627c14f46836be77080035
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450488"
---
# <a name="database-errors"></a>数据库错误

遇到 SQLite 错误时会引发 <xref:Microsoft.Data.Sqlite.SqliteException>。 消息由 SQLite 提供。 `SqliteErrorCode` 和 `SqliteExtendedErrorCode` 属性包含错误的 SQLite [结果代码](https://www.sqlite.org/rescode.html)。

在 Microsoft.Data.Sqlite 与本机 SQLite 库进行交互的任何时候，都可能会遇到错误。 以下列表显示可能发生错误的常见情况：

* 打开连接。
* 启动事务。
* 执行命令。
* 调用 <xref:Microsoft.Data.Sqlite.SqliteDataReader.NextResult%2A>。

仔细考虑应用将如何处理这些错误。

## <a name="locking-retries-and-timeouts"></a>锁定、重试和超时

当涉及锁定表和数据库文件时，SQLite 会比较主动。 如果应用启用了任何并发数据库访问，则可能会遇到繁忙和锁定错误。 可以使用[共享缓存](connection-strings.md#cache)和[预写日志记录](async.md)来缓解大量错误。

每当 Microsoft.Data.Sqlite 遇到繁忙或锁定错误时，它会自动重试，直到它成功或达到命令超时值。

可以通过设置 <xref:Microsoft.Data.Sqlite.SqliteCommand.CommandTimeout%2A> 来增加命令的超时值。 默认超时值为 30 秒。 值 `0` 意味着无超时。

```csharp
// Retry for 60 seconds while locked
command.CommandTimeout = 60;
```

Microsoft.Data.Sqlite 有时需要创建隐式命令对象。 例如，在 BeginTransaction 期间。 若要为这些命令设置超时值，请使用 <xref:Microsoft.Data.Sqlite.SqliteConnection.DefaultTimeout%2A>。

```csharp
// Set the default timeout of all commands on this connection
connection.DefaultTimeout = 60;
```

## <a name="see-also"></a>请参阅

* [SQLite 错误代码](https://www.sqlite.org/rescode.html)
* [SQLite 中的文件锁定](https://www.sqlite.org/lockingv3.html)
* [共享缓存模式](https://www.sqlite.org/sharedcache.html)
* [预写日志记录](https://www.sqlite.org/wal.html)
