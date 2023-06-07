---
title: dotnet tool uninstall 命令
description: dotnet tool uninstall 命令从计算机上卸载指定的 .NET 工具。
ms.date: 02/14/2020
ms.openlocfilehash: 34dffde8f9c930327c6b42d1d89bb4f511959fb2
ms.sourcegitcommit: b201d177e01480a139622f3bf8facd367657a472
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2020
ms.locfileid: "94634085"
---
# <a name="dotnet-tool-uninstall"></a>dotnet tool uninstall

本文适用于： ✔️ .NET Core 2.1 SDK 及更高版本

## <a name="name"></a>“属性”

`dotnet tool uninstall` - 从计算机上卸载指定的 [.NET 工具](global-tools.md)。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet tool uninstall <PACKAGE_NAME> -g|--global

dotnet tool uninstall <PACKAGE_NAME> --tool-path <PATH>

dotnet tool uninstall <PACKAGE_NAME>

dotnet tool uninstall -h|--help
```

## <a name="description"></a>描述

`dotnet tool uninstall` 提供一种从计算机上卸载 .NET 工具的方法。 若要使用命令，请指定以下选项之一：

* 若要卸载安装在默认位置的全局工具，请使用 `--global` 选项。
* 若要卸载安装在自定义位置的全局工具，请使用 `--tool-path` 选项。
* 若要卸载本地工具，请省略 `--global` 和 `--tool-path` 选项。

**本地工具从 .NET Core SDK 3.0 开始可用。**

## <a name="arguments"></a>自变量

- **`PACKAGE_NAME`**

  包含要卸载的 .NET 工具的 NuGet 包的名称/ID。 你可以使用 [dotnet tool list](dotnet-tool-list.md) 命令查找包名称。

## <a name="options"></a>选项

- **`-g|--global`**

  指定要从用户范围的安装中删除工具。 不能与 `--tool-path` 选项一起使用。 省略 `--global` 和 `--tool-path` 指定要删除的工具是本地工具。

- **`-h|--help`**

  打印出有关命令的简短帮助。

- **`--tool-path <PATH>`**

  指定卸载工具的位置。 路径可以是绝对的，也可以是相对的。 不能与 `--global` 选项一起使用。 省略 `--global` 和 `--tool-path` 指定要删除的工具是本地工具。

## <a name="examples"></a>示例

- **`dotnet tool uninstall -g dotnetsay`**

  卸载 [dotnetsay](https://www.nuget.org/packages/dotnetsay/) 全局工具。

- **`dotnet tool uninstall dotnetsay --tool-path c:\global-tools`**

  从特定 Windows 目录卸载 [dotnetsay](https://www.nuget.org/packages/dotnetsay/) 全局工具。

- **`dotnet tool uninstall dotnetsay --tool-path ~/bin`**

  从特定 Linux/macOS 目录卸载 [dotnetsay](https://www.nuget.org/packages/dotnetsay/) 全局工具。

- **`dotnet tool uninstall dotnetsay`**

  从当前目录卸载 [dotnetsay](https://www.nuget.org/packages/dotnetsay/) 全局工具。

## <a name="see-also"></a>请参阅

- [.NET 工具](global-tools.md)
- [教程：使用 .NET CLI 安装和使用 .NET 全局工具](global-tools-how-to-use.md)
- [教程：使用 .NET CLI 安装和使用 .NET 本地工具](local-tools-how-to-use.md)
