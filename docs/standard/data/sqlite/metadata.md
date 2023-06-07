---
title: 元数据
ms.date: 12/13/2019
description: 了解如何检索有关数据库的元数据。
ms.openlocfilehash: b2f2704a748627d9943943fa2fa7b1b7e9f3007f
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450428"
---
# <a name="metadata"></a>元数据

提供两种用于检索 ADO.NET 中元数据的 API。 一个检索有关查询结果的元数据。 另一个检索有关数据库架构的元数据。

## <a name="query-result-metadata"></a>查询结果元数据

可以及使用 `SqliteDataReader` 上的 <xref:Microsoft.Data.Sqlite.SqliteDataReader.GetSchemaTable%2A> 方法检索有关查询结果的元数据。 返回的 <xref:System.Data.DataTable> 包含以下列：

| 列             | 类型    | 描述                                                               |
| ------------------ | ------- | ------------------------------------------------------------------------- |
| `AllowDBNull`      | Boolean | 如果原点列可以为 NULL，则为 True。                                    |
| `BaseCatalogName`  | String  | 原点列的数据库的名称。 表达式始终为 NULL。    |
| `BaseColumnName`   | String  | 原点列的真实名称。 表达式始终为 NULL。    |
| `BaseSchemaName`   | String  | 始终为 NULL。 SQLite 不支持架构。                              |
| `BaseServerName`   | String  | 在连接字符串中指定的数据库文件的路径。         |
| `BaseTableName`    | String  | 原点列的表的名称。 表达式始终为 NULL。       |
| `ColumnName`       | String  | 结果集中列的名称或别名。                        |
| `ColumnOrdinal`    | Int32   | 结果集中列的序号。                              |
| `ColumnSize`       | Int32   | 始终为 -1。 在将来的 `Microsoft.Data.Sqlite` 版本中这可能会发生变化。   |
| `DataType`         | 类型    | 列的默认 .NET 数据类型。                                 |
| `DataTypeName`     | String  | 列的 SQLite 数据类型。                                       |
| `IsAliased`        | Boolean | 如果列名在结果集中具有别名，则为 True。                     |
| `IsAutoIncrement`  | Boolean | 如果原点列是使用 AUTOINCREMENT 关键字创建的，则为 True。     |
| `IsExpression`     | Boolean | 如果列源自查询中的表达式，则为 True。            |
| `IsKey`            | Boolean | 如果原点列是主键的一部分，则为 True。                     |
| `IsUnique`         | Boolean | 如果原点列是唯一的，则为 True。                                      |
| `NumericPrecision` | Int16   | 始终为 NULL。 在将来的 `Microsoft.Data.Sqlite` 版本中这可能会发生变化。 |
| `NumericScale`     | Int16   | 始终为 NULL。 在将来的 `Microsoft.Data.Sqlite` 版本中这可能会发生变化。 |

以下示例显示如何使用 `GetSchemaTable` 创建显示有关结果的元数据的调试字符串：

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/ResultMetadataSample/Program.cs?name=snippet_ResultMetadata)]

例如，此查询将产生以下调试字符串：

```sql
SELECT id AS post_id,
       title,
       body,
       random() AS random
FROM post
```

```output
post.id AS post_id INTEGER PRIMARY KEY AUTOINCREMENT
post.title TEXT NOT NULL UNIQUE
post.body TEXT
(expression) AS random BLOB
```

## <a name="schema-metadata"></a>架构元数据

Microsoft.Data.Sqlite 未在 DbConnection 上实现 GetSchema 方法。 相反，可以使用 [sqlite_master](https://www.sqlite.org/fileformat.html#storage_of_the_sql_database_schema) 表和 PRAGMA 语句（如 [table_info](https://www.sqlite.org/pragma.html#pragma_table_info) 和 [foreign_key_list](https://www.sqlite.org/pragma.html#pragma_foreign_key_list)）直接查询架构信息。

例如，此查询将检索数据库中所有列的元数据。

```sql
SELECT t.name AS tbl_name, c.name, c.type, c.notnull, c.dflt_value, c.pk
FROM sqlite_master AS t,
     pragma_table_info(t.name) AS c
WHERE t.type = 'table';
```

## <a name="see-also"></a>请参阅

* [SQL 数据库架构的存储](https://www.sqlite.org/fileformat.html#storage_of_the_sql_database_schema)
* [PRAGMA 语句](https://www.sqlite.org/pragma.html)
* [数据类型](types.md)
