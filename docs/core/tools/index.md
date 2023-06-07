---
title: .NET CLI
titleSuffix: ''
description: .NET CLI 及其功能概述。
ms.topic: overview
ms.date: 02/13/2020
ms.openlocfilehash: 6a12e2d16afe36092c10e14a7465fa3bdbb23f32
ms.sourcegitcommit: b201d177e01480a139622f3bf8facd367657a472
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2020
ms.locfileid: "94633851"
---
# <a name="net-cli-overview"></a>.NET CLI 概述

本文适用于： ✔️ .NET Core 2.1 SDK 及更高版本

.NET 命令行接口 (CLI) 工具是用于开发、生成、运行和发布 .NET 应用程序的跨平台工具链。

.NET CLI 附带了 [.NET SDK](../sdk.md)。 若要了解如何安装 .NET SDK，请参阅[安装 .NET Core](../install/windows.md)。

## <a name="cli-commands"></a>CLI 命令

默认安装以下命令：

### <a name="basic-commands"></a>基本命令

- [`new`](dotnet-new.md)
- [`restore`](dotnet-restore.md)
- [`build`](dotnet-build.md)
- [`publish`](dotnet-publish.md)
- [`run`](dotnet-run.md)
- [`test`](dotnet-test.md)
- [`vstest`](dotnet-vstest.md)
- [`pack`](dotnet-pack.md)
- [`migrate`](dotnet-migrate.md)
- [`clean`](dotnet-clean.md)
- [`sln`](dotnet-sln.md)
- [`help`](dotnet-help.md)
- [`store`](dotnet-store.md)

### <a name="project-modification-commands"></a>项目修改命令

- [`add package`](dotnet-add-package.md)
- [`add reference`](dotnet-add-reference.md)
- [`remove package`](dotnet-remove-package.md)
- [`remove reference`](dotnet-remove-reference.md)
- [`list reference`](dotnet-list-reference.md)

### <a name="advanced-commands"></a>高级命令

- [`nuget delete`](dotnet-nuget-delete.md)
- [`nuget locals`](dotnet-nuget-locals.md)
- [`nuget push`](dotnet-nuget-push.md)
- [`msbuild`](dotnet-msbuild.md)
- [`dotnet install script`](dotnet-install-script.md)

### <a name="tool-management-commands"></a>工具管理命令

- [`tool install`](dotnet-tool-install.md)
- [`tool list`](dotnet-tool-list.md)
- [`tool update`](dotnet-tool-update.md)
- [`tool restore`](global-tools.md#install-a-local-tool) 自 .NET Core SDK 3.0 起可用。
- [`tool run`](global-tools.md#invoke-a-local-tool) 自 .NET Core SDK 3.0 起可用。
- [`tool uninstall`](dotnet-tool-uninstall.md)

工具是控制台应用程序，它们从 NuGet 包中安装并从命令提示符处进行调用。 你可自行编写工具，也可安装由第三方编写的工具。 工具也称为全局工具、工具路径工具和本地工具。 有关详细信息，请参阅 [.NET 工具概述](global-tools.md)。

## <a name="command-structure"></a>命令结构

CLI 命令结构包含[驱动程序（“dotnet”）](#driver)和[命令](#command)，还可能包含命令[参数](#arguments)和[选项](#options)。 在大部分 CLI 操作中可看到此模式，例如创建新控制台应用并从命令行运行该应用，因为从名为 *my_app* 的目录中执行时，显示以下命令：

```dotnetcli
dotnet new console
dotnet build --output /build_output
dotnet /build_output/my_app.dll
```

### <a name="driver"></a>驱动程序

驱动程序名为 [dotnet](dotnet.md)，并具有两项职责，即运行[依赖于框架的应用](../deploying/index.md)或执行命令。

若要运行依赖于框架的应用，请在驱动程序后指定应用，例如，`dotnet /path/to/my_app.dll`。 从应用的 DLL 驻留的文件夹执行命令时，只需执行 `dotnet my_app.dll` 即可。 如果要使用特定版本的 .NET 运行时，请使用 `--fx-version <VERSION>` 选项（请参阅 [dotnet 命令](dotnet.md)参考）。

为驱动程序提供命令时，`dotnet.exe` 启动 CLI 命令执行过程。 例如：

```dotnetcli
dotnet build
```

首先，驱动程序确定要使用的 SDK 版本。 如果没有 [global.json](global-json.md) 文件，则使用可用的最新版本 SDK。 这有可能是预览版或稳定版，具体取决于计算机上的最新版本。  确定 SDK 版本后，它便会执行命令。

### <a name="command"></a>命令

由命令执行操作。 例如，`dotnet build` 生成代码。 `dotnet publish` 发布代码。 使用 `dotnet {command}` 约定将命令作为控制台应用程序实现。

### <a name="arguments"></a>自变量

在命令行上传递的参数是被调用的命令的参数。 例如，执行 `dotnet publish my_app.csproj` 时，`my_app.csproj` 参数指示要发布的项目，并被传递到 `publish` 命令。

### <a name="options"></a>选项

在命令行上传递的选项是被调用的命令的选项。 例如，执行 `dotnet publish --output /build_output` 时，`--output` 选项及其值被传递到 `publish` 命令。

## <a name="see-also"></a>请参阅

- [dotnet/sdk GitHub 存储库](https://github.com/dotnet/sdk/)
- [.NET 安装指南](../install/windows.md)
