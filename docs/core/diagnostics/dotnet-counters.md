---
title: dotnet-counters 诊断工具 - .NET CLI
description: 了解如何安装和使用 dotnet-counter CLI 工具进行临时运行状况监视和初级性能调查。
ms.date: 11/17/2020
ms.openlocfilehash: 48e3b038ddb5c9421367612a592c5ba6b9459791
ms.sourcegitcommit: 81f1bba2c97a67b5ca76bcc57b37333ffca60c7b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/10/2020
ms.locfileid: "97009542"
---
# <a name="investigate-performance-counters-dotnet-counters"></a>调查性能计数器 (dotnet-counters)

 本文适用于： ✔️ .NET Core 3.0 SDK 及更高版本

## <a name="install"></a>安装

可采用两种方法来下载和安装 `dotnet-counters`：

- **dotnet 全局工具：**

  若要安装最新版 `dotnet-counters` [NuGet 包](https://www.nuget.org/packages/dotnet-counters)，请使用 [dotnet tool install](../tools/dotnet-tool-install.md) 命令：

  ```dotnetcli
  dotnet tool install --global dotnet-counters
  ```

- **直接下载：**

  下载与平台相匹配的工具可执行文件：

  | (OS)  | 平台 |
  | --- | -------- |
  | Windows | [x86](https://aka.ms/dotnet-counters/win-x86) \| [x64](https://aka.ms/dotnet-counters/win-x64) \| [arm](https://aka.ms/dotnet-counters/win-arm) \| [arm-x64](https://aka.ms/dotnet-counters/win-arm64) |
  | macOS   | [x64](https://aka.ms/dotnet-counters/osx-x64) |
  | Linux   | [x64](https://aka.ms/dotnet-counters/linux-x64) \| [arm](https://aka.ms/dotnet-counters/linux-arm) \| [arm64](https://aka.ms/dotnet-counters/linux-arm64) \| [musl-x64](https://aka.ms/dotnet-counters/linux-musl-x64) \| [musl-arm64](https://aka.ms/dotnet-counters/linux-musl-arm64) |

## <a name="synopsis"></a>摘要

```console
dotnet-counters [-h|--help] [--version] <command>
```

## <a name="description"></a>描述

`dotnet-counters` 是一个性能监视工具，用于临时运行状况监视和初级性能调查。 它可以观察通过 <xref:System.Diagnostics.Tracing.EventCounter> API 发布的性能计数器值。 例如，可以快速监视 CPU 使用情况或 .NET Core 应用程序中引发的异常率，以了解在使用 `PerfView` 或 `dotnet-trace` 深入调查更严重的性能问题之前是否有任何可疑操作。

## <a name="options"></a>选项

- **`--version`**

  显示 dotnet-counters 实用工具的版本。

- **`-h|--help`**

  显示命令行帮助。

## <a name="commands"></a>命令

| 命令                                             |
|-----------------------------------------------------|
| [dotnet-counters collect](#dotnet-counters-collect) |
| [dotnet-counters list](#dotnet-counters-list)       |
| [dotnet-counters monitor](#dotnet-counters-monitor) |
| [dotnet-counters ps](#dotnet-counters-ps)           |

## <a name="dotnet-counters-collect"></a>dotnet-counters collect

定期收集所选计数器的值，并将它们导出为指定的文件格式以进行后续处理。

### <a name="synopsis"></a>摘要

```console
dotnet-counters collect [-h|--help] [-p|--process-id] [-n|--name] [--diagnostic-port] [--refresh-interval] [--counters <COUNTERS>] [--format] [-o|--output] [-- <command>]
```

### <a name="options"></a>选项

- **`-p|--process-id <PID>`**

  要从中收集计数器数据的进程的 ID。

- **`-n|--name <name>`**

  要从中收集计数器数据的进程的名称。

- **`--diagnostic-port`**

  要创建的诊断端口的名称。 请参阅[使用诊断端口](#using-diagnostic-port)，了解如何使用此选项从应用启动时开始监视计数器。

- **`--refresh-interval <SECONDS>`**

  更新显示的计数器之间延迟的秒数

- **`--counters <COUNTERS>`**

  计数器的逗号分隔列表。 计数器可以指定为 `provider_name[:counter_name]`。 如果使用 `provider_name` 时没有限定的计数器列表，则显示来自提供程序的所有计数器。 若要发现提供程序和计数器名称，请使用 [dotnet-counters list](#dotnet-counters-list) 命令。

- **`--format <csv|json>`**

  要导出的格式。 当前可用的格式：csv 和 json。

- **`-o|--output <output>`**

  输出文件的名称。

- `-- <command>`（仅适用于运行 .NET 5.0 或更高版本的目标应用程序）

  在集合配置参数之后，用户可以追加 `--`，后跟一个命令，以启动至少具有 5.0 运行时的 .NET 应用程序。 `dotnet-counters` 将启动一个进程，并收集请求的指标。 这通常用于收集应用程序的启动路径的指标，并可用于诊断或监视在主入口点前后发生的问题。

  > [!NOTE]
  > 使用此选项监视第一个 .NET 5.0 进程，该进程与该工具通信，这意味着如果命令启动多个 .NET 应用程序，它将仅收集第一个应用。 因此，建议在自包含应用程序上使用此选项，或使用 `dotnet exec <app.dll>` 选项。

  > [!NOTE]
  > 通过 dotnet-counters 启动 .NET 可执行文件将重定向其输入/输出，你将无法与其 stdin/stdout 进行交互。 通过 CTRL+C 或 SIGTERM 退出工具将安全地结束该工具和子进程。 如果子进程在工具之前退出，工具也将退出，应可安全查看跟踪。 如果需要使用 stdin/stdout，可以使用 `--diagnostic-port` 选项。 有关详细信息，请参阅[使用诊断端口](#using-diagnostic-port)。

### <a name="examples"></a>示例

- 以 3 秒的刷新间隔时间收集所有计数器的值，并生成 csv 输出文件：

  ```console
  > dotnet-counters collect --process-id 1902 --refresh-interval 3 --format csv

  counter_list is unspecified. Monitoring all counters by default.
  Starting a counter session. Press Q to quit.
  ```

- 将 `dotnet mvc.dll` 作为子进程启动，开始从启动中收集运行时计算器和 ASP.NET Core Hosting 计算器，并将其另存为 JSON 输出：

  ```console
  > dotnet-counters collect --format json --counters System.Runtime,Microsoft.AspNetCore.Hosting -- dotnet mvc.dll
  Starting a counter session. Press Q to quit.
  File saved to counter.json
  ```

## <a name="dotnet-counters-list"></a>dotnet-counters list

显示按提供程序分组的计数器名称和说明的列表。

### <a name="synopsis"></a>摘要

```console
dotnet-counters list [-h|--help]
```

### <a name="example"></a>示例

```console
> dotnet-counters list
Showing well-known counters only. Specific processes may support additional counters.

System.Runtime
    cpu-usage                                    Amount of time the process has utilized the CPU (ms)
    working-set                                  Amount of working set used by the process (MB)
    gc-heap-size                                 Total heap size reported by the GC (MB)
    gen-0-gc-count                               Number of Gen 0 GCs / min
    gen-1-gc-count                               Number of Gen 1 GCs / min
    gen-2-gc-count                               Number of Gen 2 GCs / min
    time-in-gc                                   % time in GC since the last GC
    gen-0-size                                   Gen 0 Heap Size
    gen-1-size                                   Gen 1 Heap Size
    gen-2-size                                   Gen 2 Heap Size
    loh-size                                     LOH Heap Size
    alloc-rate                                   Allocation Rate
    assembly-count                               Number of Assemblies Loaded
    exception-count                              Number of Exceptions / sec
    threadpool-thread-count                      Number of ThreadPool Threads
    monitor-lock-contention-count                Monitor Lock Contention Count
    threadpool-queue-length                      ThreadPool Work Items Queue Length
    threadpool-completed-items-count             ThreadPool Completed Work Items Count
    active-timer-count                           Active Timers Count

Microsoft.AspNetCore.Hosting
    requests-per-second                  Request rate
    total-requests                       Total number of requests
    current-requests                     Current number of requests
    failed-requests                      Failed number of requests
```

> [!NOTE]
> 当有已标记的进程支持 `Microsoft.AspNetCore.Hosting` 计数器时（例如当 ASP.NET Core 应用程序在主机上运行时），将显示这些计数器。

## <a name="dotnet-counters-monitor"></a>dotnet-counters monitor

显示所选计数器的定期刷新值。

### <a name="synopsis"></a>摘要

```console
dotnet-counters monitor [-h|--help] [-p|--process-id] [-n|--name] [--diagnostic-port] [--refresh-interval] [--counters] [-- <command>]
```

### <a name="options"></a>选项

- **`-p|--process-id <PID>`**

  要监视的进程的 ID。

- **`-n|--name <name>`**

  要监视的进程的名称。

- **`--diagnostic-port`**

  要创建的诊断端口的名称。 请参阅[使用诊断端口](#using-diagnostic-port)，了解如何使用此选项从应用启动时开始监视计数器。

- **`--refresh-interval <SECONDS>`**

  更新显示的计数器之间延迟的秒数

- **`--counters <COUNTERS>`**

  计数器的逗号分隔列表。 计数器可以指定为 `provider_name[:counter_name]`。 如果使用 `provider_name` 时没有限定的计数器列表，则显示来自提供程序的所有计数器。 若要发现提供程序和计数器名称，请使用 [dotnet-counters list](#dotnet-counters-list) 命令。

 `-- <command>`（仅适用于运行 .NET 5.0 或更高版本的目标应用程序）

  在集合配置参数之后，用户可以追加 `--`，后跟一个命令，以启动至少具有 5.0 运行时的 .NET 应用程序。 `dotnet-counters` 将启动一个进程，并监视请求的指标。 这通常用于收集应用程序的启动路径的指标，并可用于诊断或监视在主入口点前后发生的问题。

  > [!NOTE]
  > 使用此选项监视第一个 .NET 5.0 进程，该进程与该工具通信，这意味着如果命令启动多个 .NET 应用程序，它将仅收集第一个应用。 因此，建议在自包含应用程序上使用此选项，或使用 `dotnet exec <app.dll>` 选项。

  > [!NOTE]
  > 通过 dotnet-counters 启动 .NET 可执行文件将重定向其输入/输出，你将无法与其 stdin/stdout 进行交互。 通过 CTRL+C 或 SIGTERM 退出工具将安全地结束该工具和子进程。 如果子进程在工具之前退出，工具也将退出，应可安全查看跟踪。 如果需要使用 stdin/stdout，可以使用 `--diagnostic-port` 选项。 有关详细信息，请参阅[使用诊断端口](#using-diagnostic-port)。

### <a name="examples"></a>示例

- 以 3 秒的刷新间隔监视 `System.Runtime` 中的所有计数器：

  ```console
  > dotnet-counters monitor --process-id 1902  --refresh-interval 3 --counters System.Runtime
  Press p to pause, r to resume, q to quit.
      Status: Running

  [System.Runtime]
      % Time in GC since last GC (%)                                 0
      Allocation Rate (B / 1 sec)                                5,376
      CPU Usage (%)                                                  0
      Exception Count (Count / 1 sec)                                0
      GC Fragmentation (%)                                          48.467
      GC Heap Size (MB)                                              0
      Gen 0 GC Count (Count / 1 sec)                                 1
      Gen 0 Size (B)                                                24
      Gen 1 GC Count (Count / 1 sec)                                 1
      Gen 1 Size (B)                                                24
      Gen 2 GC Count (Count / 1 sec)                                 1
      Gen 2 Size (B)                                           272,000
      IL Bytes Jitted (B)                                       19,449
      LOH Size (B)                                              19,640
      Monitor Lock Contention Count (Count / 1 sec)                  0
      Number of Active Timers                                        0
      Number of Assemblies Loaded                                    7
      Number of Methods Jitted                                     166
      POH (Pinned Object Heap) Size (B)                             24
      ThreadPool Completed Work Item Count (Count / 1 sec)           0
      ThreadPool Queue Length                                        0
      ThreadPool Thread Count                                        2
      Working Set (MB)                                              19
  ```

- 仅监视 `System.Runtime` 中的 CPU 使用情况和 GC 堆大小：

  ```console
  > dotnet-counters monitor --process-id 1902 --counters System.Runtime[cpu-usage,gc-heap-size]

  Press p to pause, r to resume, q to quit.
    Status: Running

  [System.Runtime]
      CPU Usage (%)                                 24
      GC Heap Size (MB)                            811
  ```

- 监视用户定义的 `EventSource` 中的 `EventCounter` 值。 有关详细信息，请参阅[教程：使用 .NET Core 中的 EventCounters 衡量性能](event-counter-perf.md)。

  ```console
  > dotnet-counters monitor --process-id 1902 --counters Samples-EventCounterDemos-Minimal

  Press p to pause, r to resume, q to quit.
      request                                      100
  ```

- 启动 `my-aspnet-server.exe` 并从其启动监视加载的程序集的数量（仅限 .NET 5.0 或更高版本）：

  > [!IMPORTANT]
  > 这仅适用于运行 .NET 5.0 或更高版本的应用。

  ```console
  > dotnet-counters monitor --counters System.Runtime[assembly-count] -- my-aspnet-server.exe

  Press p to pause, r to resume, q to quit.
    Status: Running

  [System.Runtime]
      Number of Assemblies Loaded                   24
  ```
  
- 启动 `my-aspnet-server.exe`，以 `arg1` 及 `arg2` 作为命令行参数，并从其启动监视其工作集和 GC 堆大小（仅限 .NET 5.0 或更高版本）：

  > [!IMPORTANT]
  > 这仅适用于运行 .NET 5.0 或更高版本的应用。

  ```console
  > dotnet-counters monitor --counters System.Runtime[working-set,gc-heap-size] -- my-aspnet-server.exe arg1 arg2
  ```

  ```console
  Press p to pause, r to resume, q to quit.
    Status: Running

  [System.Runtime]
      GC Heap Size (MB)                                 39
      Working Set (MB)                                  59
  ```

## <a name="dotnet-counters-ps"></a>dotnet-counters ps

显示可监视的 dotnet 进程的列表。

### <a name="synopsis"></a>摘要

```console
dotnet-counters ps [-h|--help]
```

### <a name="example"></a>示例

```console
> dotnet-counters ps
  
  15683 WebApi     /home/user/repos/WebApi/WebApi
  16324 dotnet     /usr/local/share/dotnet/dotnet
```

## <a name="using-diagnostic-port"></a>使用诊断端口

  > [!IMPORTANT]
  > 这仅适用于运行 .NET 5.0 或更高版本的应用。

诊断端口是 .NET 5 中新增的运行时功能，你可以通过它从应用启动时开始监视或收集计数器。 若要使用 `dotnet-counters` 执行此操作，可以使用以上示例中所述的 `dotnet-counters <collect|monitor> -- <command>`，也可以使用 `--diagnostic-port` 选项。

使用 `dotnet-counters <collect|monitor> -- <command>` 以子进程的形式启动应用程序，是从启动时开始对其进行快速监视的最简单方法。

但是，如果想要更好地控制所监视应用的生存期（例如，仅在前 10 分钟内监视应用并继续执行），或者如果需要使用 CLI 与应用进行交互，则使用 `--diagnostic-port` 选项可以同时控制要监视的目标应用和 `dotnet-counters`。

1. 以下命令使 dotnet-counters 创建一个名为 `myport.sock` 的诊断套接字并等待连接。

    > ```dotnet-cli
    > dotnet-counters collect --diagnostic-port myport.sock
    > ```

    输出：

    > ```bash
    > Waiting for connection on myport.sock
    > Start an application with the following environment variable: DOTNET_DiagnosticPorts=/home/user/myport.sock
    > ```

2. 在单独的控制台中，通过将环境变量 `DOTNET_DiagnosticPorts` 设置为 `dotnet-counters` 输出中的值，启动目标应用程序。

    > ```bash
    > export DOTNET_DiagnosticPorts=/home/user/myport.sock
    > ./my-dotnet-app arg1 arg2
    > ```

    这应该会使 `dotnet-counters` 开始在 `my-dotnet-app` 上收集计数器：

    > ```bash
    > Waiting for connection on myport.sock
    > Start an application with the following environment variable: DOTNET_DiagnosticPorts=myport.sock
    > Starting a counter session. Press Q to quit.
    > ```

    > [!IMPORTANT]
    > 通过 `dotnet run` 启动应用可能会产生问题，因为 dotnet CLI 可能会生成许多子进程，这些子程序不是应用，并且可以在应用之前连接到 `dotnet-counters`，从而导致应用在运行时挂起。 建议直接使用应用的独立版本或使用 `dotnet exec` 来启动应用程序。
