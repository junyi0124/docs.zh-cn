---
title: dotnet build 命令
description: dotnet build 命令可生成项目及其所有依赖项。
ms.date: 02/14/2020
ms.openlocfilehash: ea0291129aeaed3bebef5c454ff003131bd3562b
ms.sourcegitcommit: b201d177e01480a139622f3bf8facd367657a472
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2020
ms.locfileid: "94634476"
---
# <a name="dotnet-build"></a>dotnet build

**本文适用于：** ✔️ .NET Core 2.x SDK 及更高版本

## <a name="name"></a>“属性”

`dotnet build` - 生成项目及其所有依赖项。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet build [<PROJECT>|<SOLUTION>] [-c|--configuration <CONFIGURATION>]
    [-f|--framework <FRAMEWORK>] [--force] [--interactive] [--no-dependencies]
    [--no-incremental] [--no-restore] [--nologo] [-o|--output <OUTPUT_DIRECTORY>]
    [-r|--runtime <RUNTIME_IDENTIFIER>] [--source <SOURCE>]
    [-v|--verbosity <LEVEL>] [--version-suffix <VERSION_SUFFIX>]

dotnet build -h|--help
```

## <a name="description"></a>描述

`dotnet build` 命令将项目及其依赖项生成为一组二进制文件。 二进制文件包括扩展名为 .dll 的中间语言 (IL) 文件中的项目代码。  根据项目类型和设置，可能会包含其他文件，例如：

- 可用于运行应用程序的可执行文件（如果项目类型是面向 .NET Core 3.0 或更高版本的可执行文件）。
- 用于调试的扩展名为 .pdb 的符号文件。
- 列出了应用程序或库的依赖项的 .deps.json 文件。
- 用于指定应用程序的共享运行时及其版本的 .runtimeconfig.json 文件。
- 项目通过项目引用或 NuGet 包引用所依赖的其他库。

对于目标版本低于 .NET Core 3.0 的可执行项目，通常不会将 NuGet 中的库依赖项复制到输出文件夹。  而是在运行时从 NuGet 全局包文件夹中对其进行解析。 考虑到这一点，`dotnet build` 的产品还未准备好转移到另一台计算机进行运行。 要创建可部署的应用程序版本，需要发布该应用程序（例如，使用 [dotnet publish](dotnet-publish.md) 命令）。 有关详细信息，请参阅 [.NET 应用程序部署](../deploying/index.md)。

对于面向 .NET Core 3.0 及更高版本的可执行项目，库依赖项会被复制到输出文件夹。 这意味着如果没有其他任何特定于发布的逻辑（例如，Web 项目具有的逻辑），则应可部署生成输出。

### <a name="implicit-restore"></a>隐式还原

构建需要 *project.assets.json* 文件，该文件列出了你的应用程序的依赖项。 此文件在 [`dotnet restore`](dotnet-restore.md) 执行时创建。 如果资产文件未就位，那么工具将无法解析引用程序集，进而导致错误生成。

[!INCLUDE[dotnet restore note + options](~/includes/dotnet-restore-note-options.md)]

### <a name="executable-or-library-output"></a>可执行文件或库输出

项目是否可执行由项目文件中的 `<OutputType>` 属性决定。 以下示例显示生成可执行代码的项目：

```xml
<PropertyGroup>
  <OutputType>Exe</OutputType>
</PropertyGroup>
```

要生成库，请省略 `<OutputType>` 属性或将其值更改为 `Library`。 库的 IL DLL 不包含入口点，因此无法执行。

### <a name="msbuild"></a>MSBuild

`dotnet build` 使用 MSBuild 生成项目，因此它支持并行生成和增量生成。 有关详细信息，请参阅[增量生成](/visualstudio/msbuild/incremental-builds)。

除其自己的选项外，`dotnet build` 命令也接受 MSBuild 选项，如用来设置属性的 `-p` 或用来定义记录器的 `-l`。 有关这些选项的详细信息，请参阅 [MSBuild 命令行参考](/visualstudio/msbuild/msbuild-command-line-reference)。 或者也可以使用 [dotnet msbuild](dotnet-msbuild.md) 命令。

运行 `dotnet build` 等同于运行 `dotnet msbuild -restore`；但是，输出的默认详细程度不同。

## <a name="arguments"></a>自变量

`PROJECT | SOLUTION`

要生成的项目或解决方案文件。 如果未指定项目或解决方案文件，MSBuild 会在当前工作目录中搜索文件扩展名以 *proj* 或 *sln* 结尾的文件并使用该文件。

## <a name="options"></a>选项

- **`-c|--configuration <CONFIGURATION>`**

  定义生成配置。 大多数项目的默认配置为 `Debug`，但你可以覆盖项目中的生成配置设置。

- **`-f|--framework <FRAMEWORK>`**

  编译特定[框架](../../standard/frameworks.md)。 必须在[项目文件](csproj.md)中定义该框架。

- **`--force`**

  强制解析所有依赖项，即使上次还原已成功，也不例外。 指定此标记等同于删除 project.assets.json 文件。

- **`-h|--help`**

  打印出有关命令的简短帮助。

- **`--interactive`**

  允许命令停止并等待用户输入或操作。 例如，完成身份验证。 自 .NET Core 3.0 SDK 起可用。

- **`--no-dependencies`**

  忽略项目到项目 (P2P) 引用，并仅生成指定的根项目。

- **`--no-incremental`**

  将生成标记为对增量生成不安全。 此标记关闭增量编译，并强制完全重新生成项目依赖项关系图。

- **`--no-restore`**

  在生成期间不执行隐式还原。

- **`--nologo`**

  不显示启动版权标志或版权消息。 自 .NET Core 3.0 SDK 起可用。

- **`-o|--output <OUTPUT_DIRECTORY>`**

  放置生成二进制文件的目录。 如果未指定，则默认路径为 `./bin/<configuration>/<framework>/`。  对于具有多个目标框架的项目（通过 `TargetFrameworks` 属性），在指定此选项时还需要定义 `--framework`。

- **`-r|--runtime <RUNTIME_IDENTIFIER>`**

  指定目标运行时。 有关运行时标识符 (RID) 的列表，请参阅 [RID 目录](../rid-catalog.md)。

- **`--source <SOURCE>`**

  要在还原操作期间使用的 NuGet 包源的 URI。

- **`-v|--verbosity <LEVEL>`**

  设置 MSBuild 详细级别。 允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。 默认值为 `minimal`。

- **`--version-suffix <VERSION_SUFFIX>`**

  设置生成项目时使用的 `$(VersionSuffix)` 属性的值。 这仅在未设置 `$(Version)` 属性时有效。 然后，`$(Version)` 设置为 `$(VersionPrefix)` 与 `$(VersionSuffix)` 组合，并用短划线分隔。

## <a name="examples"></a>示例

- 生成项目及其依赖项：

  ```dotnetcli
  dotnet build
  ```

- 使用“发布”配置生成项目及其依赖项：

  ```dotnetcli
  dotnet build --configuration Release
  ```

- 针对特定运行时（本例中为 Ubuntu 18.04）生成项目及其依赖项：

  ```dotnetcli
  dotnet build --runtime ubuntu.18.04-x64
  ```

- 生成项目，并在还原操作过程中使用指定的 NuGet 包源：

  ```dotnetcli
  dotnet build --source c:\packages\mypackages
  ```

- 生成项目并设置版本 1.2.3.4 作为使用 `-p` [MSBuild 选项](#msbuild)的生成参数：

  ```dotnetcli
  dotnet build -p:Version=1.2.3.4
  ```
