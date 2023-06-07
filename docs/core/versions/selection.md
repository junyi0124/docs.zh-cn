---
title: 选择要使用的 .NET Core 版本
description: 了解 .NET Core 如何自动查找和选择适用于程序的运行时版本。 此外，本文还将介绍如何强制使用特定版本。
author: adegeo
ms.author: adegeo
ms.date: 03/24/2020
ms.openlocfilehash: 82b5522601b0ed5d3f4faf6e6c6c970ba285b11f
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608204"
---
# <a name="select-the-net-core-version-to-use"></a>选择要使用的 .NET Core 版本

本文介绍 .NET Core 工具、SDK 和运行时在选择版本时所使用的策略。 这些策略可通过使用指定版本，使正在运行的应用程序之间达到平衡，同时实现开发人员和最终用户计算机的轻松升级。 这些策略执行以下操作：

- 实现 .NET Core 的轻松高效部署，包括安全性和可靠性更新。
- 使用独立于目标运行时的最新工具和命令。

需要选择版本的情况如下：

- 运行 SDK 命令时，[SDK 使用最新安装的版本](#the-sdk-uses-the-latest-installed-version)。
- 生成程序集时，[目标框架名字对象定义生成时 API](#target-framework-monikers-define-build-time-apis)。
- 运行 .NET Core 应用程序时，[依赖于目标框架的应用程序将前滚](#framework-dependent-apps-roll-forward)。
- 发布独立应用程序时，[独立部署包括所选的运行时](#self-contained-deployments-include-the-selected-runtime)。

本文档其余部分将介绍这四种方案。

## <a name="the-sdk-uses-the-latest-installed-version"></a>SDK 使用最新安装的版本

SDK 命令包括 `dotnet new` 和 `dotnet run`。 .NET Core CLI 必须为每个 `dotnet` 命令选择 SDK 版本。 即使在以下情况下，它也会默认使用计算机上安装的最新 SDK：

- 项目面向早期 .NET Core 运行时版本。
- .NET Core SDK 的最新版本是预览版本。

面向较旧的 .NET Core 运行时版本时，可利用最新的 SDK 功能和功能改进。 可在不同项目上面向 .NET Core 的多个运行时版本，同时对所有项目使用相同的 SDK 工具。

在少数情况下，可能需要使用版本较旧的 SDK。 在 [global.json 文件](../tools/global-json.md)中指定该版本。 “使用最新”策略表示仅使用 global.json 指定早于最新安装版本的一个 .NET Core SDK 版本。

可将 global.json 放置在文件层次结构中的任何位置。 CLI 从项目目录中向上搜索其找到的第一个 global.json。 由用户控制对哪些项目应用给定的 global.json（按其在文件系统中的位置）。 .NET CLI 从当前工作目录路径向上导航，以迭代方式搜索 global.json 文件。 找到的第一个 global.json 文件指定要使用的版本。 如果已安装该 SDK 版本，则使用该版本。 如果找不到 global.json 中指定的 SDK，则 .NET CLI 将使用[匹配规则](../tools/global-json.md#matching-rules)来选择兼容的 SDK，如果找不到，则会失败。

下面的示例演示 global.json 语法：

``` json
{
  "sdk": {
    "version": "3.0.0"
  }
}
```

选择 SDK 版本的过程如下：

1. `dotnet` 从当前工作目录向下导航路径，以迭代方式搜索 global.json 文件。
1. `dotnet` 使用所找到的第一个 global.json 中指定的 SDK。
1. 如果未找到 global.json，`dotnet` 使用最新安装的 SDK。

有关选择 SDK 版本的详细信息，可参阅 global.json 相关文章的[匹配规则](../tools/global-json.md#matching-rules)部分。

## <a name="target-framework-monikers-define-build-time-apis"></a>目标框架名字对象用于定义生成时 API

针对在“目标框架名字对象”(TFM) 中定义的 API 构建项目。 在项目文件中指定[目标框架](../../standard/frameworks.md)。 按如下示例所示，设置项目文件中的 `TargetFramework` 元素：

``` xml
<TargetFramework>netcoreapp3.0</TargetFramework>
```

可能会针对多个 TFM 构建项目。 对库设置多个目标框架更为常见，但也可对应用程序执行此操作。 指定 `TargetFrameworks` 属性（`TargetFramework` 的复数形式）。 目标框架以分号分隔，如下例所示：

``` xml
<TargetFrameworks>netcoreapp3.0;net47</TargetFrameworks>
```

给定的 SDK 支持固定的一组框架，其中的上限框架为 SDK 附带的运行时的目标框架。 例如，.NET Core 3.0 SDK 包含 .NET Core 3.0 运行时，该运行时是 `netcoreapp3.0` 目标框架的一个实现。 .NET Core 3.0 SDK 支持 `netcoreapp2.1`、`netcoreapp2.2` 和 `netcoreapp3.0`，但不支持 `netcoreapp3.1`（或更高版本）。 安装 .NET Core 3.1 SDK 以针对 `netcoreapp3.1` 进行构建。

.NET Standard 目标框架中的上限框架同样是 SDK 附带的运行时的目标框架。 .NET Core 3.1 SDK 的上限为 `netstandard2.1`。 有关详细信息，请参阅 [.NET Standard](../../standard/net-standard.md)。

## <a name="framework-dependent-apps-roll-forward"></a>依赖于框架的应用会前滚

在使用 [`dotnet run`](../tools/dotnet-run.md) 从源运行应用程序时，在使用 [`dotnet myapp.dll`](../tools/dotnet.md#description) 从[框架相关部署](../deploying/index.md#publish-framework-dependent)运行应用程序时，或使用 `myapp.exe` 从[框架相关可执行文件](../deploying/index.md#publish-framework-dependent)运行应用程序时，`dotnet` 可执行文件是应用程序的主机。

该主机选择计算机上安装的最新修补程序版本。 例如，如果在项目文件中指定 `netcoreapp3.0`，并且 `3.0.2` 是安装的最新 .NET 运行时，则使用 `3.0.2` 运行时。

如果未找到可接受的 `3.0.*` 版本，则使用新的 `3.*` 版本。 例如，如果指定了 `netcoreapp3.0` 并且仅安装了 `3.1.0`，则应用程序在运行时使用 `3.1.0` 运行时。 此行为称为“次要版本前滚”。 此外，不会考虑较低版本。 如果未安装可接受的运行时，应用程序将不会运行。

如果你面向版本 3.0，则下面几个使用示例展示了此行为：

- ✔️ 指定 3.0。 3.0.3 是安装的最高修补程序版本。 使用 3.0.3。
- ❌ 指定 3.0。 未安装 3.0.* 版本。 2.1.1 是安装的最高运行时版本。 会显示一条错误消息。
- ✔️ 指定 3.0。 未安装 3.0.* 版本。 3.1.0 是安装的最高运行时版本。 使用 3.1.0。
- ❌ 指定 2.0。 未安装 2.x 版本。 3.0.0 是安装的最高运行时版本。 会显示一条错误消息。

次要版本回滚会产生一个可能影响最终用户的副作用。 请参考以下方案：

1. 应用程序指定需要版本 3.0。
2. 运行时，未安装 3.0.*，安装的是 3.1.0。 将使用版本 3.1.0。
3. 稍后，用户重新安装 3.0.3 和运行应用程序，而现将使用版本 3.0.3。

3\.0.3 和 3.1.0 可能具有不同行为，序列化二进制数据等方案中尤其如此。

## <a name="self-contained-deployments-include-the-selected-runtime"></a>独立部署包括所选的运行时

可以将应用程序作为[独立分发](../deploying/index.md#publish-self-contained)进行发布。 此方法可将 .NET Core 运行时和库与应用程序进行捆绑。 独立部署不具有对运行时环境的依赖关系。 在发布时（而不是运行时）选择运行时版本。

发布进程将选择给定运行时系列中的最新修补程序版本。 例如，`dotnet publish` 将选择 .NET Core 3.0.3（如果它是 .NET Core 3.0 运行时系列中的最新修补程序版本）。 目标框架（包括最新安装的安全修补程序）与应用程序捆绑打包。

如果为应用程序指定的最新版本不满足要求，会出现错误。 `dotnet publish` 绑定到最新的运行时修补程序版本（在给定的主要及次要版本系列内）。 `dotnet publish` 不支持 `dotnet run` 的前滚语义。 有关修补程序和独立部署的详细信息，请参阅有关 .NET Core 应用程序部署中[运行时修补程序选择](../deploying/runtime-patch-selection.md)的文章。

独立部署可能需要特定修补程序版本。 可以重写项目文件中的最低运行时修补程序版本（重写为更高或更低版本），如下例所示：

``` xml
<RuntimeFrameworkVersion>3.0.3</RuntimeFrameworkVersion>
```

`RuntimeFrameworkVersion` 元素重写默认版本策略。 对于独立部署，`RuntimeFrameworkVersion` 指定确切的运行时框架版本。 对于依赖于框架的应用程序，`RuntimeFrameworkVersion` 指定所需的最低运行时框架版本。

## <a name="see-also"></a>请参阅

- [下载和安装 .NET Core](../install/index.yml)。
- [如何删除 .NET Core 运行时和 SDK](../install/remove-runtime-sdk-versions.md)。
