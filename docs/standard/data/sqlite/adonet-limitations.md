---
title: ADO.NET 限制
ms.date: 12/13/2019
description: 描述你可能会遇到的一些 ADO.NET 限制。
ms.openlocfilehash: 8664b73071fc859ed30080f549b05e7d6ed020f4
ms.sourcegitcommit: 7088f87e9a7da144266135f4b2397e611cf0a228
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2020
ms.locfileid: "75901252"
---
# <a name="adonet-limitations"></a>ADO.NET 限制

Microsoft.Data.Sqlite 提供了许多 ADO.NET 抽象实现，但存在一些限制。

## <a name="database-schema-information"></a>数据库架构信息

使用 <xref:Microsoft.Data.Sqlite.SqliteDataReader.GetSchemaTable%2A> 方法可获取有关查询结果的元数据。

不需要实现 `DbConnection.GetSchema()`。 此 API 定义不明确，因此，我们建议直接使用标准 SQLite API（如 [sqlite_master](https://www.sqlite.org/fileformat.html#storage_of_the_sql_database_schema) 表和 [table_info](https://www.sqlite.org/pragma.html#pragma_table_info) PRAGMA）直接检索数据库元数据。

有关详细信息，请参阅[元数据](metadata.md)。

## <a name="systemtransactions"></a>System.Transactions

Microsoft.Data.Sqlite 尚不支持 System.Transactions。 请改用 ADO.NET 事务。 有关详细信息，请参阅[事务](transactions.md)。

在问题 [#13825](https://github.com/dotnet/efcore/issues/13825) 上提供有关缺少对 System.Transactions 的支持的反馈。

## <a name="data-adapters"></a>数据适配器

`DbDataAdapter` 尚未通过 Microsoft.Data.Sqlite 实现。 这意味着，只能使用 ADO.NET `DataSet` 和 `DataTable` 来加载数据，而不能对其进行更新。

请使用问题 [#13838](https://github.com/dotnet/efcore/issues/13838) 提供有关实现 `DbDataAdapter` 的反馈。

## <a name="output-parameters"></a>输出参数

SQLite 不支持输出参数。

## <a name="positional-parameters"></a>位置参数

Microsoft.Data.Sqlite 仅支持命名[参数](parameters.md)。 不支持位置参数。

## <a name="stored-procedures"></a>存储过程

SQLite 不支持存储过程。

## <a name="isolation-levels"></a>隔离级别

SQLite 事务不支持 `Chaos` 和 `Snapshot` 隔离级别。

## <a name="see-also"></a>请参阅

* [异步限制](async.md)
* [数据类型](types.md)
* [事务](transactions.md)
