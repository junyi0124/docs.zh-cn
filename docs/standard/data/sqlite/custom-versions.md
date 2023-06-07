---
title: 自定义 SQLite 版本
ms.date: 09/04/2020
description: 了解如何使用本机 SQLite 库的自定义版本。
ms.openlocfilehash: fbf4b4cd33e6e890ce0c0cfe0b7688487b94b4a3
ms.sourcegitcommit: 0c3ce6d2e7586d925a30f231f32046b7b3934acb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/08/2020
ms.locfileid: "89516133"
---
# <a name="custom-sqlite-versions"></a>自定义 SQLite 版本

`Microsoft.Data.Sqlite` 是在 `SQLitePCLRaw` 基础上生成的。 可以通过使用捆绑或配置 `SQLitePCLRaw` 提供程序来使用本机 SQLite 库的自定义版本。

## <a name="bundles"></a>捆绑

`SQLitePCLRaw` 提供了方便的捆绑包，使你可以轻松地在不同平台间引入正确的依赖项。 默认情况下，主 `Microsoft.Data.Sqlite` 包引入 `SQLitePCLRaw.bundle_e_sqlite3`。 若要使用不同的捆绑，请改为安装 `Microsoft.Data.Sqlite.Core` 包以及要使用的捆绑包。 捆绑由 `Microsoft.Data.Sqlite` 自动初始化。

| 捆绑 | 描述 |
|--|--|
| [SQLitePCLRaw.bundle_e_sqlite3](https://www.nuget.org/packages/SQLitePCLRaw.bundle_e_sqlite3) | 在所有平台上提供一致版本的 SQLite。 包括 FTS4、FTS5、JSON1 和 R* 树扩展。 这是默认设置。 |
| [SQLitePCLRaw.bundle_e_sqlcipher](https://www.nuget.org/packages/SQLitePCLRaw.bundle_e_sqlcipher) | 提供 `SQLCipher` 的非官方开放源代码内部版本。 |
| [SQLitePCLRaw.bundle_green](https://www.nuget.org/packages/SQLitePCLRaw.bundle_green) | 与 `bundle_e_sqlite3` 相同，不同之处是在 iOS 上使用系统 SQLite 库。 |
| [SQLitePCLRaw.bundle_sqlite3](https://www.nuget.org/packages/SQLitePCLRaw.bundle_sqlite3) | 使用系统 SQLite 库。 |
| [SQLitePCLRaw.bundle_winsqlite3](https://www.nuget.org/packages/SQLitePCLRaw.bundle_winsqlite3) | 使用 `winsqlite3.dll`（Windows 10 上的系统 SQLite 库）。 |
| [SQLitePCLRaw.bundle_zetetic](https://www.nuget.org/packages/SQLitePCLRaw.bundle_zetetic) | 使用 Zetetic 提供的官方 `SQLCipher` 内部版本（不包括在内）。 |

例如，若要使用 `SQLCipher` 的非官方开放源代码内部版本，请使用以下命令。

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.Data.Sqlite.Core
dotnet add package SQLitePCLRaw.bundle_e_sqlcipher
```

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

``` PowerShell
Install-Package Microsoft.Data.Sqlite.Core
Install-Package SQLitePCLRaw.bundle_e_sqlcipher
```

---

## <a name="sqlitepclraw-available-providers"></a>SQLitePCLRaw 可用的提供程序

当不依赖于捆绑时，可以将 SQLite 可用的提供程序用于核心程序集。

| 提供程序 | 描述 |
|--|--|
| [SQLitePCLRaw.provider.dynamic](https://www.nuget.org/packages/SQLitePCLRaw.provider.dynamic) | `dynamic` 提供程序加载本机库，而不是使用 <xref:System.Runtime.InteropServices.DllImportAttribute?displayProperty=nameWithType> 属性。 有关使用此提供程序的详细信息，请参阅[使用动态提供程序](#use-the-dynamic-provider)。 |
| [SQLitePCLRaw.provider.e_sqlite3](https://www.nuget.org/packages/SQLitePCLRaw.provider.e_sqlite3) | `e_sqlite3` 是默认提供程序。 |
| [SQLitePCLRaw.provider.e_sqlcipher](https://www.nuget.org/packages/SQLitePCLRaw.provider.e_sqlcipher) | `e_sqlcipher` 提供程序是非官方且不受支持的 `SQLCipher`。 |
| [SQLitePCLRaw.provider.sqlite3](https://www.nuget.org/packages/SQLitePCLRaw.provider.sqlite3) | `sqlite3` 提供程序是系统提供的适用于 iOS、macOS 和 Linux 的 `SQLite`。 |
| [SQLitePCLRaw.provider.sqlcipher](https://www.nuget.org/packages/SQLitePCLRaw.provider.sqlcipher) | `sqlcipher` 提供程序适用于来自 `Zetetic` 的官方 `SQLCipher` 内部版本。 |
| [SQLitePCLRaw.provider.winsqlite3](https://www.nuget.org/packages/SQLitePCLRaw.provider.winsqlite3) | `winsqlite3` 提供程序适用于 Windows 10 环境。 |

若要使用 `sqlite3` 提供程序，请使用以下命令：

### <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.Data.Sqlite.Core
dotnet add package SQLitePCLRaw.core
dotnet add package SQLitePCLRaw.provider.sqlite3
```

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

``` PowerShell
Install-Package Microsoft.Data.Sqlite.Core
Install-Package SQLitePCLRaw.core
Install-Package SQLitePCLRaw.provider.sqlite3
```

---

安装包后，你可以将提供程序设置为 `sqlite3` 实例。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/SqliteProviderSample/Program.cs)]

## <a name="use-the-dynamic-provider"></a>使用动态提供程序

通过利用 `SQLitePCLRaw.provider.dynamic_cdecl` 包，可以使用自己的 SQLite 内部版本。 在这种情况下，由你负责随应用部署本机库。 请注意，根据所使用的 .NET 平台和运行时，随应用部署本机库的详细信息差异很大。

首先，需要实现 `IGetFunctionPointer`。 .NET Core 上的实现如下：

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/SystemLibrarySample/Program.cs?name=snippet_NativeLibraryAdapter)]

接下来，配置 `SQLitePCLRaw` 提供程序。 确保在应用中使用 `Microsoft.Data.Sqlite` 之前完成此操作。 另外，请避免使用可能替代提供程序的 `SQLitePCLRaw` 捆绑包。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/SystemLibrarySample/Program.cs?name=snippet_SetProvider)]
