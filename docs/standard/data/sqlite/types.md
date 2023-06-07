---
title: 数据类型
ms.date: 12/13/2019
description: 介绍支持的数据类型和这些数据类型的一些限制。
ms.openlocfilehash: a11ff382f80cd979506d6195c299c8234c3eb8ea
ms.sourcegitcommit: c91110ef6ee3fedb591f3d628dc17739c4a7071e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/15/2020
ms.locfileid: "81389041"
---
# <a name="data-types"></a>数据类型

SQLite 仅有四个基元数据类型：INTEGER、REAL、TEXT 和 BLOB。 将数据库值返回为 `object` 的 API 只返回这四种类型之一。 Microsoft.Data.Sqlite 支持其他 .NET 类型，但最终强制这些值在这些类型和四种基元类型中的一种类型之间进行转换。

| .NET           | SQLite  | 备注                                                       |
| -------------- | ------- | ------------------------------------------------------------- |
| Boolean        | INTEGER | `0` 或 `1`                                                    |
| Byte           | INTEGER |                                                               |
| Byte[]         | BLOB    |                                                               |
| Char           | TEXT    | UTF-8                                                         |
| DateTime       | TEXT    | yyyy-MM-dd HH:mm:ss.FFFFFFF                                   |
| DateTimeOffset | TEXT    | yyyy-MM-dd HH:mm:ss.FFFFFFFzzz                                |
| 十进制        | TEXT    | `0.0###########################` 格式。 REAL 将有损。 |
| Double         | real    |                                                               |
| GUID           | TEXT    | 00000000-0000-0000-0000-000000000000                          |
| Int16          | INTEGER |                                                               |
| Int32          | INTEGER |                                                               |
| Int64          | INTEGER |                                                               |
| SByte          | INTEGER |                                                               |
| Single         | real    |                                                               |
| String         | TEXT    | UTF-8                                                         |
| TimeSpan       | TEXT    | d.hh:mm:ss.fffffff                                            |
| UInt16         | INTEGER |                                                               |
| UInt32         | INTEGER |                                                               |
| UInt64         | INTEGER | 大值溢出                                         |

## <a name="alternative-types"></a>替代类型

某些 .NET 类型可以从替代 SQLite 类型中读取。 还可以将参数配置为使用这些替代类型。 有关详细信息，请参阅[参数](parameters.md#alternative-types)。

| .NET           | SQLite  | 备注          |
| -------------- | ------- | ---------------- |
| Char           | INTEGER | UTF-16           |
| DateTime       | real    | 儒略日值 |
| DateTimeOffset | real    | 儒略日值 |
| GUID           | BLOB    |                  |
| TimeSpan       | real    | 以天为单位          |

例如，下面的查询从结果集的 REAL 列中读取 TimeSpan 值。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/DateAndTimeSample/Program.cs?name=snippet_AlternativeType)]

## <a name="column-types"></a>列类型

SQLite 使用动态类型系统，其中值的类型与值本身相关联，而不是与存储值的列相关联。 可以随意使用任何所需的列类型名称。 Microsoft.Data.Sqlite 不会对这些名称应用任何额外的语义。

列类型名称确实对[类型相关性](https://www.sqlite.org/datatype3.html#type_affinity)有影响。 一个常见的问题是，使用 STRING 列类型会试图将值转换为 INTEGER 或 REAL，这可能会导致意外的结果。 建议仅使用四个基元 SQLite 类型名称：INTEGER、REAL、TEXT 和 BLOB。

SQLite 可让用户指定类型方面，例如长度、精度和小数位数，但数据库引擎不强制执行这些方面。 你的应用负责强制执行这些方面。

## <a name="see-also"></a>请参阅

- [SQLite 中的数据类型](https://www.sqlite.org/datatype3.html)
