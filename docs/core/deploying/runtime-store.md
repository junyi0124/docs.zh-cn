---
title: 运行时包存储区
description: 了解如何使用 .NET Core 使用的运行时包存储以面向清单。
ms.date: 08/12/2017
ms.openlocfilehash: e9e27ef535dbd9e7197c323f7e49a9960aeff0f9
ms.sourcegitcommit: cbb19e56d48cf88375d35d0c27554d4722761e0d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/19/2020
ms.locfileid: "88608352"
---
# <a name="runtime-package-store"></a>运行时包存储区

自 .NET Core 2.0 起，可以根据目标环境中已知的一组包来打包和部署应用程序。 优点是部署速度更快、磁盘空间使用更少，并可以在某些情况下提升启动性能。

此功能实现为运行时包存储区  ，这是包在磁盘上的存储目录（通常情况下，在 macOS/Linux 上是 /usr/local/share/dotnet/store  ，在 Windows 上是 C:/Program Files/dotnet/store  ）。 此目录下有各个体系结构和[目标框架](../../standard/frameworks.md)的子目录。 文件布局类似于[磁盘上的 NuGet 资产布局](/nuget/create-packages/supporting-multiple-target-frameworks#framework-version-folder-structure)：

```
\dotnet
    \store
        \x64
            \netcoreapp2.0
                \microsoft.applicationinsights
                \microsoft.aspnetcore
                ...
        \x86
            \netcoreapp2.0
                \microsoft.applicationinsights
                \microsoft.aspnetcore
                ...
```

目标清单  文件列出了运行时包存储区中的包。 开发者可以在发布应用程序时以此清单为目标。 目标清单通常是由目标生产环境的所有者提供。

## <a name="preparing-a-runtime-environment"></a>准备运行时环境

运行时环境管理员可以生成运行时包存储区和相应的目标清单，从而优化应用程序，以加快部署速度并释放磁盘空间。

第一步是创建包存储区清单  ，用于列出运行时包存储区中的包。 此文件格式与项目文件格式 (csproj  ) 兼容。

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <ItemGroup>
    <PackageReference Include="NUGET_PACKAGE" Version="VERSION" />
    <!-- Include additional packages here -->
  </ItemGroup>
</Project>
```

**示例**

下面的示例包存储区清单 (packages.csproj  ) 用于将 [`Newtonsoft.Json`](https://www.nuget.org/packages/Newtonsoft.Json/) 和 [`Moq`](https://www.nuget.org/packages/moq/) 添加到运行时包存储区：

```xml
<Project Sdk="Microsoft.NET.Sdk">
  <ItemGroup>
    <PackageReference Include="Newtonsoft.Json" Version="10.0.3" />
    <PackageReference Include="Moq" Version="4.7.63" />
  </ItemGroup>
</Project>
```

通过执行 `dotnet store` 并指定包存储区清单、运行时和框架，预配运行时包存储区：

```dotnetcli
dotnet store --manifest <PATH_TO_MANIFEST_FILE> --runtime <RUNTIME_IDENTIFIER> --framework <FRAMEWORK>
```

**示例**

```dotnetcli
dotnet store --manifest packages.csproj --runtime win10-x64 --framework netcoreapp2.0 --framework-version 2.0.0
```

可以将多个目标包存储区清单路径传递到一个 [`dotnet store`](../tools/dotnet-store.md) 命令，具体方法是在此命令中重复指定选项和路径。

默认情况下，命令输出是在用户配置文件的 .dotnet/store  子目录下生成包存储区。 可以使用 `--output <OUTPUT_DIRECTORY>` 选项指定其他位置。 存储区的根目录包含目标清单 artifact.xml  文件。 此文件可供下载。如果应用程序创建者在发布时以这个存储区为目标，可以使用此文件。

**示例**

运行上一示例后生成以下 artifact.xml  文件。 请注意，由于 [`Castle.Core`](https://www.nuget.org/packages/Castle.Core/) 是 `Moq` 的依赖项，因此 artifacts.xml  清单文件会自动包含并显示它。

```xml
<StoreArtifacts>
  <Package Id="Newtonsoft.Json" Version="10.0.3" />
  <Package Id="Castle.Core" Version="4.1.0" />
  <Package Id="Moq" Version="4.7.63" />
</StoreArtifacts>
```

## <a name="publishing-an-app-against-a-target-manifest"></a>根据目标清单发布应用程序

如果磁盘上有目标清单文件，请在使用 [`dotnet publish`](../tools/dotnet-publish.md) 命令发布应用程序时指定此文件的路径：

```dotnetcli
dotnet publish --manifest <PATH_TO_MANIFEST_FILE>
```

**示例**

```dotnetcli
dotnet publish --manifest manifest.xml
```

将生成的已发布应用程序部署到包含目标清单中所述包的环境内。 如果不这样做，应用程序便无法启动。

发布应用程序时，通过重复指定选项和路径（例如，`--manifest manifest1.xml --manifest manifest2.xml`），可以指定多个目标清单。 这样，应用程序会进行剪裁，依据为提供给命令的目标清单文件中指定的包并集。

如果使用部署中的清单依赖项（程序集位于 bin  文件夹中）部署应用程序，运行时包存储区不会  在主机上用于相应程序集。 将使用 bin  文件夹程序集，无论它是否位于主机上的运行时包存储区中。

清单中指明的依赖项版本必须与运行时包存储区中的依赖项版本一致。 如果目标清单与运行时包存储区中的依赖项版本不一致，并且应用程序的部署中没有包的相应版本，那么应用程序将无法启动。 例外情况包括，为运行时包存储区程序集调用的目标清单名称，这有助于排查不一致问题。

如果在发布时  部署发生剪裁，只有指明的特定版本清单包，才不会出现在已发布的输出中。 主机上必须有指明的包版本，应用程序才能启动。

## <a name="specifying-target-manifests-in-the-project-file"></a>在项目文件中指定目标清单

除了使用 [`dotnet publish`](../tools/dotnet-publish.md) 命令指定目标清单，还可以在项目文件中将目标清单指定为 \<TargetManifestFiles> 标记下的路径分号分隔列表。

```xml
<PropertyGroup>
  <TargetManifestFiles>manifest1.xml;manifest2.xml</TargetManifestFiles>
</PropertyGroup>
```

仅在应用程序的目标环境已知的情况下（如 .NET Core 项目），才在项目文件中指定目标清单。 开放源代码项目的情况有所不同。 开放源代码项目的用户通常将项目部署到不同的生产环境。 这些生产环境通常都预安装了各组不同的包。 在这样的环境中，不能对目标清单作出假设，所以应使用 [`dotnet publish`](../tools/dotnet-publish.md) 的 `--manifest` 选项。

## <a name="aspnet-core-implicit-store-net-core-20-only"></a>ASP.NET Core 隐式存储区（仅限 .NET Core 2.0）

ASP.NET Core 隐式存储仅适用于 ASP.NET Core 2.0。 我们强烈建议应用程序使用 ASP.NET Core 2.1 及更高版本，这些版本不使用隐式存储  。 ASP.NET Core 2.1 及更高版本使用共享框架。

对于 .NET Core 2.0，当 ASP.NET Core 应用部署为[依赖框架的部署](index.md#publish-framework-dependent)应用时，该应用会隐式使用运行时包存储区功能。 [`Microsoft.NET.Sdk.Web`](https://github.com/aspnet/websdk) 中的目标包括引用目标系统上的隐式包存储区的清单。 另外，如果依赖框架的应用依赖 `Microsoft.AspNetCore.All` 包，则会生成仅包含应用及其资产的已发布应用，而不是 `Microsoft.AspNetCore.All` 元包中列出的包。 假定这些包都位于目标系统上。

安装 .NET Core SDK 后，便会在主机上安装运行时包存储区。 其他安装程序可能会提供运行时包存储区，包括 .NET Core SDK 的 Zip/tarball 安装、`apt-get`、Red Hat Yum、.NET Core Windows Server Hosting 捆绑包和手动运行时包存储区安装。

部署[从属框架部署](index.md#publish-framework-dependent)应用时，请确保目标环境中已安装 .NET Core SDK。 如果应用部署环境中未安装 ASP.NET Core，可以在项目文件中指定将 \<PublishWithAspNetCoreTargetManifest> 设置为 `false`，从而选择退出隐式存储区，如以下示例所示：

```xml
<PropertyGroup>
  <PublishWithAspNetCoreTargetManifest>false</PublishWithAspNetCoreTargetManifest>
</PropertyGroup>
```

> [!NOTE]
> 对于[独立部署](index.md#publish-self-contained)应用，假定目标系统不一定包含所需的清单包。 因此，对于自包含应用，不能将 \<PublishWithAspNetCoreTargetManifest> 设置为 `true`。

## <a name="see-also"></a>请参阅

- [dotnet-publish](../tools/dotnet-publish.md)
- [dotnet-store](../tools/dotnet-store.md)
