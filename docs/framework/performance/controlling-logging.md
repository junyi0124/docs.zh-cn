---
title: 控制 .NET Framework 日志记录
description: 使用 Windows 事件跟踪 (ETW) 控制 .NET 日志记录并记录公共语言运行时 (CLR) 事件。 使用 Logman、Tracerpt 和 Xperf 等工具。
ms.date: 03/30/2017
helpviewer_keywords:
- CLR ETW events, logging
ms.assetid: ce13088e-3095-4f0e-9f6b-fad30bbd3d41
ms.openlocfilehash: bce5ea41149dc3b19106031fae202872dd8a8fb5
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90553800"
---
# <a name="controlling-net-framework-logging"></a>控制 .NET Framework 日志记录

可以使用 Windows 事件跟踪 (ETW) 来记录公共语言运行时 (CLR) 事件。 可以使用以下工具来创建和查看跟踪：

- Windows 操作系统附带的 [Logman](/windows-server/administration/windows-commands/logman) 和 [Tracerpt](/windows-server/administration/windows-commands/tracerpt_1) 命令行工具。

- [Windows 性能工具包](/windows-hardware/test/wpt/)中的 [Xperf](/windows-hardware/test/wpt/xperf-command-line-reference) 工具。 有关 Xperf 的详细信息，请参阅 [Windows 性能博客](/archive/blogs/pigscanfly/)。

若要捕获 CLR 事件信息，必须在计算机上安装 CLR 提供程序。 若要确认该提供程序已安装，请在命令提示符处键入 `logman query providers`。 将显示提供程序的列表。 此列表应包含与 CLR 提供程序对应的项，如下所示。

```output
Provider                                 GUID
-------------------------------------------------------------------------------
.NET Common Language Runtime    {E13C0D23-CCBC-4E12-931B-D9CC2EEE27E4}.
```

如果未列出 CLR 提供程序，则可以通过使用 Windows [Wevtutil](/windows-server/administration/windows-commands/wevtutil) 命令行工具在 Windows Vista 和更高版本的操作系统上安装该提供程序。 以管理员身份打开“命令提示符”窗口。 将提示目录更改为 .NET Framework 4 文件夹 (%WINDIR%\Microsoft.NET\Framework [64] \v4. \<.NET version>\ ) 。 此文件夹包含 CLR-ETW.man 文件。 在命令提示符处，键入以下命令来安装 CLR 提供程序：

`wevtutil im CLR-ETW.man`

## <a name="capturing-clr-etw-events"></a>捕获 CLR ETW 事件

可使用 [Logman](/windows-server/administration/windows-commands/logman) 和 [Xperf](/windows-hardware/test/wpt/xperf-command-line-reference) 命令行工具来捕获 ETW 事件，并使用 [Tracerpt](/windows-server/administration/windows-commands/tracerpt_1) 和 [Xperf](/windows-hardware/test/wpt/xperf-command-line-reference) 工具来解码跟踪事件。

若要启用日志记录，用户必须指定以下三项：

- 进行通信的提供程序。

- 一个表示一组关键字的 64 位数。 每个关键字都表示提供程序可启用的一组事件。 此数字表示要启用的关键字的组合集。

- 一个表示进行记录的级别（详细信息）的较小数。 级别 1 表示最简明，级别 5 表示最详细。 级别 0 是默认值，其含义将因提供程序而异。

### <a name="to-capture-clr-etw-events-using-logman"></a>使用 Logman 捕获 CLR ETW 事件

1. 在命令提示符处，键入：

     `logman start clrevents -p {e13c0d23-ccbc-4e12-931b-d9cc2eee27e4} 0x1CCBD 0x5 -ets -ct perf`

     其中：

    - `-p` 参数标识提供程序 GUID。

    - `0x1CCBD` 指定将引发的事件的类别。

    - `0x5` 设置记录的级别（在本例中为详细级别 (5)）。

    - `-ets` 参数指示 Logman 将命令发送给事件跟踪会话。

    - `-ct perf` 参数指定 `QueryPerformanceCounter` 函数将用于记录每个事件的时间戳。

2. 若要停止记录事件，请键入：

     `logman stop clrevents -ets`

     此命令创建一个名为 clrevents.etl 的二进制跟踪文件。

### <a name="to-capture-clr-etw-events-using-xperf"></a>使用 Xperf 捕获 CLR ETW 事件

1. 在命令提示符处，键入：

     `xperf -start clr -on e13c0d23-ccbc-4e12-931b-d9cc2eee27e4:0x1CCBD:5 -f clrevents.etl`

     其中 GUID 是 CLR ETW 提供程序 GUID，`0x1CCBD:5` 指跟踪级别 5（详细）及以下级别的所有事件。

2. 若要停止跟踪，请键入：

     `Xperf -stop clr`

     此命令创建一个名为 clrevents.etl 的跟踪文件。

## <a name="viewing-clr-etw-events"></a>查看 CLR ETW 事件

使用下面列出的命令来查看 CLR ETW 事件。 有关事件的说明，请参见 [CLR ETW 事件](clr-etw-events.md)。

### <a name="to-view-clr-etw-events-using-tracerpt"></a>使用 Tracerpt 查看 CLR ETW 事件

- 在命令提示符处，键入：

     `tracerpt clrevents.etl`

     此命令创建两个文件：dumpfile.xml 和 summary.txt。 dumpfile.xml 文件列出所有事件，summary.txt 提供事件的摘要。

### <a name="to-view-clr-etw-events-using-xperf"></a>使用 Xperf 查看 CLR ETW 事件

- 在命令提示符处，键入：

     `xperf clrevents.etl`

     此命令打开 Xperf ETL 文件查看器。 在此查看器中，CLR 事件将显示在“一般事件”视图中****。 要显示按类型分类的事件的数据网格，请选择此视图中的时间区域，然后右键单击并选择“摘要”****。

### <a name="to-convert-the-etl-file-to-a-comma-separated-value-file"></a>将 .etl 文件转换为逗号分隔值文件

- 在命令提示符处，键入：

     `xperf -i clrevents.etl -f clrevents.csv`

     此命令可使 XPerf 将事件转储为可以查看的逗号分隔值 (CSV) 文件。 由于各个事件具有不同的字段，因此该 CSV 文件中的数据之前有多个标题行。 每行的第一个字段为事件类型，它指示应使用哪一个标题来确定字段的其余部分。

## <a name="see-also"></a>请参阅

- [Windows 性能工具包](/windows-hardware/test/wpt/)
- [公共语言运行时中的 ETW 事件](etw-events-in-the-common-language-runtime.md)
