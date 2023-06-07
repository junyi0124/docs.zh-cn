---
title: 与 System.Data.SQLite 的比较
ms.date: 12/13/2019
description: 介绍 Microsoft.Data.Sqlite 和 System.Data.SQLite 库之间的一些差异。
ms.openlocfilehash: 076bbc6f746cf9296c96ec73047397a21a3b2558
ms.sourcegitcommit: 7088f87e9a7da144266135f4b2397e611cf0a228
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2020
ms.locfileid: "75900715"
---
# <a name="comparison-to-systemdatasqlite"></a>与 System.Data.SQLite 的比较

在 2005 年，Robert Simpson 创建了System.Data.SQLite，这是 ADO.NET 2.0 的一个 SQLite 提供程序。 在 2010 年，SQLite 团队接管了项目的维护和开发工作。 同样值得注意的是，Mono 团队在 2007 年以 Mono.Data.Sqlite 形式为代码创建了分支。 System.Data.SQLite 历史悠久，并且已发展成为具有 Visual Studio 工具的稳定且功能齐全的 ADO.NET 提供程序。 新版本会继续将与每个版本的 .NET Framework 兼容的程序集传送回版本 2.0，甚至已涉及 .NET Compact Framework 3.5。

.NET Core 的第一个版本（于 2016 年发布）是 .NET 的单一轻量级跨平台新式实现。 过时的 API 和具有更新式替代项的 API 均已被有意删除。 ADO.NET 不包含任何 DataSet API（包括 DataTable 和 DataAdapter 在内）。

实体框架团队对 System.Data.SQLite 代码库多少有些熟悉。 Brice Lambson 是 EF 团队的成员，之前曾帮助 SQLite 团队添加对实体框架版本 5 和 6 的支持。 另外，Brice 在规划 .NET Core 的同时，还试验了自己的 SQLite ADO.NET 提供程序实现。 经过长时间的讨论，实体框架团队决定基于 Brice 的原型创建 Microsoft.Data.Sqlite。 这将使他们能够创建一种全新的轻量级新式实现方式，它将与 .NET Core 的目标保持一致。

下面的示例展示了它的新式特性，此示例代码同时在 System.Data.SQLite 和 Microsoft.Data.Sqlite 中创建[用户定义函数](user-defined-functions.md)。

```csharp
// System.Data.SQLite
connection.BindFunction(
    new SQLiteFunctionAttribute("ceiling", 1, FunctionType.Scalar),
    (Func<object[], object>)((object[] args) => Math.Ceiling((double)((object[])args[1])[0])),
    null);

// Microsoft.Data.Sqlite
connection.CreateFunction(
    "ceiling",
    (double arg) => Math.Ceiling(arg));
```

2017 年，.NET Core 2.0 发生了战略变化。 团队确定了与 .NET Framework 的兼容性对于 .NET Core 的成功至关重要。 因此，将许多已删除的 API（包括 DataSet API）都重新添加回来。 就像对其他许多工具所做的一样，这个不受限制的 System.Data.SQLite 也允许将其移植到 .NET Core。 但是，Microsoft.Data.Sqlite 的最初目标是仍保持轻量级的新式特性。 请参阅 [ADO.NET 限制](adonet-limitations.md)，详细了解哪些 ADO.NET API 不是由 Microsoft.Data.Sqlite 实现的。

将新功能添加到 Microsoft.Data.Sqlite 后，将考虑 System.Data.SQLite 的设计。 我们会尽可能尝试将两者之间的变化降到最少，以简化两者之间的过渡。

## <a name="data-types"></a>数据类型

Microsoft.Data.Sqlite 和 System.Data.SQLite 之间的最大区别是处理数据类型的方式。 如[数据类型](types.md)中所述，Microsoft.Data.Sqlite 不会尝试隐藏 SQLite 的基础奇怪特征，此特征允许将任意字符串指定为列类型，并且只有四个基元类型：INTEGER、REAL、TEXT 和 BLOB。

System.Data.SQLite 将其他语义应用于列类型，从而将它们直接映射到 .NET 类型。 这为提供程序提供了一种更强类型的外观，但它具有一些粗糙的边缘。 例如，它们必须引入一个新的 SQL 语句 (TYPES) 来指定 SELECT 语句中表达式的列类型。

## <a name="connection-strings"></a>连接字符串

Microsoft.Data.Sqlite 的[连接字符串](connection-strings.md)关键字要少很多。 下表显示了可以替代使用的替代方法。

| 关键字          | 替代项                                         |
| ---------------- | --------------------------------------------------- |
| Cache Size       | 发送 `PRAGMA cache_size = <pages>`                  |
| 默认超时  | 对 SqliteConnection 使用 DefaultTimeout 属性 |
| FailIfMissing    | 使用 `Mode=ReadWrite`                                |
| FullUri          | 使用 Data Source 关键字                         |
| Journal Mode     | 发送 `PRAGMA journal_mode = <mode>`                 |
| Legacy Format    | 发送 `PRAGMA legacy_file_format = 1`                |
| Max Page Count   | 发送 `PRAGMA max_page_count = <pages>`              |
| Page Size        | 发送 `PRAGMA page_size = <bytes>`                   |
| 只读        | 使用 `Mode=ReadOnly`                                 |
| 同步      | 发送 `PRAGMA synchronous = <mode>`                  |
| URI              | 使用 Data Source 关键字                         |
| UseUTF16Encoding | 发送 `PRAGMA encoding = 'UTF-16'`                   |

## <a name="authorization"></a>授权

Microsoft.Data.Sqlite 没有任何 API 公开 SQLite 的授权回调。 请使用问题 [#13835](https://github.com/dotnet/efcore/issues/13835) 提供有关此功能的反馈。

## <a name="data-change-notifications"></a>数据更改通知

Microsoft.Data.Sqlite 没有任何 API 公开 SQLite 的数据更改通知。 请使用问题 [#13827](https://github.com/dotnet/efcore/issues/13827) 提供有关此功能的反馈。

## <a name="virtual-table-modules"></a>虚拟表模块

Microsoft.Data.Sqlite 没有用于创建虚拟表模块的任何 API。 请使用问题 [#13823](https://github.com/dotnet/efcore/issues/13823) 提供有关此功能的反馈。

## <a name="see-also"></a>请参阅

* [数据类型](types.md)
* [连接字符串](connection-strings.md)
* [加密](encryption.md)
* [ADO.NET 限制](adonet-limitations.md)
* [Dapper 限制](dapper-limitations.md)
