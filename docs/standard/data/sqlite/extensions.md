---
title: 扩展
ms.date: 12/13/2019
description: 了解如何加载 SQLite 扩展。
ms.openlocfilehash: 51c705349c25240fe42e0edda8004a3e3b013ca3
ms.sourcegitcommit: 7980a91f90ae5eca859db7e6bfa03e23e76a1a50
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2020
ms.locfileid: "81242954"
---
# <a name="extensions"></a>扩展

SQLite 支持在运行时加载扩展。 扩展包括额外的 SQL 函数、排序规则、虚拟表等。

.NET Core 包含用于在其他位置（如引用的 NuGet 包）查找本机库的附加逻辑。 遗憾的是，SQLite 无法利用此逻辑；它直接调用平台 API 来加载库。 因此，你可能需要在加载 SQLite 扩展前修改 PATH、LD_LIBRARY_PATH 或 DYLD_LIBRARY_PATH 环境变量。 GitHub 上的[一个示例](https://github.com/dotnet/docs/blob/master/samples/snippets/standard/data/sqlite/ExtensionsSample/Program.cs)演示了如何在引用的 NuGet 包内查找当前运行时的二进制文件。

若要加载扩展，请调用 <xref:Microsoft.Data.Sqlite.SqliteConnection.LoadExtension%2A> 方法。 即使连接关闭然后重新打开，Microsoft.Data.Sqlite 也会确保扩展仍处于加载状态。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/ExtensionsSample/Program.cs?name=snippet_LoadExtension)]

## <a name="see-also"></a>请参阅

* [运行时可加载扩展](https://www.sqlite.org/loadext.html)
