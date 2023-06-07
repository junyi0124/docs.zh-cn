---
title: dotnet test 命令
description: dotnet test 命令可用于在给定项目中执行单元测试。
ms.date: 04/29/2020
ms.openlocfilehash: a5666cfe4c09b2b88d77b256fac922154c7d6bd7
ms.sourcegitcommit: b201d177e01480a139622f3bf8facd367657a472
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/15/2020
ms.locfileid: "94634378"
---
# <a name="dotnet-test"></a>dotnet test

本文适用于： ✔️ .NET Core 2.1 SDK 及更高版本

## <a name="name"></a>“属性”

`dotnet test` - 用于执行单元测试的 .NET 测试驱动程序。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet test [<PROJECT> | <SOLUTION> | <DIRECTORY> | <DLL>]
    [-a|--test-adapter-path <ADAPTER_PATH>] [--blame] [--blame-crash]
    [--blame-crash-dump-type <DUMP_TYPE>] [--blame-crash-collect-always]
    [--blame-hang] [--blame-hang-dump-type <DUMP_TYPE>]
    [--blame-hang-timeout <TIMESPAN>]
    [-c|--configuration <CONFIGURATION>]
    [--collect <DATA_COLLECTOR_NAME>]
    [-d|--diag <LOG_FILE>] [-f|--framework <FRAMEWORK>]
    [--filter <EXPRESSION>] [--interactive]
    [-l|--logger <LOGGER>] [--no-build]
    [--nologo] [--no-restore] [-o|--output <OUTPUT_DIRECTORY>]
    [-r|--results-directory <RESULTS_DIR>] [--runtime <RUNTIME_IDENTIFIER>]
    [-s|--settings <SETTINGS_FILE>] [-t|--list-tests]
    [-v|--verbosity <LEVEL>] [[--] <RunSettings arguments>]

