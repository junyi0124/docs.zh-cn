---
title: dotnet-trace 诊断工具 - .NET CLI
description: 了解如何通过使用 .NET EventPipe 来安装和使用 dotnet-trace CLI 工具，以在没有本机探查器的情况下收集运行中的进程的 .NET 跟踪。
ms.date: 11/17/2020
ms.openlocfilehash: 868ce7828eee6bd7f2101d5d6a65c7f7bf87fe24
ms.sourcegitcommit: 81f1bba2c97a67b5ca76bcc57b37333ffca60c7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97009529"
---
# <a name="dotnet-trace-performance-analysis-utility"></a>dotnet-trace 性能分析实用工具

本文适用于： ✔️ .NET Core 3.0 SDK 及更高版本

## <a name="install"></a>安装

可采用两种方法来下载和安装 `dotnet-trace`：

- **dotnet 全局工具：**

  若要安装最新版 `dotnet-trace` [NuGet 包](https://www.nuget.org/packages/dotnet-trace)，请使用 [dotnet tool install](../tools/dotnet-tool-install.md) 命令：

  ```dotnetcli
  dotnet tool install --global dotnet-trace
  ```

- **直接下载：**

  下载与平台相匹配的工具可执行文件：

  | (OS)  | 平台 |
  | --- | -------- |
  | Windows | [x86](https://aka.ms/dotnet-trace/win-x86) \| [x64](https://aka.ms/dotnet-trace/win-x64) \| [arm](https://aka.ms/dotnet-trace/win-arm) \| [arm-x64](https://aka.ms/dotnet-trace/win-arm64) |
  | macOS   | [x64](https://aka.ms/dotnet-trace/osx-x64) |
  | Linux   | [x64](https://aka.ms/dotnet-trace/linux-x64) \| [arm](https://aka.ms/dotnet-trace/linux-arm) \| [arm64](https://aka.ms/dotnet-trace/linux-arm64) \| [musl-x64](https://aka.ms/dotnet-trace/linux-musl-x64) \| [musl-arm64](https://aka.ms/dotnet-trace/linux-musl-arm64) |

## <a name="synopsis"></a>摘要

```console
dotnet-trace [-h, --help] [--version] <command>
```

## <a name="description"></a>描述

`dotnet-trace` 工具：

* 是一个跨平台的 .NET Core 工具。
* 在不使用本机探查器的情况下启用正在运行的进程的 .NET Core 跟踪集合。
* 是基于 .NET Core 运行时的 [`EventPipe`](./eventpipe.md) 构建的。
* 在 Windows、Linux 或 macOS 上提供相同体验。

## <a name="options"></a>选项

- **`-h|--help`**

  显示命令行帮助。

- **`--version`**

  显示 dotnet-dump 实用工具的版本。

## <a name="commands"></a>命令

| 命令                                                   |
|-----------------------------------------------------------|
| [dotnet-trace collect](#dotnet-trace-collect)             |
| [dotnet-trace convert](#dotnet-trace-convert)             |
| [dotnet-trace ps](#dotnet-trace-ps)                       |
| [dotnet-trace list-profiles](#dotnet-trace-list-profiles) |

## <a name="dotnet-trace-collect"></a>dotnet-trace collect

从正在运行的进程中收集诊断跟踪。

### <a name="synopsis"></a>摘要

```console
dotnet-trace collect [--buffersize <size>] [--clreventlevel <clreventlevel>] [--clrevents <clrevents>]
    [--format <Chromium|NetTrace|Speedscope>] [-h|--help]
    [-n, --name <name>] [--diagnostic-port] [-o|--output <trace-file-path>] [-p|--process-id <pid>]
    [--profile <profile-name>] [--providers <list-of-comma-separated-providers>]
    [-- <command>] (for target applications running .NET 5.0 or later)
```

### <a name="options"></a>选项

- **`--buffersize <size>`**

  设置内存中循环缓冲区的大小（以 MB 表示）。 默认值为 256 MB。

- **`--clreventlevel <clreventlevel>`**

  要发出的 CLR 事件的详细级别。

- **`--clrevents <clrevents>`**

  要发出的 CLR 运行时事件的列表。

- **`--format {Chromium|NetTrace|Speedscope}`**

  设置跟踪文件转换的输出格式。 默认值为 `NetTrace`。

- **`-n, --name <name>`**

  从中收集跟踪的进程的名称。

- **`--diagnostic-port <path-to-port>`**

  要创建的诊断端口的名称。 请参阅[使用诊断端口从应用启动时开始收集跟踪](#use-diagnostic-port-to-collect-a-trace-from-app-startup)，以了解如何使用此选项从应用启动时开始收集跟踪。

- **`-o|--output <trace-file-path>`**

  收集的跟踪数据的输出路径。 如果未指定，则默认为 `trace.nettrace`。

- **`-p|--process-id <PID>`**

  从中收集跟踪的进程 ID。

- **`--profile <profile-name>`**

  一组命名的预定义提供程序配置，允许简明地指定常见跟踪方案。 可用配置文件如下：

 | 配置文件 | 说明 |
 |---------|-------------|
 |`cpu-sampling`|可用于跟踪 CPU 使用情况和一般 .NET 运行时信息。 如果未指定配置文件或提供程序，则这是默认选项。|
 |`gc-verbose`|跟踪 GC 集合并示例对象分配。|
 |`gc-collect`|仅以极低的开销跟踪 GC 集合。|

- **`--providers <list-of-comma-separated-providers>`**

  要启用的 `EventPipe` 提供程序的以逗号分隔的列表。 这些提供程序会补充 `--profile <profile-name>` 隐含的任何提供程序。 如果特定提供程序存在任何不一致的情况，此配置将优先于配置文件中的隐式配置。

  此提供程序列表的格式为：

  - `Provider[,Provider]`
  - `Provider` 的格式为：`KnownProviderName[:Flags[:Level][:KeyValueArgs]]`。
  - `KeyValueArgs` 的格式为：`[key1=value1][;key2=value2]`。

- `-- <command>`（仅适用于运行 .NET 5.0 的目标应用程序）

  在集合配置参数之后，用户可以追加 `--`，后跟一个命令，以启动至少具有 5.0 运行时的 .NET 应用程序。 这在过程早期发生诊断问题（如启动性能问题或程序集加载程序和绑定器错误）时可能会有所帮助。

  > [!NOTE]
  > 使用此选项监视第一个 .NET 5.0 进程，该进程与该工具通信，这意味着如果命令启动多个 .NET 应用程序，它将仅收集第一个应用。 因此，建议在自包含应用程序上使用此选项，或使用 `dotnet exec <app.dll>` 选项。

## <a name="dotnet-trace-convert"></a>dotnet-trace convert

将 `nettrace` 跟踪转换为备用格式，以便用于备用跟踪分析工具。

### <a name="synopsis"></a>摘要

```console
dotnet-trace convert [<input-filename>] [--format <Chromium|NetTrace|Speedscope>] [-h|--help] [-o|--output <output-filename>]
```

### <a name="arguments"></a>自变量

- **`<input-filename>`**

  要转换的输入跟踪文件。 默认为 trace.nettrace  。

### <a name="options"></a>选项

- **`--format <Chromium|NetTrace|Speedscope>`**

  设置跟踪文件转换的输出格式。

- **`-o|--output <output-filename>`**

  输出文件名。 将添加目标格式的扩展。

## <a name="dotnet-trace-ps"></a>dotnet-trace ps

 列出可从中收集跟踪的 dotnet 进程。

### <a name="synopsis"></a>摘要

```console
dotnet-trace ps [-h|--help]
```

## <a name="dotnet-trace-list-profiles"></a>dotnet-trace list-profiles

列出预生成的跟踪配置文件，并描述每个配置文件中包含的提供程序和筛选器。

### <a name="synopsis"></a>摘要

```console
dotnet-trace list-profiles [-h|--help]
```

## <a name="collect-a-trace-with-dotnet-trace"></a>使用 dotnet-trace 收集跟踪

若要使用 `dotnet-trace` 收集跟踪，请执行以下操作：

- 需要首先查找要从中收集跟踪的 .NET Core 应用程序的进程标识符 (PID)。

  - 例如，在 Windows 上，可以使用任务管理器或 `tasklist` 命令。
  - 在 Linux 上，使用 `ps` 命令。
  - [dotnet-trace ps](#dotnet-trace-ps)

- 运行下面的命令：

  ```console
  dotnet-trace collect --process-id <PID>
  ```

  前面的命令生成类似于以下内容的输出：

  ```console
  Press <Enter> to exit...
  Connecting to process: <Full-Path-To-Process-Being-Profiled>/dotnet.exe
  Collecting to file: <Full-Path-To-Trace>/trace.nettrace
  Session Id: <SessionId>
  Recording trace 721.025 (KB)
  ```

- 按 `<Enter>` 键停止收集。 `dotnet-trace` 会将完成将事件记录到 trace.nettrace 文件中  。

## <a name="launch-a-child-application-and-collect-a-trace-from-its-startup-using-dotnet-trace"></a>启动子应用程序，并使用 dotnet-trace 从启动中收集跟踪

> [!IMPORTANT]
> 这仅适用于运行 .NET 5.0 或更高版本的应用。

有时，从进程启动中收集进程的跟踪可能很有用。 对于运行 .NET 5.0 或更高版本的应用，可以使用 dotnet-trace 来做到这一点。

这将启动 `hello.exe` 并以 `arg1` 和 `arg2` 作为其命令行参数，从其运行时启动中收集跟踪：

```console
dotnet-trace collect -- hello.exe arg1 arg2
```

前面的命令生成类似于以下内容的输出：

```console
No profile or providers specified, defaulting to trace profile 'cpu-sampling'

Provider Name                           Keywords            Level               Enabled By
Microsoft-DotNETCore-SampleProfiler     0x0000F00000000000  Informational(4)    --profile
Microsoft-Windows-DotNETRuntime         0x00000014C14FCCBD  Informational(4)    --profile

Process        : E:\temp\gcperfsim\bin\Debug\net5.0\gcperfsim.exe
Output File    : E:\temp\gcperfsim\trace.nettrace


[00:00:00:05]   Recording trace 122.244  (KB)
Press <Enter> or <Ctrl+C> to exit...
```

可以通过按 `<Enter>` 或 `<Ctrl + C>` 键来停止收集跟踪。 此操作还将退出 `hello.exe`。

> [!NOTE]
> 通过 dotnet-trace 启动 `hello.exe` 将重定向其输入/输出，你将无法与其 stdin/stdout 进行交互。
> 通过 CTRL+C 或 SIGTERM 退出工具将安全地结束该工具和子进程。
> 如果子进程在工具之前退出，工具也将退出，应可安全查看跟踪。

## <a name="use-diagnostic-port-to-collect-a-trace-from-app-startup"></a>使用诊断端口从应用启动时开始收集跟踪

  > [!IMPORTANT]
  > 这仅适用于运行 .NET 5.0 或更高版本的应用。

诊断端口是 .NET 5 中新增的运行时功能，你可以通过它从应用启动时开始跟踪。 若要使用 `dotnet-trace` 执行此操作，可以使用以上示例中所述的 `dotnet-trace collect -- <command>`，也可以使用 `--diagnostic-port` 选项。

使用 `dotnet-trace <collect|monitor> -- <command>` 以子进程的形式启动应用程序，是从启动时开始对其进行快速跟踪的最简单方法。

但是，如果想要更好地控制所跟踪应用的生存期（例如，仅在前 10 分钟内监视应用并继续执行），或者如果需要使用 CLI 与应用进行交互，则使用 `--diagnostic-port` 选项可以同时控制要监视的目标应用和 `dotnet-trace`。

1. 以下命令使 `dotnet-trace` 创建一个名为 `myport.sock` 的诊断套接字并等待连接。

    > ```dotnet-cli
    > dotnet-trace collect --diagnostic-port myport.sock
    > ```

    输出：

    > ```bash
    > Waiting for connection on myport.sock
    > Start an application with the following environment variable: DOTNET_DiagnosticPorts=/home/user/myport.sock
    > ```

2. 在单独的控制台中，通过将环境变量 `DOTNET_DiagnosticPorts` 设置为 `dotnet-trace` 输出中的值，启动目标应用程序。

    > ```bash
    > export DOTNET_DiagnosticPorts=/home/user/myport.sock
    > ./my-dotnet-app arg1 arg2
    > ```

    这应该会使 `dotnet-trace` 开始跟踪 `my-dotnet-app`：

    > ```bash
    > Waiting for connection on myport.sock
    > Start an application with the following environment variable: DOTNET_DiagnosticPorts=myport.sock
    > Starting a counter session. Press Q to quit.
    > ```

    > [!IMPORTANT]
    > 通过 `dotnet run` 启动应用可能会产生问题，因为 dotnet CLI 可能会生成许多子进程，这些子程序不是应用，并且可以在应用之前连接到 `dotnet-trace`，从而导致应用在运行时挂起。 建议直接使用应用的独立版本或使用 `dotnet exec` 来启动应用程序。

## <a name="view-the-trace-captured-from-dotnet-trace"></a>查看由 dotnet-trace 捕获的跟踪

在 Windows 上，可以使用 [PerfView](https://github.com/microsoft/perfview) 查看 .nettrace 文件以进行分析  ：对于其他平台上收集的跟踪，可以将跟踪文件移动到 Windows 计算机上，以在 PerfView 上进行查看。

在 Linux 上，可以通过将 `dotnet-trace` 的输出格式更改为 `speedscope` 来查看跟踪。 可以使用 `-f|--format` 选项更改输出文件格式 - `-f speedscope` 会使 `dotnet-trace` 生成 `speedscope` 文件。 可以在 `nettrace`（默认选项）和 `speedscope` 之间进行选择。 可以在 <https://www.speedscope.app> 打开 `Speedscope` 文件。

> [!NOTE]
> .NET Core 运行时以 `nettrace` 格式生成跟踪。 跟踪完成后，跟踪将转换为 speedscope（如果指定）。 由于某些转换可能会导致数据丢失，因此，原始 `nettrace` 文件将保留在转换后的文件旁边。

## <a name="use-dotnet-trace-to-collect-counter-values-over-time"></a>使用 dotnet-trace 收集随时间变化的计数器值

`dotnet-trace` 可以：

* 在性能敏感的环境中使用 `EventCounter` 进行基本运行状况监视。 例如，在生产环境中。
* 收集跟踪，这样就不需要实时查看。

例如，若要收集运行时性能计数器值，请使用以下命令：

```console
dotnet-trace collect --process-id <PID> --providers System.Runtime:0:1:EventCounterIntervalSec=1
```

上述命令通知运行时计数器每秒报告一次，以便进行轻量型运行状况监视。 通过使用较大的值（例如 60）替换 `EventCounterIntervalSec=1`，可以收集计数器数据中粒度较小的跟踪。

以下命令比以上命令产生更小的开销和跟踪大小：

```console
dotnet-trace collect --process-id <PID> --providers System.Runtime:0:1:EventCounterIntervalSec=1,Microsoft-Windows-DotNETRuntime:0:1,Microsoft-DotNETCore-SampleProfiler:0:1
```

以上命令会禁用运行时事件和托管堆栈探查器。

## <a name="net-providers"></a>.NET 提供程序

.NET Core 运行时支持以下 .NET 提供程序： .NET Core 使用相同的关键字来启用 `Event Tracing for Windows (ETW)` 和 `EventPipe` 跟踪。

| 提供程序名称                            | 信息 |
|------------------------------------------|-------------|
| `Microsoft-Windows-DotNETRuntime`        | [运行时提供程序](../../framework/performance/clr-etw-providers.md#the-runtime-provider)<br>[CLR 运行时关键字](../../framework/performance/clr-etw-keywords-and-levels.md#runtime) |
| `Microsoft-Windows-DotNETRuntimeRundown` | [断开提供程序](../../framework/performance/clr-etw-providers.md#the-rundown-provider)<br>[CLR 断开关键字](../../framework/performance/clr-etw-keywords-and-levels.md#rundown) |
| `Microsoft-DotNETCore-SampleProfiler`    | 启用示例探查器。 |
