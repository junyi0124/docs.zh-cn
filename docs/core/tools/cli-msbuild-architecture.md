---
title: .NET Core 命令行工具体系结构
description: 了解 .NET Core 工具层及最新版本中的更改。
ms.date: 03/06/2017
ms.openlocfilehash: e1a9fe59225c17d54f6e7213d2b3c3fa70ee58e0
ms.sourcegitcommit: 73aa9653547a1cd70ee6586221f79cc29b588ebd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2020
ms.locfileid: "82102874"
---
# <a name="high-level-overview-of-changes-in-the-net-core-tools"></a>.NET Core 工具中变更的高级概述

此文档介绍了与从 project.json  移动到 MSBuild 和 csproj  项目系统有关的更改，其中包含关于 .NET Core 工具的分层和 CLI 命令的实现的更改的信息。 这些更改与 2017 年 3 月 7 日 .NET Core SDK 1.0 和 Visual Studio 2017 的发布同时发生（请参阅[通知](https://devblogs.microsoft.com/dotnet/announcing-net-core-tools-1-0/)），但是最初是在 .NET Core SDK 预览 3 版本中实现的。

## <a name="moving-away-from-projectjson"></a>弃用 project.json

.NET Core 工具的最大变更无疑是[弃用 project.json，改用 csproj](https://devblogs.microsoft.com/dotnet/changes-to-project-json/) 作为项目系统。 最新版本的命令行工具不支持 *project.json* 文件。 这意味着它不能用于生成、运行或发布基于 project.json 的应用程序和库。 若要使用此版本的工具，请迁移现有项目或启动新的项目。

作为此次移动的一部分，开发用于生成 project.json 项目的自定义生成引擎被替换为一个功能完整的成熟生成引擎，即 [MSBuild](https://github.com/Microsoft/msbuild)。 MSBuild 是 .NET 社区中的知名引擎。 自从在平台上首次发布，就一直是一项关键技术。 由于需要生成 .NET Core 应用程序，所以 MSBuild 已经移植到 .NET Core，并可在 .NET Core 运行的任何平台上使用。 .NET Core 的一个主要的好处是跨平台开发堆栈，我们已确保本次迁移不会破坏此好处。

> [!TIP]
> 如果还不熟悉 MSBuild，并且想要了解相关详细信息，可参阅 [MSBuild 概念](/visualstudio/msbuild/msbuild-concepts)一文。

## <a name="the-tooling-layers"></a>工具层

随着生成引擎发生变化，并从现有项目系统中迁移出来，一些问题也会随之而来。 这些更改是否会导致 .NET Core 工具生态系统的整体“分层”发生更改？ 是否有新的位和组件？

首先来简要介绍下预览版 2 分层，如下图所示：

![预览版 2 工具高级体系结构](media/cli-msbuild-architecture/p2-arch.png)

预览版 2 中工具的分层较为简单。 底层基底是 .NET Core CLI。 其他所有高级工具（如 Visual Studio 或 Visual Studio Code）依靠 CLI 来生成项目、还原依赖项以及完成其他操作。 例如，如果 Visual Studio 要执行还原操作，它会在 CLI 中调入 `dotnet restore` 命令。

随着迁移到新的项目系统，之前的图表会更改：

![.NET Core SDK 1.0.0 高级体系结构](media/cli-msbuild-architecture/p3-arch.png)

主要区别在于 CLI 不再作为基础层；现由“共享 SDK 组件”充当此角色。 共享 SDK 组件是一组负责编译代码、发布代码、打包 NuGet 包等操作的目标和关联任务。 .NET Core SDK 是一个开源代码，可在 GitHub 上的 [SDK 存储库](https://github.com/dotnet/sdk)中获得。

> [!NOTE]
> “目标”是一个 MSBuild 术语，指示 MSBuild 可调用的一个已命名的操作。 其通常伴随着执行此目标应执行的某个逻辑的一个或多个任务。 MSBuild 支持多个现成的目标，如 `Copy` 或 `Execute`；它还允许用户使用托管代码编写自己的任务，并定义要执行这些任务的目标。 有关详细信息，请参阅 [MSBuild 任务](/visualstudio/msbuild/msbuild-tasks)。

现在所有工具集使用共享 SDK 组件及其目标，包括 CLI。 例如，Visual Studio 2019 不会调入 `dotnet restore` 命令来还原 .NET Core 项目的依赖项。 相反，它会直接使用“还原”目标。 由于这些皆是 MSBuild 目标，因此你也可通过 [dotnet msbuild](dotnet-msbuild.md) 命令使用原始 MSBuild 来执行。

### <a name="cli-commands"></a>CLI 命令

共享 SDK 组件意味着大部分现有 CLI 命令已重新实现为 MSBuild 任务和目标。 这对 CLI 命令和工具集的使用意味着什么？

从使用角度而言，它不会更改使用 CLI 的方式。 CLI 仍具有 .NET Core 1.0 预览 2 版本中存在的核心命令：

- `new`
- `restore`
- `run`
- `build`
- `publish`
- `test`
- `pack`

这些命令的作用没有发生改变（仍可新建项目、生成项目、发布项目、打包项目等等）。 可以参阅命令的帮助屏幕（使用 `dotnet <command> --help`）或此站点上的文档，来熟悉其行为。

从执行角度而言，CLI 命令会采用其参数并构造对“原始”MSBuild 的调用，从而设置所需属性和运行所需目标。 为更好的说明这点，请参考下面的命令：

   ```dotnetcli
   dotnet publish -o pub -c Release
   ```

此命令会使用“发布”配置将应用程序发布到 `pub` 文件夹。 在内部，此命令会转换成下面的 MSBuild 调用：

   ```dotnetcli
   dotnet msbuild -t:Publish -p:OutputPath=pub -p:Configuration=Release
   ```

此规则的两个明显的例外情况是 `new` 和 `run` 命令。 它们未实现为 MSBuild 目标。

### <a name="implicit-restore"></a>隐式还原

[!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]
