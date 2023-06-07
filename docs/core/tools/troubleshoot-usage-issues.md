---
title: 排查 .NET 工具使用问题
description: 了解运行 .NET 工具出现的常见问题以及可能的解决方案。
author: kdollard
ms.topic: troubleshooting
ms.date: 02/14/2020
ms.openlocfilehash: c5bac4c273cdddae609657c65448e3cc4bd3579d
ms.sourcegitcommit: b201d177e01480a139622f3bf8facd367657a472
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2020
ms.locfileid: "94633903"
---
# <a name="troubleshoot-net-tool-usage-issues"></a>排查 .NET 工具使用问题

在尝试安装或运行 .NET 工具（可能是全局工具，也可能是本地工具）时，可能会遇到问题。 本文介绍了常见的根本原因及一些可能的解决方案。

## <a name="installed-net-tool-fails-to-run"></a>安装的 .NET 工具无法运行

当 .NET 工具无法运行时，你最可能遇到下述问题之一：

* 找不到工具的可执行文件。
* 找不到 .NET 运行时的正确版本。

### <a name="executable-file-not-found"></a>找不到可执行文件

如果找不到可执行文件，则将看到如下所示的消息：

```console
Could not execute because the specified command or file was not found.
Possible reasons for this include:
  * You misspelled a built-in dotnet command.
  * You intended to execute a .NET program, but dotnet-xyz does not exist.
  * You intended to run a global tool, but a dotnet-prefixed executable with this name could not be found on the PATH.
```

可执行文件的名称决定了调用工具的方式。 相关格式请参见下表：

| 可执行文件名称的格式  | 调用格式   |
|-------------------------|---------------------|
| `dotnet-<toolName>.exe` | `dotnet <toolName>` |
| `<toolName>.exe`        | `<toolName>`        |

* 全局工具

  全局工具可安装在默认目录中，也可安装在特定位置中。 默认目录为：

  | (OS)          | 路径                          |
  |-------------|-------------------------------|
  | Linux/macOS | `$HOME/.dotnet/tools`         |
  | Windows     | `%USERPROFILE%\.dotnet\tools` |

  如果要尝试运行全局工具，请检查计算机上的 `PATH` 环境变量是否包含安装该全局工具的路径且可执行文件是否位于该路径中。

  .NET CLI 在首次使用时尝试将默认位置添加到 PATH 环境变量。 但是，在某些情况下，位置不会自动添加至 PATH：

  * 如果使用的 Linux，并且已使用 .tar.gz 文件（而非 apt-get 或 rpm）安装 .NET SDK。
  * 如果使用的是 macOS 10.15“Catalina”或更高版本。
  * 如果使用的是 macOS 10.14“Mojave”或更低版本，并且已使用 .tar.gz 文件（而非 .pkg）安装 .NET SDK 。
  * 如果已安装 .NET Core 3.0 SDK，并且已将 `DOTNET_ADD_GLOBAL_TOOLS_TO_PATH` 环境变量设置为 `false`。
  * 如果已安装 .NET Core 2.2 SDK 或更低版本，并且已将 `DOTNET_SKIP_FIRST_TIME_EXPERIENCE` 环境变量设置为 `true`。

  在这些情况下，或者如果指定了 `--tool-path` 选项，则计算机上的 `PATH` 环境变量不会自动包含安装全局工具的路径。 在这种情况下，使用 shell 提供的用于更新环境变量的任何方法，将工具位置（例如 `$HOME/.dotnet/tools`）追加到 `PATH` 环境变量。 有关详细信息，请参阅 [.NET 工具](global-tools.md)。

* 本地工具

  如果要尝试运行本地工具，请验证当前目录或其任何父目录中是否存在一个名为 dotnet-tools.json 的清单文件。 此文件还可位于项目文件夹层次结构中任意位置的 .config 文件夹下，而不是位于根文件夹中。 如果存在 dotnet-tools.json，请将其打开，检查是否存在你要尝试运行的工具。 如果该文件不包含 `"isRoot": true` 的条目，则还要进一步检查文件层次结构中是否存在其他工具清单文件。

  如果要尝试运行已通过指定的路径安装的 .NET 工具，则需要在使用该工具时包含该路径。 下面是使用安装了工具路径的工具的示例：

  ```console
  ..\<toolDirectory>\dotnet-<toolName>
  ```

### <a name="runtime-not-found"></a>找不到运行时

