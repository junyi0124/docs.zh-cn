---
title: 异步限制
ms.date: 09/04/2020
description: 说明异步 API 的限制以及可以使用的一些替代方法。
ms.openlocfilehash: 8b14fcfeb12d331d8d43ca6d77332007a12ae5dc
ms.sourcegitcommit: 0c3ce6d2e7586d925a30f231f32046b7b3934acb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/08/2020
ms.locfileid: "89515990"
---
# <a name="async-limitations"></a>异步限制

SQLite 不支持异步 I/O。 异步 ADO.NET 方法将在 Microsoft.Data.Sqlite 中同步执行。 请避免调用它们。

请改用[共享缓存](connection-strings.md#cache)和[预写日志记录](https://www.sqlite.org/wal.html)，以提高性能和并发性。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/AsyncSample/Program.cs?name=snippet_WAL)]

> [!TIP]
> 默认情况下，在使用 [Entity Framework Core](/ef/core/) 创建的数据库上启用预写日志记录。