dotnet test -h|--help
```

## <a name="description"></a>描述

`dotnet test` 命令用于在给定的解决方案中执行单元测试。 `dotnet test` 命令生成解决方案，并为解决方案中的每个测试项目运行测试主机应用程序。 测试主机使用测试框架（例如，MSTest、NUnit 或 xUnit）在给定项目中执行测试，并报告每个测试成功与否。 如果所有测试均成功，测试运行程序将返回 0 作为退出代码；否则将返回 1。

对于多目标项目，将为每个目标框架运行测试。 测试主机和单元测试框架打包为 NuGet 包，并还原为项目的普通依赖项。

测试项目使用普通 `<PackageReference>` 元素指定测试运行程序，如下方示例项目文件所示：

[!code-xml[XUnit Basic Template](../../../samples/snippets/csharp/xunit-test/xunit-test.csproj)]

如果 `Microsoft.NET.Test.Sdk` 是测试主机，则 `xunit` 是测试框架。 另外，`xunit.runner.visualstudio` 是测试适配器，可便于 xUnit 框架与测试主机一起运行。

### <a name="implicit-restore"></a>隐式还原

[!INCLUDE[dotnet restore note](~/includes/dotnet-restore-note.md)]

## <a name="arguments"></a>自变量

- **`PROJECT | SOLUTION | DIRECTORY | DLL`**

  - 指向测试项目的路径。
  - 解决方案的路径。
  - 包含项目或解决方案的目录的路径。
  - 测试项目 .dll 文件的路径。

  如果未指定，则会在当前目录中搜索项目或解决方案。

## <a name="options"></a>选项

- **`-a|--test-adapter-path <ADAPTER_PATH>`**

  要在其中搜索其他测试适配器的目录的路径。 只检查后缀为 `.TestAdapter.dll` 的 .dll 文件。 如果未指定，则会搜索测试 .dll 的目录。

- **`--blame`**

  在意见模式中运行测试。 此选项有助于隔离导致测试主机出现故障的有问题的测试。 检测到故障时，它会在 `TestResults/<Guid>/<Guid>_Sequence.xml` 中创建一个序列文件，用于捕获在出现故障之前运行的测试的顺序。

- **`--blame-crash`** （自 .NET 5.0 预览版 SDK 起可用）

  在追责模式下运行测试，并在测试主机意外退出时收集故障转储。 此选项取决于所使用的 .NET 版本、错误的类型和操作系统。
  
  对于托管代码中的异常，将在 .NET 5.0 及更高版本上自动收集转储。 对于 testhost 或也在 .NET 5.0 上运行并且出现故障的任何子进程，它将生成转储。 本机代码中的故障将不会生成转储。 此选项适用于 Windows、macOS 和 Linux。
  
  本机代码中的故障转储（或者当使用 .NET Core 3.1 或更早版本时）只能使用 Procdump 在 Windows 上进行收集。 包含 procdump.exe 和 procdump64.exe 的目录必须位于 PATH 或 PROCDUMP_PATH 环境变量中。 [下载工具](/sysinternals/downloads/procdump)。 意味着 `--blame`。
  
  若要从 .NET 5.0 或更高版本上运行的本机应用程序收集故障转储，可以通过将 `VSTEST_DUMP_FORCEPROCDUMP` 环境变量设置为 `1` 来强制执行 Procdump 的使用。

- **`--blame-crash-dump-type <DUMP_TYPE>`** （自 .NET 5.0 预览版 SDK 起可用）

  要收集的故障转储的类型。 意味着 `--blame-crash`。

- **`--blame-crash-collect-always`** （自 .NET 5.0 预览版 SDK 起可用）

  在预期和意外的测试主机退出时收集故障转储。

- **`--blame-hang`** （自 .NET 5.0 预览版 SDK 起可用）

  在追责模式下运行测试，并在测试超过给定超时时长时收集挂起转储。

- **`--blame-hang-dump-type <DUMP_TYPE>`** （自 .NET 5.0 预览版 SDK 起可用）

  要收集的故障转储的类型。 它应为 `full`、`mini` 或 `none`。 指定 `none` 时，测试主机将在超时时终止，但不会收集任何转储。 意味着 `--blame-hang`。

- **`--blame-hang-timeout <TIMESPAN>`** （自 .NET 5.0 预览版 SDK 起可用）

  每个测试超时时间，在此时间后，将触发挂起转储，并转储和终止测试主机进程及其所有子进程。 超时值是采用以下格式之一指定的：
  
  - 1.5h、1.5hour、1.5hours
  - 90m、90min、90minute、90minutes
  - 5400s、5400sec、5400second、5400seconds
  - 5400000ms、5400000mil、5400000millisecond、5400000milliseconds

  如果未使用单位（例如，5400000），则假定该值以毫秒为单位。 与数据驱动的测试一起使用时，超时行为取决于所使用的测试适配器。 对于 xUnit 和 NUnit，会在每个测试用例后更新超时。 对于 MSTest，超时用于所有测试用例。 此选项在使用 netcoreapp 2.1 和更高版本的 Windows 上，在使用 netcoreapp 3.1 和更高版本的 Linux 上以及在使用 net5.0 或更高版本的 macOS 上受支持。 意味着 `--blame` 和 `--blame-hang`。

- **`-c|--configuration <CONFIGURATION>`**

  定义生成配置。 默认值为 `Debug`，但项目配置可以替代此默认 SDK 设置。

- **`--collect <DATA_COLLECTOR_NAME>`**

  为测试运行启用数据收集器。 有关详细信息，请参阅[监视和分析测试运行](https://aka.ms/vstest-collect)。
  
  若要在 .NET Core 支持的任何平台上收集代码覆盖率，请安装 [Coverlet](https://github.com/coverlet-coverage/coverlet/blob/master/README.md) 并使用 `--collect:"XPlat Code Coverage"` 选项。

  在 Windows 上，可以使用 `--collect "Code Coverage"` 选项收集代码覆盖率。 此选项将生成“.coverage”文件，该文件可在 Visual Studio 2019 Enterprise 中打开。 有关详细信息，请参阅[使用代码覆盖率](/visualstudio/test/using-code-coverage-to-determine-how-much-code-is-being-tested)和[自定义代码覆盖率分析](/visualstudio/test/customizing-code-coverage-analysis)。

- **`-d|--diag <LOG_FILE>`**

  启用测试平台的诊断模式，并将诊断消息写入到指定文件及其旁边的文件。 正在记录消息的进程可确定创建了哪些文件，如测试主机日志的 `*.host_<date>.txt`，以及数据收集器日志的 `*.datacollector_<date>.txt`。

- **`-f|--framework <FRAMEWORK>`**

  强制将 `dotnet` 或 .NET Framework 测试主机用于测试二进制文件。 此选项只确定要使用哪种类型的主机。 要使用的实际框架版本由测试项目的 runtimeconfig.json 决定。 如果未指定，则 [TargetFramework 程序集特性](/dotnet/api/system.runtime.versioning.targetframeworkattribute)用于确定主机的类型。 如果已从 .dll 中去除此特性，则使用的是 .NET Framework 主机。

- **`--filter <EXPRESSION>`**

  使用给定表达式筛选掉当前项目中的测试。 有关详细信息，请参阅[筛选选项详细信息](#filter-option-details)部分。 若要获取使用选择性单元测试筛选的其他信息和示例，请参阅[运行选择性单元测试](../testing/selective-unit-tests.md)。

- **`-h|--help`**

  打印出有关命令的简短帮助。

- **`--interactive`**

  允许命令停止并等待用户输入或操作。 例如，完成身份验证。 自 .NET Core 3.0 SDK 起可用。

- **`-l|--logger <LOGGER>`**

  指定测试结果记录器。 与 MSBuild 不同，dotnet 测试不接受缩写，应使用 `-l "console;verbosity=detailed"`，而不使用 `-l "console;v=d"`。

- **`--no-build`**

  不在运行测试项目之前生成它。 还将隐式设置 - `--no-restore` 标记。

- **`--nologo`**

  运行测试，而不显示 Microsoft TestPlatform 横幅。 自 .NET Core 3.0 SDK 起可用。

- **`--no-restore`**

  运行此命令时不执行隐式还原。

- **`-o|--output <OUTPUT_DIRECTORY>`**

  查找要运行的二进制文件的目录。 如果未指定，则默认路径为 `./bin/<configuration>/<framework>/`。  对于具有多个目标框架的项目（通过 `TargetFrameworks` 属性），在指定此选项时还需要定义 `--framework`。 `dotnet test` 始终从输出目录运行测试。 可以使用 <xref:System.AppDomain.BaseDirectory%2A?displayProperty=nameWithType> 以使用输出目录中的测试资产。

- **`-r|--results-directory <RESULTS_DIR>`**

  用于放置测试结果的目录。 如果指定的目录不存在，则会创建该目录。 默认值为包含项目文件的目录中的 `TestResults`。

- **`--runtime <RUNTIME_IDENTIFIER>`**

  要针对其测试的目标运行时。

- **`-s|--settings <SETTINGS_FILE>`**

  `.runsettings` 文件用于运行测试。 `TargetPlatform` 元素 (x86|x64) 对 `dotnet test` 不起作用。 若要运行面向 x86 的测试，请安装 .NET Core 的 x86 版本。 路径上 dotnet.exe 的位数是用于运行测试的内容。 有关更多信息，请参见以下资源：

  - [使用 `.runsettings` 文件配置单元测试。](/visualstudio/test/configure-unit-tests-by-using-a-dot-runsettings-file)
  - [配置测试运行](https://github.com/Microsoft/vstest-docs/blob/master/docs/configure.md)

- **`-t|--list-tests`**

  列出已发现的测试，而不是运行测试。

- **`-v|--verbosity <LEVEL>`**

  设置命令的详细级别。 允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。 默认值为 `minimal`。 有关详细信息，请参阅 <xref:Microsoft.Build.Framework.LoggerVerbosity>。

- `RunSettings` 参数

 内联的 `RunSettings` 作为“-- ”（请注意 -- 后面有空格）后的最后一个命令行参数传递。 内联的 `RunSettings` 被指定为 `[name]=[value]` 对。 空格用于分隔多个 `[name]=[value]` 对。

  示例：`dotnet test -- MSTest.DeploymentEnabled=false MSTest.MapInconclusiveToFailed=True`

  有关详细信息，请参阅[通过命令行传递 RunSettings 参数](https://github.com/Microsoft/vstest-docs/blob/master/docs/RunSettingsArguments.md)。

## <a name="examples"></a>示例

- 运行当前目录所含项目中的测试：

  ```dotnetcli
  dotnet test
  ```

- 运行 `test1` 项目中的测试：

  ```dotnetcli
  dotnet test ~/projects/test1/test1.csproj
  ```

- 在当前目录运行项目中的测试，并以 trx 格式生成测试结果文件：

  ```dotnetcli
  dotnet test --logger trx
  ```

- 在当前目录运行项目中的测试，并生成代码覆盖率文件（安装 [Coverlet](https://github.com/coverlet-coverage/coverlet/blob/master/Documentation/VSTestIntegration.md) 收集器集成后）：

  ```dotnetcli
  dotnet test --collect:"XPlat Code Coverage"
  ```

- 在当前目录运行项目中的测试，并生成代码覆盖率文件（仅限 Windows）：

  ```dotnetcli
  dotnet test --collect "Code Coverage"
  ```

- 在当前目录中运行项目中的测试，并将详细的测试结果记录到控制台：

  ```dotnetcli
  dotnet test --logger "console;verbosity=detailed"
  ```

- 在当前目录下的项目中运行测试，并报告在测试主机发生故障时正在进行的测试：

  ```dotnetcli
  dotnet test --blame
  ```

## <a name="filter-option-details"></a>筛选选项详细信息

`--filter <EXPRESSION>`

`<Expression>` 格式为 `<property><operator><value>[|&<Expression>]`。

`<property>` 是 `Test Case` 的特性。 下面介绍了常用单元测试框架支持的属性：

| 测试框架 | 支持的属性                                                                                      |
| -------------- | --------------------------------------------------------------------------------------------------------- |
| MSTest         | <ul><li>FullyQualifiedName</li><li>“属性”</li><li>ClassName</li><li>Priority</li><li>TestCategory</li></ul> |
| xUnit          | <ul><li>FullyQualifiedName</li><li>DisplayName</li><li>Traits</li></ul>                                   |
| NUnit          | <ul><li>FullyQualifiedName</li><li>“属性”</li><li>TestCategory</li><li>Priority</li></ul>                                   |

`<operator>` 说明了属性和值之间的关系：

| 运算符 | 函数        |
| :------: | --------------- |
| `=`      | 完全匹配     |
| `!=`     | 非完全匹配 |
| `~`      | 包含        |
| `!~`     | 不包含    |

`<value>` 是字符串。 所有查找都不区分大小写。

不含 `<operator>` 的表达式自动被视为 `FullyQualifiedName` 属性上的 `contains`（例如，`dotnet test --filter xyz` 与 `dotnet test --filter FullyQualifiedName~xyz` 相同）。

表达式可与条件运算符结合使用：

| 运算符            | 函数 |
| ------------------- | -------- |
| <code>&#124;</code> | 或       |
| `&`                 | AND      |

使用条件运算符时，可以用括号将表达式括起来（例如，`(Name~TestMethod1) | (Name~TestMethod2)`）。

若要获取使用选择性单元测试筛选的其他信息和示例，请参阅[运行选择性单元测试](../testing/selective-unit-tests.md)。

## <a name="see-also"></a>请参阅

- [框架和目标](../../standard/frameworks.md)
- [.NET 运行时标识符 (RID) 目录](../rid-catalog.md)
- [通过命令行传递 runsettings 参数](https://github.com/Microsoft/vstest-docs/blob/master/docs/RunSettingsArguments.md)
