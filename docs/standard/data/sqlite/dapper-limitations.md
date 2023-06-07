---
title: Dapper 限制
ms.date: 12/13/2019
description: 描述使用 Dapper 时可能会遇到的一些限制。
ms.openlocfilehash: 396507f25f591a9ab5c3bb07c0af6fd8d175e4ea
ms.sourcegitcommit: 7088f87e9a7da144266135f4b2397e611cf0a228
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2020
ms.locfileid: "75901198"
---
# <a name="dapper-limitations"></a>Dapper 限制

使用带有 [Dapper](https://stackexchange.github.io/Dapper/) 的 Microsoft.Data.Sqlite 时，需要注意几个限制。

## <a name="parameters"></a>参数

SQLite 参数名称区分大小写。 确保 SQL 中使用的参数名称与匿名对象属性的大小写匹配。 问题 [#18861](https://github.com/dotnet/efcore/issues/18861) 将改进此体验。

Dapper 还要求参数使用 `@` 前缀。 其他前缀不起作用。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/DapperSample/Program.cs?name=snippet_Parameter)]

## <a name="data-types"></a>数据类型

Dapper 使用 SqliteDataReader 索引器读取值。 此索引器的返回类型是 object，这意味着它将只返回 long、double、string 或 byte[] 值。 有关详细信息，请参阅[数据类型](types.md)。 Dapper 处理这些基元类型和其他基元类型之间的大多数转换。 遗憾的是，它不处理 `DateTimeOffset`、`Guid` 或 `TimeSpan`。 如果希望在结果中使用这些类型，请创建类型处理程序。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/DapperSample/Program.cs?name=snippet_TypeHandlers)]

请不要忘记在查询前添加类型处理程序。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/DapperSample/Program.cs?name=snippet_AddTypeHandlers)]

## <a name="see-also"></a>请参阅

* [数据类型](types.md)
* [异步限制](async.md)
