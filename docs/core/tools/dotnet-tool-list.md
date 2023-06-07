---
title: dotnet tool list 命令
description: dotnet tool list 命令列出计算机上安装的 .NET 工具。
ms.date: 02/14/2020
ms.openlocfilehash: d884f2c41834dd9704de3a8ca15417ba368fde4b
ms.sourcegitcommit: b201d177e01480a139622f3bf8facd367657a472
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2020
ms.locfileid: "94634280"
---
# <a name="dotnet-tool-list"></a>dotnet tool list

本文适用于： ✔️ .NET Core 2.1 SDK 及更高版本

## <a name="name"></a>“属性”

`dotnet tool list` - 列出计算机上当前安装的所有指定类型的 [.NET 工具](global-tools.md)。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet tool list -g|--global

dotnet tool list --tool-path <PATH>

dotnet tool list --local

dotnet tool list

dotnet tool list -h|--help
```

## <a name="description"></a>描述

`dotnet tool list` 命令提供一种方法，用于在计算机上安装所有 .NET 全局、工具路径或本地工具。 此命令列出包名称、安装的版本以及工具命令。  若要使用命令，请指定以下项之一：

* 若要列出在默认位置安装的全局工具，请使用 `--global` 选项
* 若要列出在自定义位置安装的全局工具，请使用 `--tool-path` 选项。
* 若要列出本地工具，请使用 `--local` 选项或省略 `--global`、`--tool-path` 和 `--local` 选项。

**本地工具从 .NET Core SDK 3.0 开始可用。**

## <a name="options"></a>选项

- **`-g|--global`**

  列出用户范围的全局工具。 不能与 `--tool-path` 选项一起使用。 省略 `--global` 和 `--tool-path` 将列出本地工具。

- **`-h|--help`**

  打印出有关命令的简短帮助。

- **`--local`**

  列出当前目录的本地工具。 不能与 `--global` 或 `--tool-path` 选项组合。 即使未指定 `--local`，同时省略 `--global` 和 `--tool-path` 也会列出本地工具。

- **`--tool-path <PATH>`**

  指定用于查找全局工具的自定义位置。 路径可以是绝对的，也可以是相对的。 不能与 `--global` 选项一起使用。 省略 `--global` 和 `--tool-path` 将列出本地工具。

## <a name="examples"></a>示例

- **`dotnet tool list -g`**

  列出计算机上安装的所有用户范围的全局工具（当前用户配置文件）。

- **`dotnet tool list --tool-path c:\global-tools`**

  列出特定 Windows 目录中的全局工具。

- **`dotnet tool list --tool-path ~/bin`**

  列出特定 Linux/macOS 目录中的全局工具。

- **`dotnet tool list`** 或 **`dotnet tool list --local`**

  列出当前目录中所有可用的本地工具。

## <a name="see-also"></a>请参阅

- [.NET 工具](global-tools.md)
- [教程：使用 .NET CLI 安装和使用 .NET 全局工具](global-tools-how-to-use.md)
- [教程：使用 .NET CLI 安装和使用 .NET 本地工具](local-tools-how-to-use.md)
