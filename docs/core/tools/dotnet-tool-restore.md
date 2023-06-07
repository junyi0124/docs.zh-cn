---
title: dotnet tool restore 命令
description: dotnet tool restore 命令在计算机上安装当前目录范围内的 .NET 本地工具。
ms.date: 02/14/2020
ms.openlocfilehash: 3425bc6b78fd53f578c209013f83b006305dbb81
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96242924"
---
# <a name="dotnet-tool-restore"></a>dotnet tool restore

本文适用于： ✔️ .NET Core 3.0 SDK 及更高版本

## <a name="name"></a>“属性”

`dotnet tool restore` - 安装当前目录范围内的 .NET 本地工具。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet tool restore
    [--configfile <FILE>] [--add-source <SOURCE>]
    [tool-manifest <PATH_TO_MANIFEST_FILE>] [--disable-parallel]
    [--ignore-failed-sources] [--no-cache] [--interactive]
    [-v|--verbosity <LEVEL>]

dotnet tool restore -h|--help
```

## <a name="description"></a>描述

`dotnet tool restore` 命令查找当前目录范围内的工具清单文件，并安装其中列出的工具。 有关清单文件的信息，请参阅[安装本地工具](global-tools.md#install-a-local-tool)和[调用本地工具](global-tools.md#invoke-a-local-tool)。

## <a name="options"></a>选项

- **`--configfile <FILE>`**

  要使用的 NuGet 配置 (nuget.config) 文件。

- **`--add-source <SOURCE>`**

  添加安装过程中要使用的其他 NuGet 包源。

- **`--tool-manifest <PATH>`**

  清单文件的路径。

- **`--disable-parallel`**

  防止并行还原多个项目。

- **`--ignore-failed-sources`**

  将包源失败视为警告。

- **`--no-cache`**

  不要缓存包和 HTTP 请求。

- **`--interactive`**

  允许命令停止并等待用户输入或操作（例如，完成身份验证）。

- **`-h|--help`**

  打印出有关命令的简短帮助。

- **`-v|--verbosity <LEVEL>`**

  设置命令的详细级别。 允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。

## <a name="example"></a>示例

- **`dotnet tool restore`**

  还原当前目录的本地工具。

## <a name="see-also"></a>请参阅

- [.NET 工具](global-tools.md)
- [教程：使用 .NET CLI 安装和使用 .NET 本地工具](local-tools-how-to-use.md)
