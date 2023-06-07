---
title: 从 DNX 迁移到 .NET Core CLI
description: 从使用 DNX 工具迁移到 .NET Core CLI 工具。
ms.date: 06/20/2016
ms.openlocfilehash: e5ebbab2551cf750a5b1136e7b1d4b67816c3b03
ms.sourcegitcommit: bf5c5850654187705bc94cc40ebfb62fe346ab02
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/23/2020
ms.locfileid: "91071113"
---
# <a name="migrating-from-dnx-to-net-core-cli-projectjson"></a>从 DNX 迁移到 .NET Core CLI (project.json)

## <a name="overview"></a>概述

.NET Core 和 ASP.NET Core 1.0 RC1 版本中推出了 DNX 工具。 .NET Core 和 ASP.NET Core 1.0 RC2 版本从 DNX 移动到了 .NET Core CLI。

温故知新，我们简单复习下 DNX 是什么。 DNX 是用于生成 .NET Core（更具体点，是用于生成 ASP.NET Core 1.0 应用程序）的运行时和工具集。 它主要由 3 个部分组成：

1. DNVM -- 用于获取 DNX 的安装脚本
2. DNX（Dotnet 执行运行时） - 执行代码的运行时
3. DNU（Dotnet 开发人员实用程序） - 用于管理依赖项、生成和发布应用程序的工具

引入 CLI 后，上述所有内容现在全部都属于单个工具集。 但是，由于 DNX 在 RC1 时间范围内还可用，你可能具有某些使用该 DNX 生成的项目并且想要将其移动到新的 CLI 工具。

本迁移指南就介绍了关于如何将项目从 DNX 迁移到 .NET Core CLI 的基础知识。 如果你才刚刚开始在 .NET Core 上从头创建项目，可选择跳过此文档。

## <a name="main-changes-in-the-tooling"></a>工具的主要更改

首先将概述工具中存在的一般性更改。

### <a name="no-more-dnvm"></a>不再具有 DNVM

DNVM（DotNet 版本管理器  的简称），是用于在计算机上安装 DNX 的 bash/PowerShell 脚本。 它有助于用户从指定的数据源（或默认数据源）获得需要的 DNX，以及将某个 DNX 标记为“活动”，从而将其置于给定会话的 $PATH 中。 这使你能够使用各种工具。

DNVM 现已停用，因为其功能集可能由于 .NET Core CLI 即将推出的更改变得冗余。

可以通过以下两种主要方式打包 CLI：

1. 给定平台的本机安装程序
2. 用于其他情形（如 CI 服务器）的安装脚本

因此，不再需要 DNVM 安装功能。 但运行时选择功能又如何呢？

将某个版本的包添加到依赖项，可以引用 `project.json` 中的运行时。 正因有此更改，应用程序将能够使用新的运行时数据。 将这些数据引入计算机与安装 CLI 方法一样：通过其支持的本机安装程序之一或通过其安装脚本安装运行时。

### <a name="different-commands"></a>命令不同

如果使用的是 DNX，则使用某些来自其三个部件（DNX、DNU 或 DNVM）之一的命令。 借助 CLI，其中的某些命令发生了改变，有些命令不再适用，有些保持不变，但语义稍有不同。

下表显示了 DNX/DNU 命令及其 CLI 对应项之间的映射。

