---
title: 概述
ms.date: 12/13/2019
description: 有关 Microsoft.Data.Sqlite 的概述
ms.openlocfilehash: 7c952848f7e7ea04f11fe9340f77a1f376a1be07
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90552435"
---
# <a name="microsoftdatasqlite-overview"></a>Microsoft.Data.Sqlite 概述

Microsoft.Data.Sqlite 是用于 SQLite 的轻型 [ADO.NET](../../../framework/data/adonet/index.md) 提供程序。 用于 SQLite 的 [Entity Framework Core](/ef/core/) 提供程序就是基于此库而构建。 但它还可以单独使用，也可以与其他数据访问库一起使用。

## <a name="installation"></a>安装

可从 [NuGet](https://www.nuget.org/packages/Microsoft.Data.Sqlite) 获取最新的稳定版本。

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.Data.Sqlite
```

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

``` PowerShell
Install-Package Microsoft.Data.Sqlite
```

---

## <a name="usage"></a>用法

此库实现了用于连接、命令、数据读取器等的常见 ADO.NET 抽象。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/HelloWorldSample/Program.cs?name=snippet_HelloWorld)]

## <a name="see-also"></a>请参阅

* [连接字符串](connection-strings.md)
* [API 参考](../../../../api/index.md?view=msdata-sqlite-3.0)
* [SQL 语法](https://www.sqlite.org/lang.html)
