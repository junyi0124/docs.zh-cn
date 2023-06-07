---
title: Blob I/O
ms.date: 12/13/2019
description: 了解如何使用 SQLite 的 BLOB I/O 功能。
ms.openlocfilehash: 0c133deacdc19684eca3a6724fb398dc01fda558
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450302"
---
# <a name="blob-io"></a>Blob I/O

通过将数据流式传输到以及流式传输出数据库，可以在读写大型对象的同时减少内存使用量。 在分析或转换数据时，此功能特别有用。

首先像往常一样插入一个行。 使用 `zeroblob()` SQL 函数分配数据库空间，用于保存大型对象。 通过 `last_insert_rowid()` 函数，可快捷地获得其 rowid。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/StreamingSample/Program.cs?name=snippet_Insert)]

插入行后，打开一个流，以使用 <xref:Microsoft.Data.Sqlite.SqliteBlob> 写入大型对象。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/StreamingSample/Program.cs?name=snippet_Write)]

若要将大型对象流式传输出数据库，除了选中大型对象的列之外，还必须选中 rowid 或它的一个别名，如此处所示。 若未选中 rowid，则会将整个对象加载到内存。 如果操作正确，`GetStream()` 返回的对象将为 `SqliteBlob`。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/StreamingSample/Program.cs?name=snippet_Read)]