| DNX 命令                    | CLI 命令    | 说明                                                                                                     |
|--------------------------------|----------------|-----------------------------------------------------------------------------------------------------------------|
| dnx 运行                        | `dotnet run`     | 从源运行代码。                                                                                           |
| dnu 生成                      | `dotnet build`   | 生成代码的 IL 二进制。                                                                                |
| dnu 包                       | `dotnet pack`    | 打包代码的 NuGet 包。                                                                        |
| dnx \[command]（例如，“dnx web”） | 不适用\*          | 在 DNX 领域中，按照 project.json 中的定义运行命令。                                                     |
| dnu 安装                    | 不适用\*          | 在 DNX 领域中，将包安装为一个依赖项。                                                            |
| dnu 还原                    | `dotnet restore` | 还原 project.json 中指定的依赖项。 （[请参阅注释](#dotnet-restore-note)）                                                            |
| dnu 发布                    | `dotnet publish` | 使用三种形式（可移植、本机可移植和独立形式）之一发布应用程序以进行部署。 |
| dnu 包装                       | 不适用\*          | 在 DNX 领域中，包装 csproj 中的 project.json。                                                                    |
| dnu 命令                   | 不适用\*          | 在 DNX 领域中，管理全局安装的命令。                                                           |

(\*) - 按照设计，CLI 中不支持这些功能。

## <a name="dnx-features-that-are-not-supported"></a>不支持 DNX 功能

如上表所示，CLI 中不支持（至少暂时不支持）来自 DNX 领域的某些功能。 本部分将介绍最重要的这部分内容，并概述不支持它们的根本原因，以及如果确实需要某些功能时相应的解决办法。

### <a name="global-commands"></a>全局命令

DNU 附带称为“全局命令”的概念。 从本质上来说，这些都是打包成 NuGet 包的控制台应用程序，其中 shell 脚本可以调用指定用于运行应用程序的 DNX。

CLI 不支持此概念。 但是，它确实支持添加所有项目命令的这一概念，这些命令可以使用熟悉的 `dotnet <command>` 语法调用。

### <a name="installing-dependencies"></a>安装依赖项

自 v1 起，.NET Core CLI 就没有用于安装依赖项的 `install` 命令。 为了从 NuGet 安装包，需要将其作为依赖项添加到 `project.json` 文件，然后运行 `dotnet restore`（[请参阅注释](#dotnet-restore-note)）。

### <a name="running-your-code"></a>运行代码

运行代码主要有两种方法。 一种是从源中使用 `dotnet run` 运行。 与 `dnx run` 不同，这种方法不能执行任何内存中编译。 实际上，它将调用 `dotnet build` 生成代码，然后运行生成的二进制文件。

另一种是使用 `dotnet` 自身运行代码。 可通过提供程序集路径实现：`dotnet path/to/an/assembly.dll`。

## <a name="migrating-your-dnx-project-to-net-core-cli"></a>将 DNX 项目迁移到 .NET Core CLI

使用代码时，除了使用新的命令，从 DNX 迁移还剩三件主要的事情：

1. 若要使其能够使用 CLI，请迁移 `global.json` 文件。
2. 将项目文件 (`project.json`) 本身迁移到 CLI 工具。
3. 将任何 DNX API 迁移到 BCL 对应项。

### <a name="changing-the-globaljson-file"></a>更改 global.json 文件

对于 RC1 和 RC2（或更高版本）项目，`global.json` 文件充当两者的解决方案文件。 为了在 RC1 和更高版本间区分 .NET Core CLI（以及 Visual Studio），可以使用 `"sdk": { "version" }` 属性来区分哪个项目是 RC1 或更高版本。 如果 `global.json` 根本无此节点，则假定为最新版本。

为了更新 `global.json` 文件，可以删除此属性或将其设置为想要使用的工具的确切版本，在本示例中为 **1.0.0-preview2-003121**：

```json
{
    "sdk": {
        "version": "1.0.0-preview2-003121"
    }
}
```

### <a name="migrating-the-project-file"></a>迁移项目文件

CLI 和 DNX 都使用基于 `project.json` 文件的相同基本项目系统。 项目文件的语法和语义大致相同，但根据应用场景，会略有不同。 此外还对架构进行了一些更改，可在[架构文件](http://json.schemastore.org/project)中查看这些更改。

如果要生成控制台应用程序，需要将以下代码段添加到项目文件：

```json
"buildOptions": {
    "emitEntryPoint": true
}
```

这会指示 `dotnet build` 发出应用程序入口点，可以有效地使代码具有可运行性。 如果要生成类库，则可以直接省略以上内容。 当然，将上述代码段添加到 `project.json` 文件后，需要添加静态入口点。 从 DNX 迁移后，它提供的 DI 服务将不再可用，因此需要属于基本 .NET 入口点：`static void Main()`。

如果 `project.json` 中有“命令”部分，可将其删除。 过去作为 DNU 命令（例如，实体框架 CLI 命令）存在的某些命令，将作为每个项目的扩展移植到 CLI。 如果生成了自己正在项目中使用的命令，需要使用 CLI 扩展将其替换。 在这种情况下，`project.json` 中的 `commands` 节点需要替换为 `tools` 节点，并且需要列出工具依赖项。

完成这些操作后，需要决定希望应用使用的可移植性类型。 借助 .NET Core，我们在提供一系列可从中进行选择的可移植性选项方面投入了大量工作。 例如，可能想要一个完全可移植  的应用程序或者想要一个独立的  应用程序。 可移植应用程序选项工作原理更像 .NET Framework 应用程序的工作原理：它需要共享组件才能在目标计算机 (.NET Core) 上执行。 独立应用程序不需要在目标上安装 .NET Core，但需要为每个要支持的 OS 生成一个应用程序。 有关这些可移植性类型等内容，请参阅[应用程序可移植性类型](../deploying/index.md)文档。

调用想要的可移植性类型后，需要更改目标框架。 如果是为 .NET Core 编写应用程序，很可能要使用 `dnxcore50` 作为目标框架。 鉴于 CLI 和全新 [.NET Standard](../../standard/net-standard.md) 引入的变化，框架需要为下列之一：

1. `netcoreapp1.0` - 如果要在 .NET Core 上编写应用程序（包括 ASP.NET Core 应用程序）
2. `netstandard1.6` - 如果要为 .NET Core 编写类库

如果要使用其他 `dnx` 目标，如 `dnx451`，则还需要更改这些内容。 `dnx451` 应更改为 `net451`。
有关详细信息，请参阅 [.NET Standard](../../standard/net-standard.md) 主题。

`project.json` 现已差不多准备就绪。 需要浏览依赖项列表并将依赖项更新至最新版本，尤其在使用 ASP.NET Core 依赖项时。 如果使用的是 BCL API 的单独包，可使用运行时包，如[应用程序可移植性类型](../deploying/index.md)文档中所述。

准备就绪后，就可以尝试使用 `dotnet restore`（[请参阅注释](#dotnet-restore-note)）进行还原。 具体取决于依赖项版本，如果 NuGet 无法解析上述目标框架的依赖项，可能会遇到错误。 这是一个“时间点”问题；随着时间的推移，支持这些框架的包会越来越多。 目前，如果遇到此类问题，可以使用 `framework` 节点内的 `imports` 语句指定到 NuGet，还原“imports”语句内面向该框架的包。
此种情况下遇到的还原错误可提供足够的信息，告知需要导入的框架类型。 一般而言，如果对此感到迷惑或不熟悉，在 `imports` 语句中指定 `dnxcore50` 和 `portable-net45+win8` 应该有效。 下面的 JSON 代码段显示了这种情况是怎样的：

```json
    "frameworks": {
        "netcoreapp1.0": {
            "imports": ["dnxcore50", "portable-net45+win8"]
        }
    }
```

运行 `dotnet build` 会显示任何最终的生成错误，但应该不会有很多。 生成代码并正常运行后，可以使用运行程序测试它。 执行 `dotnet <path-to-your-assembly>` 并查看其运行状况。

<a name="dotnet-restore-note"></a>

[!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]
