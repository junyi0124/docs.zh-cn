---
title: dotnet tool update 命令
description: dotnet tool update 命令更新计算机上指定的 .NET 工具。
ms.date: 07/08/2020
ms.openlocfilehash: 18b153e53a6dbcb32e50ae4a7d06a1c2f53d1eb5
ms.sourcegitcommit: b201d177e01480a139622f3bf8facd367657a472
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2020
ms.locfileid: "94634072"
---
# <a name="dotnet-tool-update"></a>dotnet tool update

本文适用于： ✔️ .NET Core 2.1 SDK 及更高版本

## <a name="name"></a>“属性”

`dotnet tool update` - 在计算机上更新指定的 [.NET 工具](global-tools.md)。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet tool update <PACKAGE_ID> -g|--global
    [--add-source <SOURCE>] [--configfile <FILE>]
    [--disable-parallel] [--framework <FRAMEWORK>]
    [--ignore-failed-sources] [--interactive] [--no-cache]
    [-v|--verbosity <LEVEL>] [--version <VERSION>]

dotnet tool update <PACKAGE_ID> --tool-path <PATH>
    [--add-source <SOURCE>] [--configfile <FILE>]
    [--disable-parallel] [--framework <FRAMEWORK>]
    [--ignore-failed-sources] [--interactive] [--no-cache]
    [-v|--verbosity <LEVEL>] [--version <VERSION>]

dotnet tool update <PACKAGE_ID> --local
    [--add-source <SOURCE>] [--configfile <FILE>]
    [--disable-parallel] [--framework <FRAMEWORK>]
    [--ignore-failed-sources] [--interactive] [--no-cache]
    [--tool-manifest <PATH>]
    [-v|--verbosity <LEVEL>] [--version <VERSION>]

dotnet tool update -h|--help
```

## <a name="description"></a>描述

`dotnet tool update` 命令让你可以将计算机上的 .NET Core 工具更新为包的最新稳定版。 此命令卸载并重新安装工具，有效地对工具进行更新。 若要使用命令，请指定以下选项之一：

* 若要更新安装在默认位置的全局工具，请使用 `--global` 选项
* 若要更新安装在自定义位置的全局工具，请使用 `--tool-path` 选项。
* 若要更新本地工具，请使用 `--local` 选项。

**本地工具从 .NET Core SDK 3.0 开始可用。**

## <a name="arguments"></a>自变量

- **`PACKAGE_ID`**

  包含要更新的 .NET 全局工具的 NuGet 包的名称/ID。 你可以使用 [dotnet tool list](dotnet-tool-list.md) 命令查找包名称。

## <a name="options"></a>选项

- **`--add-source <SOURCE>`**

  添加安装过程中要使用的其他 NuGet 包源。

- **`--configfile <FILE>`**

  要使用的 NuGet 配置 (nuget.config) 文件。

- **`--disable-parallel`**

  防止并行还原多个项目。

- **`--framework <FRAMEWORK>`**

  指定要更新工具的[目标框架](../../standard/frameworks.md)。

- **`--ignore-failed-sources`**

  将包源失败视为警告。

- **`--interactive`**

  允许命令停止并等待用户输入或操作（例如，完成身份验证）。

- **`--local`**

  更新工具和本地工具清单。 不能与 `--global` 选项或 `--tool-path` 选项一起使用。

- **`--no-cache`**

  不要缓存包和 HTTP 请求。

- **`--tool-manifest <PATH>`**

  清单文件的路径。

- **`--tool-path <PATH>`**

  指定全局工具的安装位置。 路径可以是绝对的，也可以是相对的。 不能与 `--global` 选项一起使用。 省略 `--global` 和 `--tool-path` 均指定要更新的工具是本地工具。

- **`--version <VERSION>`**

  要将工具包更新到的版本范围。 这不能用于降级版本，必须首先 `uninstall` 较新的版本。

- **`-g|--global`**

  指定此更新用于用户范围的工具。 不能与 `--tool-path` 选项一起使用。 省略 `--global` 和 `--tool-path` 均指定要更新的工具是本地工具。

- **`-h|--help`**

  打印出有关命令的简短帮助。

- **`-v|--verbosity <LEVEL>`**

  设置命令的详细级别。 允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。

## <a name="examples"></a>示例

- **`dotnet tool update -g dotnetsay`**

  更新 [dotnetsay](https://www.nuget.org/packages/dotnetsay/) 全局工具。

- **`dotnet tool update dotnetsay --tool-path c:\global-tools`**

  更新位于特定 Windows 目录的 [dotnetsay](https://www.nuget.org/packages/dotnetsay/) 全局工具。

- **`dotnet tool update dotnetsay --tool-path ~/bin`**

  更新位于特定 Linux/macOS 目录的 [dotnetsay](https://www.nuget.org/packages/dotnetsay/) 全局工具。

- **`dotnet tool update dotnetsay`**

  更新为当前目录安装的 [dotnetsay](https://www.nuget.org/packages/dotnetsay/) 本地工具。

- **`dotnet tool update -g dotnetsay --version 2.0.*`**

  将 [dotnetsay](https://www.nuget.org/packages/dotnetsay/) 全局工具更新到最新的修补程序版本，主版本为 `2`，次要版本为 `0`。

- **`dotnet tool update -g dotnetsay --version (2.0.*,2.1.4)`**

  将 [dotnetsay](https://www.nuget.org/packages/dotnetsay/) 全局工具更新到指定范围 `(> 2.0.0 && < 2.1.4)` 内的最低版本，因此将安装版本 `2.1.0`。 有关语义化版本控制范围的详细信息，请参阅 [NuGet 打包版本范围](/nuget/concepts/package-versioning#version-ranges)。

## <a name="see-also"></a>请参阅

- [.NET 工具](global-tools.md)
- [语义化版本控制](https://semver.org)
- [教程：使用 .NET CLI 安装和使用 .NET 全局工具](global-tools-how-to-use.md)
- [教程：使用 .NET CLI 安装和使用 .NET 本地工具](local-tools-how-to-use.md)
