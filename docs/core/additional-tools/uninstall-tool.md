---
title: 卸载工具
description: .NET Core 卸载工具概述，它是一种可实现 .NET Core SDK 和运行时的受控清理的引导式工具。
author: sfoslund
ms.date: 05/27/2020
ms.openlocfilehash: dcfa12a3ec5fe0e8a29c5897ee4c71bfc7352eda
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 06/09/2020
ms.locfileid: "84590794"
---
# <a name="net-core-uninstall-tool"></a>.NET Core 卸载工具

[.NET Core 卸载工具](https://aka.ms/dotnet-core-uninstall-tool) (`dotnet-core-uninstall`) 使你可以从系统中删除 .NET Core SDK 和运行时。 可使用选项集合来指定要卸载的版本。

该工具支持 Windows 和 macOS。 目前不支持 Linux。

在 Windows 上，该工具只能卸载使用以下安装程序之一安装的 SDK 和运行时：

- .NET Core SDK 和运行时安装程序。
- Visual studio 安装程序的版本早于 Visual Studio 2019 版本 16.3。

在 macOS 上，该工具只能卸载位于 /usr/local/share/dotnet 文件夹中的 SDK 和运行时。

由于这些限制，该工具可能无法卸载计算机上的所有 .NET Core SDK 和运行时。 可以使用 `dotnet --info` 命令来查找所有安装的 .NET Core SDK 和运行时，包括此工具无法删除的 SDK 和运行时。 `dotnet-core-uninstall list` 命令显示可以通过该工具卸载的 SDK。

## <a name="install-the-tool"></a>安装工具

可以从[工具的发布页面](https://aka.ms/dotnet-core-uninstall-tool)下载 .NET Core 卸载工具，然后在 [dotnet/cli-lab](https://github.com/dotnet/cli-lab) GitHub 存储库中找到源代码。

> [!NOTE]
> 此工具需要提升才能卸载 .NET Core SDK 和运行时。 因此，应将其安装在写入保护的目录中，如 Windows 上的 C:\Program Files 或 macOS 上的 /usr/local/bin。 另请参阅[提升的 Dotnet 命令访问权限](../tools/elevated-access.md)。 有关详细信息，请参阅[详细安装说明](https://aka.ms/dotnet-core-uninstall-tool)。

## <a name="run-the-tool"></a>运行该工具

以下步骤说明了运行卸载工具的建议方法：

- [步骤 1 - 显示已安装的 .NET Core Sdk 和运行时](#step-1---display-installed-net-core-sdks-and-runtimes)
- [步骤 2 - 执行试运行](#step-2---do-a-dry-run)
- [步骤 3 - 卸载 .NET Core SDK 和运行时](#step-3---uninstall-net-core-sdks-and-runtimes)
- [步骤 4 - 删除 NuGet 回退文件夹（可选）](#step-4---delete-the-nuget-fallback-folder-optional)

### <a name="step-1---display-installed-net-core-sdks-and-runtimes"></a>步骤 1 - 显示安装的 .NET Core SDK 和运行时

`dotnet-core-uninstall list` 命令列出了已安装的 .NET Core SDK 和运行时，可以通过此工具将其删除。 Visual Studio 可能需要某些 SDK 和运行时，它们将显示出来，并说明为何不建议将其卸载。

> [!NOTE]
> 在大多数情况下，`dotnet-core-uninstall list` 命令的输出将与 `dotnet --info` 输出中的已安装版本列表不匹配。 具体而言，此工具将不会显示通过 zip 文件安装的版本，也不会显示由 Visual Studio（Visual Studio 2019 16.3 或更高版本）托管的版本。 检查某个版本是否由 Visual Studio 托管的一种方法是在 `Add or Remove Programs` 中查看该版本，由 Visual Studio 托管的版本在显示名称中会以这种方式标记。

**dotnet-core-uninstall list**

#### <a name="synopsis"></a>摘要

```console
dotnet-core-uninstall list [options]
```

#### <a name="options"></a>选项

## <a name="windows"></a>[Windows](#tab/windows)

* **`--aspnet-runtime`**

  列出可通过此工具卸载的所有 ASP.NET Core 运行时。

* **`--hosting-bundle`**

  列出可通过此工具卸载的所有 .NET Core 托管捆绑包。

* **`--runtime`**

  列出可通过此工具卸载的所有 .NET Core 运行时。

* **`--sdk`**

  列出可通过此工具卸载的所有 .NET Core SDK。

* **`-v, --verbosity <LEVEL>`**

  设置详细程度。 允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。 默认值为 `normal`。

* **`--x64`**

  列出可通过此工具卸载的所有 x64 .NET Core SDK 和运行时。

* **`--x86`**

  列出可通过此工具卸载的所有 x86 .NET Core SDK 和运行时。

## <a name="macos"></a>[macOS](#tab/macos)

* **`--runtime`**

  列出可通过此工具卸载的所有 .NET Core 运行时。

* **`--sdk`**

  列出可通过此工具卸载的所有 .NET Core SDK。

* **`-v, --verbosity <LEVEL>`**

  设置详细程度。 允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。 默认值为 `normal`。
  
---

#### <a name="examples"></a>示例

* 列出可通过此工具删除的所有 .NET Core SDK 和运行时：

  ```console
  dotnet-core-uninstall list
  ```

* 列出所有 x64 .NET Core SDK 和运行时：

  ```console
  dotnet-core-uninstall list --x64
  ```

* 列出所有 x86 .NET Core SDK：

  ```console
  dotnet-core-uninstall list --sdk --x86
  ```

### <a name="step-2---do-a-dry-run"></a>步骤 2 - 执行试运行

`dotnet-core-uninstall dry-run` 和 `dotnet-core-uninstall whatif` 命令显示将根据提供的选项删除的 .NET Core SDK 和运行时，而无需执行卸载。 这些命令是同义词。

**dotnet-core-uninstall dry-run 和 dotnet-core-uninstall whatif**

#### <a name="synopsis"></a>摘要

```console
dotnet-core-uninstall dry-run [options] [<VERSION>...]

dotnet-core-uninstall whatif [options] [<VERSION>...]
```

#### <a name="arguments"></a>自变量

* **`VERSION`**

  要卸载的指定版本。 可以逐一列出多个版本，用空格分隔。 此外还支持响应文件。

  > [!TIP]
  > 响应文件是在命令行上放置所有版本的替代方法。
  > 它们是文本文件，通常具有 \*.rsp 扩展名，每个版本都在单独的行上列出。
  > 若要为 `VERSION` 参数指定响应文件，请使用后面紧跟响应文件名的 \@ 字符。

#### <a name="options"></a>选项

## <a name="windows"></a>[Windows](#tab/windows)

* **`--all`**

  删除所有 .NET Core SDK 和运行时。

* **`--all-below <VERSION>[ <VERSION>...]`**

  仅删除版本小于指定版本的 .NET Core SDK 和运行时。 仍安装指定版本。

* **`--all-but <VERSIONS>[ <VERSION>...]`**

  除了那些指定版本外，删除所有 .NET Core SDK 和运行时。

* **`--all-but-latest`**

  删除 .NET Core SDK 和运行时（最高版本除外）。

* **`--all-lower-patches`**

  删除由较高版本的修补程序取代的 .NET Core SDK 和运行时。 此选项保护 global.json。

* **`--all-previews`**

  删除标记为预览的 .NET Core SDK 和运行时。

* **`--all-previews-but-latest`**

  删除标记为预览的 .NET Core SDK 和运行时（最高预览版除外）。

* **`--aspnet-runtime`**

  仅删除 ASP.NET Core 运行时。

* **`--hosting-bundle`**

  仅删除 .NET Core 运行时和托管绑定。

* **`--major-minor <MAJOR_MINOR>`**

  删除与指定 `major.minor` 版本相匹配的 .NET Core SDK 和运行时。

* **`--runtime`**

  仅删除 .NET Core 运行时。

* **`--sdk`**

  仅删除 .NET Core SDK。

* **`-v, --verbosity <LEVEL>`**

  设置详细程度。 允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。 默认值为 `normal`。

* **`--x64`**

  必须与 `--sdk`、`--runtime` 和 `--aspnet-runtime` 结合使用才能删除 x64 SDK 或运行时。

* **`--x86`**

  必须与 `--sdk`、`--runtime` 和 `--aspnet-runtime` 结合使用才能删除 x86 SDK 或运行时。

* `--force` 强制删除可能由 Visual Studio 使用的版本。

注意：

1. 只需 `--sdk`、`--runtime`、`--aspnet-runtime` 和 `--hosting-bundle` 中的一个。
2. `--all`、`--all-below`、`--all-but`、`--all-but-latest`、`--all-lower-patches`、`--all-previews`、`--all-previews-but-latest`、`--major-minor` 和 `[<VERSION>...]` 除外。
3. 如果未指定 `--x64` 或 `--x86`，则同时删除 x64 和 x86。

## <a name="macos"></a>[macOS](#tab/macos)

* **`--all`**

  删除所有 .NET Core SDK 和运行时。

* **`--all-below <VERSION>[ <VERSION>...]`**

  删除低于指定版本的 .NET Core SDK 和运行时。 将保留指定的版本。

* **`--all-but <VERSIONS>[ <VERSION>...]`**

  删除 .NET Core SDK 和运行时（指定版本除外）。

* **`--all-but-latest`**

  删除 .NET Core SDK 和运行时（最高版本除外）。

* **`--all-lower-patches`**

  删除由较高版本的修补程序取代的 .NET Core SDK 和运行时。 此选项保护 global.json。

* **`--all-previews`**

  删除标记为预览的 .NET Core SDK 和运行时。

* **`--all-previews-but-latest`**

  删除标记为预览的 .NET Core SDK 和运行时（最高预览版除外）。

* **`--major-minor <MAJOR_MINOR>`**

  删除与指定 `major.minor` 版本相匹配的 .NET Core SDK 和运行时。

* **`--runtime`**

  仅删除 .NET Core 运行时。

* **`--sdk`**

  仅删除 .NET Core SDK。

* **`-v, --verbosity <LEVEL>`**

  设置详细程度。 允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。 默认值为 `normal`。
  
* `--force` 强制删除可能由 Visual Studio 或 SDK 使用的版本。

注意：

1. 只需 `--sdk` 和 `--runtime` 中的一个。
2. `--all`、`--all-below`、`--all-but`、`--all-but-latest`、`--all-lower-patches`、`--all-previews`、`--all-previews-but-latest`、`--major-minor` 和 `[<VERSION>...]` 除外。

---

#### <a name="examples"></a>示例

> [!NOTE]
> 默认情况下，Visual Studio 或其他 SDK 可能需要的 .NET Core SDK 和运行时不会包含在 `dotnet-core-uninstall dry-run` 输出中。 在下面的示例中，某些指定的 SDK 和运行时可能不会包含在输出中，具体取决于计算机的状态。 若要包括所有 SDK 和运行时，请将它们显式列出为参数或使用 `--force` 选项。

* 试运行删除已被较高版本的修补程序取代的所有 .NET Core 运行时：

  ```console
  dotnet-core-uninstall dry-run --all-lower-patches --runtime
  ```

* 试运行删除低于版本 `2.2.301` 的所有 .NET Core SDK：

  ```console
  dotnet-core-uninstall whatif --all-below 2.2.301 --sdk
  ```

### <a name="step-3---uninstall-net-core-sdks-and-runtimes"></a>步骤 3 - 卸载 .NET Core SDK 和运行时

`dotnet-core-uninstall remove` 卸载由选项集合指定的 .NET Core SDK 和运行时。 该工具不能用于卸载版本 5.0 或更高版本的 SDK 和运行时。

由于此工具具有破坏性行为，因此强烈建议在运行 remove 命令之前执行试运行。 使用 `remove` 命令时，试运行将显示要删除的 .NET Core SDK 和运行时。 请参阅[是否应删除版本？](../install/remove-runtime-sdk-versions.md#should-i-remove-a-version)了解哪些 SDK 和运行时可以安全删除。

> [!CAUTION]
> 请记住以下注意事项：
>
>- 此工具可以卸载计算机上 `global.json` 文件所需的 .NET Core SDK 版本。 可以从[下载 .NET Core](https://dotnet.microsoft.com/download/dotnet-core) 页面重新安装 .NET Core SDK。
>- 此工具可以卸载计算机上依赖于框架的应用程序所需的 .NET Core 运行时版本。 可以从[下载 .NET Core](https://dotnet.microsoft.com/download/dotnet-core) 页面重新安装 .NET Core 运行时。
>- 此工具可以卸载 Visual Studio 所依赖的 .NET Core SDK 和运行时版本。 如果中断 Visual Studio 安装，请在 Visual Studio 安装程序中运行“修复”以返回到工作状态。

默认情况下，所有命令都将保留 Visual Studio 或其他 SDK 可能需要的 .NET Core SDK 和运行时。 可以通过将这些 SDK 和运行时显式列出为参数或使用 `--force` 选项来卸载这些 SDK 和运行时。

此工具需要提升才能卸载 .NET Core SDK 和运行时。 在 Windows 上的管理员命令提示符中运行此工具，在 macOS 上则通过 `sudo` 运行。 `dry-run` 和 `whatif` 命令不需要提升。

**dotnet-core-uninstall remove**

#### <a name="synopsis"></a>摘要

```console
dotnet-core-uninstall remove [options] [<VERSION>...]
```

#### <a name="arguments"></a>自变量

* **`VERSION`**

  要卸载的指定版本。 可以逐一列出多个版本，用空格分隔。 此外还支持响应文件。

  > [!TIP]
  > 响应文件是在命令行上放置所有版本的替代方法。
  > 它们是文本文件，通常具有 \*.rsp 扩展名，每个版本都在单独的行上列出。
  > 若要为 `VERSION` 参数指定响应文件，请使用后面紧跟响应文件名的 \@ 字符。

#### <a name="options"></a>选项

## <a name="windows"></a>[Windows](#tab/windows)

* **`--all`**

  删除所有 .NET Core SDK 和运行时。

* **`--all-below <VERSION>[ <VERSION>...]`**

  仅删除版本小于指定版本的 .NET Core SDK 和运行时。 仍安装指定版本。

* **`--all-but <VERSIONS>[ <VERSION>...]`**

  除了那些指定版本外，删除所有 .NET Core SDK 和运行时。

* **`--all-but-latest`**

  删除 .NET Core SDK 和运行时（最高版本除外）。

* **`--all-lower-patches`**

  删除由较高版本的修补程序取代的 .NET Core SDK 和运行时。 此选项保护 global.json。

* **`--all-previews`**

  删除标记为预览的 .NET Core SDK 和运行时。

* **`--all-previews-but-latest`**

  删除标记为预览的 .NET Core SDK 和运行时（最高预览版除外）。

* **`--aspnet-runtime`**

  仅删除 ASP.NET Core 运行时。

* **`--hosting-bundle`**

  仅删除 .NET Core 托管绑定。

* **`--major-minor <MAJOR_MINOR>`**

  删除与指定 `major.minor` 版本相匹配的 .NET Core SDK 和运行时。

* **`--runtime`**

  仅删除 .NET Core 运行时。

* **`--sdk`**

  仅删除 .NET Core SDK。

* **`-v, --verbosity <LEVEL>`**

  设置详细程度。 允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。 默认值为 `normal`。

* **`--x64`**

  必须与 `--sdk`、`--runtime` 和 `--aspnet-runtime` 结合使用才能删除 x64 SDK 或运行时。

* **`--x86`**

  必须与 `--sdk`、`--runtime` 和 `--aspnet-runtime` 结合使用才能删除 x86 SDK 或运行时。

* `-y, --yes` 执行命令而不需要进行是或否确认。

* `--force` 强制删除可能由 Visual Studio 使用的版本。

注意：

1. 只需 `--sdk`、`--runtime`、`--aspnet-runtime` 和 `--hosting-bundle` 中的一个。
2. `--all`、`--all-below`、`--all-but`、`--all-but-latest`、`--all-lower-patches`、`--all-previews`、`--all-previews-but-latest`、`--major-minor` 和 `[<VERSION>...]` 除外。
3. 如果未指定 `--x64` 或 `--x86`，则同时删除 x64 和 x86。

## <a name="macos"></a>[macOS](#tab/macos)

* **`--all`**

  删除所有 .NET Core SDK 和运行时。

* **`--all-below <VERSION>[ <VERSION>...]`**

  删除低于指定版本的 .NET Core SDK 和运行时。 将保留指定的版本。

* **`--all-but <VERSIONS>[ <VERSION>...]`**

  删除 .NET Core SDK 和运行时（指定版本除外）。

* **`--all-but-latest`**

  删除 .NET Core SDK 和运行时（最高版本除外）。

* **`--all-lower-patches`**

  删除由较高版本的修补程序取代的 .NET Core SDK 和运行时。 此选项保护 global.json。

* **`--all-previews`**

  删除标记为预览的 .NET Core SDK 和运行时。

* **`--all-previews-but-latest`**

  删除标记为预览的 .NET Core SDK 和运行时（最高预览版除外）。

* **`--major-minor <MAJOR_MINOR>`**

  删除与指定 `major.minor` 版本相匹配的 .NET Core SDK 和运行时。

* **`--runtime`**

  仅删除 .NET Core 运行时。

* **`--sdk`**

  仅删除 .NET Core SDK。

* **`-v, --verbosity <LEVEL>`**

  设置详细程度。 允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。 默认值为 `normal`。

* `-y, --yes` 执行命令而不需要进行 Y/N 确认。
  
* `--force` 强制删除可能由 Visual Studio 或 SDK 使用的版本。

注意：

1. 只需 `--sdk` 和 `--runtime` 中的一个。
2. `--all`、`--all-below`、`--all-but`、`--all-but-latest`、`--all-lower-patches`、`--all-previews`、`--all-previews-but-latest`、`--major-minor` 和 `[<VERSION>...]` 除外。

---

#### <a name="examples"></a>示例

> [!NOTE]
> 默认情况下，将保留 Visual Studio 或其他 SDK 可能需要的 .NET Core SDK 和运行时。 在下面的示例中，可能保留某些指定的 SDK 和运行时，具体取决于计算机的状态。 若要删除所有 SDK 和运行时，请将它们显式列出为参数或使用 `--force` 选项。

* 删除除版本 `3.0.0-preview6-27804-01` 之外的所有 .NET Core 运行时，无需进行 Y/N 确认：

  ```console
  dotnet-core-uninstall remove --all-but 3.0.0-preview6-27804-01 --runtime --yes
  ```

* 删除所有 .NET Core 1.1 SDK，无需进行 Y/N 确认：

  ```console
  dotnet-core-uninstall remove --sdk --major-minor 1.1 -y
  ```

* 删除没有控制台输出的 .NET Core 1.1.11 SDK：

  ```console
  dotnet-core-uninstall remove 1.1.11 --sdk --yes --verbosity q
  ```

* 删除可由此工具安全删除的所有 .NET Core SDK：

  ```console
  dotnet-core-uninstall remove --all --sdk
  ```

* 删除此工具可删除的所有 .NET Core SDK，包括 Visual Studio 可能需要的 SDK（不推荐）：

  ```console
  dotnet-core-uninstall remove --all --sdk --force
  ```

* 删除响应文件 `versions.rsp` 中指定的所有 .NET Core SDK

  ```console
  dotnet-core-uninstall remove --sdk @versions.rsp
  ```

  versions.rsp 的内容如下所示：
  
  ```text
  2.2.300
  2.1.700
  ```

### <a name="step-4---delete-the-nuget-fallback-folder-optional"></a>步骤 4 - 删除 NuGet 回退文件夹（可选）

在某些情况下，你不再需要 `NuGetFallbackFolder`，可能希望将其删除。 有关删除此文件夹的详细信息，请参阅[删除 NuGetFallbackFolder](../install/remove-runtime-sdk-versions.md#remove-the-nuget-fallback-folder)。

## <a name="uninstall-the-tool"></a>卸载工具

## <a name="windows"></a>[Windows](#tab/windows)

1. 打开“添加或删除程序”。
2. 搜索 `Microsoft .NET Core SDK Uninstall Tool`。
3. 选择“卸载”。

## <a name="macos"></a>[macOS](#tab/macos)

从安装目录中删除已下载的 dotnet-core-uninstall.tar.gz 文件。 如果将此文件的内容解压缩到其他目录中，请务必同时删除该内容。

---
