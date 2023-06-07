---
title: 版本控制和 .NET 库
description: 版本控制 .NET 库的最佳实践建议。
ms.date: 12/10/2018
ms.openlocfilehash: ab15d56e40abedd842b681496b9e5ee737c8b1cd
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290118"
---
# <a name="versioning"></a>版本管理

软件库在版本 1.0 中很少是完整的。 良好的库随时间而演变，例如，添加功能、修复缺陷和提高性能。 重要的是你可以发布新版本的 .NET 库，为每个版本提供附加值，而不会中断现有用户。

## <a name="breaking-changes"></a>重大更改

有关处理版本之间的重大更改的信息，请参阅[重大更改](./breaking-changes.md)。

## <a name="version-numbers"></a>版本号

.NET 库有多种方法用来指定版本。 以下版本是最重要的：

### <a name="nuget-package-version"></a>NuGet 包版本

使用 NuGet 包时，[NuGet 包版本](/nuget/reference/package-versioning)会显示在 NuGet.org（即 Visual Studio NuGet 包管理器）上，并添加到源代码。 NuGet 包版本是用户通常看到的版本号，并且会在用户谈论所用的库的版本时引用该包版本。 NuGet 包版本由 NuGet 使用，并且对运行时行为没有影响。

```xml
<PackageVersion>1.0.0-alpha1</PackageVersion>
```

与 NuGet 包版本结合使用的 NuGet 包标识符用于标识 NuGet 中的包。 例如，`Newtonsoft.Json` + `11.0.2`。 带有后缀的包是一个预发行包，其中的特殊行为非常适用于测试。 有关详细信息，请参阅[预发行包](./nuget.md#pre-release-packages)。

由于 NuGet 包版本是开发人员最易于可见的版本，因此最好使用[语义化版本控制 (SemVer)](https://semver.org/) 进行更新。 SemVer 指出版本之间的重大更改，并且可以帮助开发人员在选择要使用哪个版本时做出明智的决策。 例如，从 `1.0` 到 `2.0` 指示可能有重大更改。

✔️ 请考虑使用 [SemVer 2.0.0](https://semver.org/) 控制 NuGet 包的版本。

✔️ 请在公共文档中使用 NuGet 包版本，因为它是用户通常看到的版本号。

✔️ 请在发布非稳定版包时包含预发行后缀。

> 用户必须选择获取预发行包，以便了解包不是完整的。

### <a name="assembly-version"></a>程序集版本

程序集版本是 CLR 在运行时用来选择要加载哪个程序集版本的依据。 使用版本控制选择程序集仅适用于具有强名称的程序集。

```xml
<AssemblyVersion>1.0.0.0</AssemblyVersion>
```

.NET Framework CLR 要求完全匹配以便加载具有强名称的程序集。 例如，`Libary1, Version=1.0.0.0` 已使用对 `Newtonsoft.Json, Version=11.0.0.0` 的引用进行编译。 .NET Framework 将仅加载该确切版本 `11.0.0.0`。 若要在运行时加载其他版本，必须向 .NET 应用程序的配置文件添加绑定重定向。

与程序集版本组合使用的强命名启用[严格的程序集版本加载](../assembly/versioning.md)。 虽然强命名库具有很多好处，但通常会导致出现找不到程序集的运行时异常，并且在要修复的 `app.config` 或 `web.config` 中[需要绑定重定向](../../framework/configure-apps/redirect-assembly-versions.md)。 在 .NET Core 中，程序集加载更加宽松。 .NET Core 运行时在运行时自动加载版本较高的程序集。

✔️ 请考虑在 AssemblyVersion 中仅包括主版本。

> 例如，库 1.0 和库 1.0.1 都具有 AssemblyVersion `1.0.0.0`，而库 2.0 具有 AssemblyVersion `2.0.0.0`。 当程序集版本不常更改时，会减少绑定重定向。

✔️ 请考虑使 AssemblyVersion 的主版本号与 NuGet 包版本保持同步。

> AssemblyVersion 包含在向用户显示的一些信息性消息中，例如异常消息中的程序集名称和程序集限定类型名称。 维持版本之间的关系可向开发人员提供有关所用版本的详细信息。

❌ 不要有固定的 AssemblyVersion。

> 虽然不变的 AssemblyVersion 不需要绑定重定向，但意味着在全局程序集缓存 (GAC) 中只能安装单个程序集版本。 此外，引用 GAC 中的程序集的应用程序将中断，前提是另一个应用程序使用重大更改更新 GAC 程序集。

### <a name="assembly-file-version"></a>程序集文件版本

程序集文件版本用于在 Windows 中显示文件版本，并且对运行时行为没有影响。 设置此版本是可选的。 它在 Windows 资源管理器的“文件属性”对话框中可见：

```xml
<FileVersion>11.0.2.21924</FileVersion>
```

![Windows 资源管理器](./media/versioning/win-properties.png "Windows 资源管理器")

✔️ 请考虑包括持续集成内部版本号作为 AssemblyFileVersion 修订号。

> 例如，生成的项目版本为 1.0.0 且持续集成内部版本号为 99，则 AssemblyFileVersion 为 1.0.0.99。

✔️ 请对文件版本使用 `Major.Minor.Build.Revision` 格式。

> 虽然 .NET 从不使用此文件版本，但 [Windows 期望文件版本](/windows/desktop/menurc/versioninfo-resource)采用 `Major.Minor.Build.Revision` 格式。 如果版本不遵循此格式，则会引发警告。

### <a name="assembly-informational-version"></a>程序集信息性版本

程序集信息性版本用于记录附加版本信息，并且对运行时行为没有影响。 设置此版本是可选的。 如果使用的是源链接，则将在具有 NuGet 包版本以及源代码管理版本的内部版本上设置此版本。 例如，`1.0.0-beta1+204ff0a` 包括从中生成程序集的源代码的提交哈希。 有关详细信息，请参阅[源链接](./sourcelink.md)。

```xml
<AssemblyInformationalVersion>The quick brown fox jumped over the lazy dog.</AssemblyInformationalVersion>
```

> [!NOTE]
> 如果此版本不遵循格式 `Major.Minor.Build.Revision`，则较早版本的 Visual Studio 会引发生成警告。 可放心忽略此警告。

❌ 请避免自行设置程序集信息性版本。

> 允许 SourceLink 自动生成包含 NuGet 和源代码管理元数据的版本。

>[!div class="step-by-step"]
>[上一页](publish-nuget-package.md)
>[下一页](breaking-changes.md)