.NET 工具是[依赖框架的应用程序](../deploying/index.md#publish-framework-dependent)，也就是说它们依赖于计算机上安装的 .NET 运行时。 如果找不到所需的运行时，则遵循常规的 .NET 运行时前滚规则，例如：

* 应用程序前滚至指定的主要版本和次要版本的最高修补程序版本。
* 如果主要版本号和次要版本号没有匹配的运行时，则使用下一个较高的次要版本。
* 前滚不会发生在运行时的预览版之间，也不会发生在预览版和发行版之间。 因此，使用预览版创建的 .NET 工具必须由作者重新生成和重新发布，再重新安装。

在下面两种常见场景中，默认不进行前滚：

* 只有运行时的较低版本可用。 前滚仅选择运行时的较高版本。
* 只有运行时的较高版本可用。 前滚不跨越主要版本边界。

如果应用程序找不到合适的运行时，则它无法运行并报告错误。

若要查看计算机上安装了哪些 .NET 运行时，可使用以下命令之一：

```dotnetcli
dotnet --list-runtimes
dotnet --info
```

如果你认为此工具应支持你当前安装的运行时版本，可联系工具作者，询问他们是否可更新版本号或实现多目标。 在工具作者编译其工具包并使用更新后的版本号将其重新发布到 NuGet 之后，你可更新你的副本。 虽然这种情况不会发生，但最快捷的解决方案是安装适合你要尝试运行的工具的运行时版本。 若要下载特定的 .NET 运行时版本，请访问 [.NET 下载页](https://dotnet.microsoft.com/download/dotnet-core)。

如果将 .NET SDK 安装到非默认位置，则需要将环境变量 `DOTNET_ROOT` 设置到包含 `dotnet` 可执行文件的目录。

## <a name="net-tool-installation-fails"></a>.NET 工具安装失败

.NET 全局或本地工具安装失败的原因有很多。 当工具安装失败时，你将看到如下所示的消息：

```console
Tool '{0}' failed to install. This failure may have been caused by:

* You are attempting to install a preview release and did not use the --version option to specify the version.
* A package by this name was found, but it was not a .NET tool.
* The required NuGet feed cannot be accessed, perhaps because of an Internet connection problem.
* You mistyped the name of the tool.

For more reasons, including package naming enforcement, visit https://aka.ms/failure-installing-tool
```

为帮助诊断这些失败情况，会随同之前的消息直接向用户显示 NuGet 消息。 NuGet 消息可帮助你确定问题。

### <a name="package-naming-enforcement"></a>包命名强制

Microsoft 针对工具的包 ID 更改了相关指导，导致没法用预测出的名称找到很多工具。 新的指导是所有 Microsoft 工具均附上“Microsoft”这一前缀。 此前缀已预留，仅用于使用 Microsoft 授权证书签名的包。

在过渡期间，一些 Microsoft 工具将采用包 ID 的旧格式，而另外一些将采用新格式：

```dotnetcli
dotnet tool install -g Microsoft.<toolName>
dotnet tool install -g <toolName>
```

更新包 ID 后，你将需要更改到新的包 ID 以获取最新更新。 带简化工具名称的包将遭到弃用。

### <a name="preview-releases"></a>预览版本

* 你正在尝试安装预览版本，但未使用 `--version` 选项来指定该版本。

必须用名称的一部分指定处于预览状态的 .NET 工具，这样才能指示它们现为预览版。 不需要包含整个预览版。 假定版本号都采用预期的格式，则你可使用与下例类似的内容：

```dotnetcli
dotnet tool install -g --version 1.1.0-pre <toolName>
```

### <a name="package-isnt-a-net-tool"></a>包不是 .NET 工具

* 已按此名称找到 NuGet 包，但它不是 .NET 工具。

如果尝试安装的 NuGet 包是常规 NuGet 包而非 .NET 工具，你将看到如下所示的错误：

> NU1212：`<ToolName>` 的项目包组合无效。 DotnetToolReference 项目类型仅可包含 DotnetTool 类型的引用。

### <a name="nuget-feed-cant-be-accessed"></a>无法访问 NuGet 源

* 无法访问必需的 NuGet 源，这可能是由 Internet 连接问题造成的。

需要访问包含工具包的 NuGet 源才能安装工具。 如果源不可用，则安装将失败。 可使用 `nuget.config` 修改源、请求特定的 `nuget.config` 文件，也可使用 `--add-source` 开关指定其他源。 默认情况下，对于无法连接的任何源，NuGet 都将引发错误。 标志 `--ignore-failed-sources` 可跳过这些不可访问的源。

### <a name="package-id-incorrect"></a>包 ID 错误

* 工具的名称拼写错误。

常见的失败原因是工具名称不正确。 原因可能是拼写错误，或者工具已移动或已被弃用。 对于 NuGet.org 上的工具，确保名称正确的一种方式是在 NuGet.org 处搜索该工具并复制安装命令。

## <a name="see-also"></a>请参阅

* [.NET 工具](global-tools.md)
