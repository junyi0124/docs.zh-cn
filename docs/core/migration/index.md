---
title: 从 project.json 迁移 .NET Core
description: 了解如何使用 project.json 迁移较旧的 .NET Core 项目
ms.date: 07/19/2017
ms.openlocfilehash: 73fbfed6943e3eb535e6eead3b3496edd3426c26
ms.sourcegitcommit: 7ef96827b161ef3fcde75f79d839885632e26ef1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/07/2021
ms.locfileid: "97970715"
---
# <a name="migrating-net-core-projects-from-projectjson"></a>从 project.json 迁移 .NET Core 项目

本文档介绍了以下三种适用于 .NET Core 项目的迁移方案：

1. [从 project.json 的一个最新有效架构迁移到 csproj](#migration-from-projectjson-to-csproj)
2. [从 DNX 迁移到 csproj](#migration-from-dnx-to-csproj)
3. [从 RC3 和以前的 .NET Core csproj 项目迁移到最终格式](#migration-from-earlier-net-core-csproj-formats-to-rtm-csproj)

本文档仅适用于使用 project.json 的较旧的 .NET Core 项目。 它不适用于从 .NET Framework 迁移到 .NET Core。

## <a name="migration-from-projectjson-to-csproj"></a>从 project.json 迁移到 csproj

可使用以下任一方法从 project.json 迁移到 .csproj：

- [Visual Studio](#visual-studio)
- [dotnet migrate 命令行工具](#dotnet-migrate)

这两种方法使用同一个基础引擎来迁移项目，因此，二者结果相同。 在大多数情况下，只需使用这两种方法中的一种将 project.json 迁移到 csproj，而无需进一步对项目文件执行手动编辑。 生成的 .csproj 文件的名称与包含目录名称相同。

### <a name="visual-studio"></a>Visual Studio

在 Visual Studio 2017 或 Visual Studio 2019 版本 16.2 及更早版本中打开 .xproj 文件或引用 .xproj 文件的解决方案文件时，将显示“单向升级”对话框。 该对话框将显示要迁移的项目。 如果打开解决方案文件，则将列出解决方案文件中指定的所有项目。 查看要迁移的项目的列表，然后选择“确定”。

![单向升级对话框，其中显示要迁移的项目的列表](media/one-way-upgrade.jpg)

Visual Studio 自动迁移所选的项目。 迁移解决方案时，如果不选择所有项目，则会显示相同的对话框，要求升级该解决方案的其余项目。 迁移项目后，可通过在“解决方案资源管理器”窗口中右键单击该项目，并选择“编辑 \<project name>.csproj”来查看和修改其内容 。

已迁移的文件（project.json、global.json、.xproj 和解决方案文件）会移动到“备份”文件夹。 迁移的解决方案文件会升级到 Visual Studio 2017 或 Visual Studio 2019，并且将无法在 Visual Studio 2015 或更早版本中打开该解决方案文件。 还会保存并自动打开名为 UpgradeLog.htm 的文件，该文件包含迁移报告。

> [!IMPORTANT]
> 在 Visual Studio 2019 版本 16.3 和更高版本中，无法加载或迁移 .xproj 文件。 此外，Visual Studio 2015 也不提供 .xproj 文件迁移的功能。 如果使用以上任一 Visual Studio 版本，请安装适当版本的 Visual Studio，或使用下述命令行迁移工具。

### <a name="dotnet-migrate"></a>dotnet migrate

在该命令行方案中，可以使用 [`dotnet migrate`](../tools/dotnet-migrate.md) 命令。 它会按顺序迁移项目、解决方案或一组文件夹，具体取决于所找到的项。 迁移项目时，将迁移项目及其所有依赖项。

已迁移的文件（project.json、global.json 和 .xproj）会移动到“备份”文件夹。

> [!NOTE]
> 如果使用 Visual Studio Code，则 `dotnet migrate` 命令不会修改 tasks.json 等 Visual Studio Code 专属文件。 需要手动更改这些文件。
> 如果使用 Visual Studio 以外的编辑器或集成开发环境 (IDE)，也是如此。

请参阅 [project.json 和 csproj 属性之间的映射](../tools/project-json-to-csproj.md)，了解 project.json 和 csproj 格式的比较情况。 

如果看到错误消息：

> 找不到匹配命令 dotnet-migrate 的可执行文件

请运行 `dotnet --version` 查看所使用的版本。 [`dotnet migrate`](../tools/dotnet-migrate.md) 在 .NET Core SDK 1.0.0 中引入，并在版本 3.0.100 中删除。
如果当前目录或父级目录中有 global.json 文件，且它指定的 `sdk` 版本在此范围外，则会收到此错误。

## <a name="migration-from-dnx-to-csproj"></a>从 DNX 迁移到 csproj

如果仍在使用 DNX 进行 .NET Core 开发，则应分两个阶段完成迁移过程：

1. 使用[现有 DNX 迁移指南](from-dnx.md)从 DNX 迁移到启用了 project-json 的 CLI。
2. 请按照上一部分中的步骤，从 project.json 迁移到 .csproj。

> [!NOTE]
> 已于 .NET Core CLI 的预览版 1 发布期间正式弃用 DNX。

## <a name="migration-from-earlier-net-core-csproj-formats-to-rtm-csproj"></a>从较早的 .NET Core csproj 格式迁移到 RTM csproj

随着工具的每个新的预发布版本的推出，.NET Core csproj 格式也在不断变化发展。 没有工具可以将项目文件从早期版本的 csproj 迁移到最新版本，因此需要手动编辑项目文件。 实际步骤取决于要迁移的项目文件的版本。 根据版本之间的变化，需考虑以下指导信息：

- 从 `<Project>` 元素中删除工具版本属性（如果存在）。
- 从 `<Project>` 元素中删除 XML 命名空间 (`xmlns`)。
- 如果不存在，请将 `Sdk` 属性添加到 `<Project>` 元素，并将其设置为 `Microsoft.NET.Sdk` 或 `Microsoft.NET.Sdk.Web`。 此属性指定项目使用要使用的 SDK。 `Microsoft.NET.Sdk.Web` 用于 Web 应用。
- 从项目的顶部和底部删除 `<Import Project="$(MSBuildExtensionsPath)\$(MSBuildToolsVersion)\Microsoft.Common.props" />` 和 `<Import Project="$(MSBuildToolsPath)\Microsoft.CSharp.targets" />` 语句。 SDK 隐含这些 import 语句，因此项目中不需要这些语句。
- 如果项目中含 `Microsoft.NETCore.App` 或 `NETStandard.Library``<PackageReference>` 项，应将其删除。 [SDK 隐含](../tools/csproj.md)这些包引用。
- 删除 `Microsoft.NET.Sdk` `<PackageReference>` 元素（如果存在）。 SDK 引用来自 `<Project>` 元素上的 `Sdk` 属性。
- 删除 [SDK](../project-sdk/overview.md#default-includes-and-excludes) 隐含的 [glob](https://en.wikipedia.org/wiki/Glob_(programming))。 在项目中留下这些 glob 会引发生成错误，因为编译项会发生重复。

完成这些步骤后，项目应与 RTM .NET Core csproj 格式完全兼容。

有关从旧的 csproj 格式迁移到新的 csproj 格式之前和之后情况的示例，请参阅 .NET 博客上的 [Updating Visual Studio 2017 RC – .NET Core Tooling improvements](https://devblogs.microsoft.com/dotnet/updating-visual-studio-2017-rc-net-core-tooling-improvements/)（更新 Visual Studio 2017 RC - .NET Core 工具改进）文章。

## <a name="see-also"></a>请参阅

- [移植、迁移和升级 Visual Studio 项目](/visualstudio/porting/port-migrate-and-upgrade-visual-studio-projects)
