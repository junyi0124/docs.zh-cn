---
title: NuGet 和 .NET 库
description: 使用 .NET 库的 NuGet 打包的最佳实践建议。
ms.date: 01/15/2019
ms.openlocfilehash: d9f8d7cc4402a87e1429791b57a0306b318dfbe4
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/29/2020
ms.locfileid: "87382108"
---
# <a name="nuget"></a>NuGet

NuGet 是 .NET 生态系统的包管理器，并且是开发人员用来发现并获取 .NET 开放源代码库的主要方法。 [NuGet.org](https://www.nuget.org/)（由托管 NuGet 包的 Microsoft 提供的免费服务）是公共 NuGet 包的主要主机，但可以发布到自定义 NuGet 服务，如 [MyGet](https://www.myget.org/) 和 [Azure Artifacts](https://azure.microsoft.com/services/devops/artifacts/)。

![NuGet](./media/nuget/nuget-logo.png "NuGet")

## <a name="create-a-nuget-package"></a>创建 NuGet 包

NuGet 包 (`*.nupkg`) 是一个 zip 文件，其中包含 .NET 程序集和关联的元数据。

创建 NuGet 包有两种主要方式。 较新的推荐方式是从 SDK 样式项目（其内容以 `<Project Sdk="Microsoft.NET.Sdk">` 开头的项目文件）创建包。 程序集和目标会自动添加到包，剩余元数据会添加到 MSBuild 文件，如包名称和版本号。 使用 [`dotnet pack`](../../core/tools/dotnet-pack.md) 命令编译会输出 `*.nupkg` 文件，而不是程序集。

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <PropertyGroup>
    <TargetFramework>netstandard2.0</TargetFramework>
    <AssemblyName>Contoso.Api</AssemblyName>
    <PackageVersion>1.1.0</PackageVersion>
    <Authors>John Doe</Authors>
  </PropertyGroup>
</Project>
```

创建 NuGet 包的较旧方式是使用 `*.nuspec` 文件和 `nuget.exe` 命令行工具。 nuspec 文件为你提供更好的控制，但必须仔细指定要包含在最终 NuGet 包中的程序集和目标。 很容易犯错或很容易忘记在发生更改时更新 nuspec。 nuspec 的优点是可以将其用于创建尚不支持 SDK 样式项目文件的框架的 NuGet 包。

✔️ 请考虑使用 SDK 样式项目文件创建 NuGet 包。

## <a name="package-dependencies"></a>包依赖项

[依赖项](./dependencies.md)一文详细介绍了 NuGet 包依赖项。

## <a name="important-nuget-package-metadata"></a>重要的 NuGet 包元数据

NuGet 包支持多个[元数据属性](/nuget/reference/nuspec)。 下表包含 NuGet.org 上的每个包应提供的核心元数据：

| MSBuild 属性名称              | Nuspec 名称              | 描述  |
| ---------------------------------- | ------------------------ | ------------ |
| `PackageId`                        | `id`                       | 包标识符。 如果标识符的前缀满足[条件](/nuget/reference/id-prefix-reservation)，则可以保留该前缀。 |
| `PackageVersion`                   | `version`                  | NuGet 包版本。 有关详细信息，请参阅 [NuGet 包版本](./versioning.md#nuget-package-version)。             |
| `Title`                            | `title`                    | 明了易用的包标题。 默认为 `PackageId`。             |
| `Description`                      | `description`              | UI 中显示的包的详细说明。             |
| `Authors`                          | `authors`                  | 包创建者的逗号分隔列表，与 nuget.org 上的配置文件名称一致。             |
| `PackageTags`                      | `tags`                     | 描述包的标记和关键字的空格分隔列表。 搜索包时使用标记。             |
| `PackageIcon`                   | `icon`                  | 包中要用作包图标的图像的路径。 详细了解 [`icon` 元数据](/nuget/reference/nuspec#icon)。 |
| `PackageProjectUrl`                | `projectUrl`               | 项目主页或源存储库的 URL。             |
| `PackageLicenseExpression`         | `license`                  | 项目许可证的 [SPDX 标识符](https://spdx.org/licenses/)。 只有获得 OSI 和 FSF 批准的许可证才能使用标识符。 其他许可证应使用 `PackageLicenseFile`。 详细了解 [`license` 元数据](/nuget/reference/nuspec#license)。 |

> [!IMPORTANT]
> 无许可证的项目默认为 [exclusive copyright](https://choosealicense.com/no-permission/)（独占版权所有），从而无法供其他人使用。

✔️ 请考虑选择带有满足 NuGet 前缀预留[条件](/nuget/reference/id-prefix-reservation)的前缀的 NuGet 包名称。

✔️ 请使用指向包图标的 HTTPS href。

> 启用 HTTPS 运行并显示非 HTTPS 图像的 NuGet.org 等网站将创建混合内容警告。

✔️ 请使用属于 64x64 并具有透明背景的包图标图像以获得最佳查看结果。

✔️ 请考虑设置[源链接](./sourcelink.md)以将源代码管理元数据添加到程序集和 NuGet 包中。

> 源链接会自动将 `RepositoryUrl` 和 `RepositoryType` 元数据添加到 NuGet 包中。 源链接还会添加用于构建包的确切源代码的相关信息。 例如，从 Git 存储库创建的包将添加提交哈希作为元数据。

## <a name="pre-release-packages"></a>预发行包

具有版本后缀的 NuGet 包被视为[预发行版](/nuget/create-packages/prerelease-packages)。 默认情况下，NuGet 包管理器 UI 显示稳定版本，除非用户选择预发行包，使预发行包适用于受限的用户测试。

```xml
<PackageVersion>1.0.1-beta1</PackageVersion>
```

> [!NOTE]
> 稳定版包不能依赖于预发行包。 必须创建自己的预发行包或依赖于较旧的稳定版本。

![NuGet 预发布版本包依赖项](./media/nuget/nuget-prerelease-package.png "NuGet 预发布版本包依赖项")

✔️ 请在测试、预览或试用预发行包后进行发布。

✔️ 请在稳定版包就绪后进行发布，以便其他稳定版包可以引用它。

## <a name="symbol-packages"></a>符号包

符号文件 (`*.pdb`) 由 .NET 编译器与程序集一起生成。 符号文件将执行位置映射到原始源代码，以便可以逐行执行源代码（因为它使用调试程序运行）。 NuGet 支持[生成单独的符号包 (`*.snupkg`)](/nuget/create-packages/symbol-packages-snupkg)（包含符号文件）以及主包（包含 .NET 程序集）。 符号包的理念是它们托管在符号服务器上并仅由 Visual Studio 等工具按需下载。

NuGet.org 托管了自己的[符号服务器存储库](/nuget/create-packages/symbol-packages-snupkg#nugetorg-symbol-server)。 开发人员可以通过向其在 [Visual Studio 中的符号源](/visualstudio/debugger/specify-symbol-dot-pdb-and-source-files-in-the-visual-studio-debugger)添加 `https://symbols.nuget.org/download/symbols`，来使用发布到 NuGet.org 符号服务器的符号。

> [!IMPORTANT]
> NuGet.org 符号服务器仅支持由 SDK 样式项目创建的新的[可移植符号文件](https://github.com/dotnet/core/blob/master/Documentation/diagnostics/portable_pdb.md) (`*.pdb`)。
>
> 若要在调试 .NET 库时使用 NuGet.org 符号服务器，开发人员必须安装有 Visual Studio 2017 版本 15.9 或更高版本。

创建符号包的另一种方法是在主 NuGet 包中嵌入符号文件。 主 NuGet 包将变大，但嵌入的符号文件意味着开发人员不需要配置 NuGet.org 符号服务器。 如果使用 SDK 样式项目生成 NuGet 包，则可以通过设置 `AllowedOutputExtensionsInPackageBuildOutputFolder` 属性来嵌入符号文件：

```xml
<Project Sdk="Microsoft.NET.Sdk">
 <PropertyGroup>
    <!-- Include symbol files (*.pdb) in the built .nupkg -->
    <AllowedOutputExtensionsInPackageBuildOutputFolder>$(AllowedOutputExtensionsInPackageBuildOutputFolder);.pdb</AllowedOutputExtensionsInPackageBuildOutputFolder>
  </PropertyGroup>
</Project>
```

嵌入式符号文件的缺点是，对于使用 SDK 样式项目编译的 .NET 库，它们会将包的大小增加约 30%。 如果要考虑包大小，应改成在符号包中发布符号。

✔️ 请考虑将符号作为符号包 (`*.snupkg`) 发布到 NuGet.org

> 符号包 (`*.snupkg`) 为开发人员提供了良好的按需调试体验，而不会使主程序包大小膨胀，也不会影响那些不打算调试 NuGet 包的用户的还原性能。
>
> 需要注意的是，用户可能需要在其 IDE 中查找和配置 NuGet 符号服务器（作为一次性设置）来获取符号文件。 Visual Studio 2019 版本 16.1 将 NuGet.org 的符号服务器添加到了默认符号服务器列表中。

>[!div class="step-by-step"]
>[上一页](strong-naming.md)
>[下一页](dependencies.md)
