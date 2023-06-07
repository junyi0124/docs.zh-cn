---
title: 参数
ms.date: 12/13/2019
description: 了解如何使用 SQL 参数。
ms.openlocfilehash: b24610a5cb65e2b24171452acef9bf55b4995431
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/15/2020
ms.locfileid: "84768945"
---
# <a name="parameters"></a>参数

可以使用参数来防范 SQL 注入攻击。 与其将用户输入与 SQL 语句连接起来，不如使用参数来确保输入只被视为文本值，而不会执行。 在 SQLite 中，通常允许在 SQL 语句中允许文本的任何位置使用参数。

参数可以使用 `:`、`@` 或 `$` 作为前缀。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/HelloWorldSample/Program.cs?name=snippet_Parameter)]

若要详细了解如何将 .NET 值映射到 SQLite 值，请参阅[数据类型](types.md)。

## <a name="truncation"></a>截断

使用 <xref:Microsoft.Data.Sqlite.SqliteParameter.Size> 属性可截断 TEXT 和 BLOB 值。

```csharp
// Truncate name to 30 characters
command.Parameters.AddWithValue("$name", name).Size = 30;
```

## <a name="alternative-types"></a>替代类型

有时，你可能想要使用替代的 SQLite 类型。 通过设置 <xref:Microsoft.Data.Sqlite.SqliteParameter.SqliteType> 属性可实现此目的。

可以使用以下替代类型映射。 有关默认映射，请参阅[数据类型](types.md)。

| “值”          | SqliteType | 备注          |
| -------------- | ---------- | ---------------- |
| Char           | 整数    | UTF-16           |
| DateTime       | Real       | 儒略日值 |
| DateTimeOffset | Real       | 儒略日值 |
| GUID           | Blob       |                  |
| TimeSpan       | Real       | 以天为单位          |

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/DateAndTimeSample/Program.cs?name=snippet_SqliteType)]

## <a name="output-parameters"></a>输出参数

SQLite 不支持输出参数。 改为返回查询结果中的值。

## <a name="see-also"></a>请参阅

* [数据类型](types.md)
