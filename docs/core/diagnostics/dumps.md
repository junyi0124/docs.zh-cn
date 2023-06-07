---
title: 转储 - .NET
description: 介绍 .NET 中的转储。
ms.date: 10/12/2020
ms.openlocfilehash: 56cf4085d10658c828bac39be93eed3f774e00d5
ms.sourcegitcommit: bc293b14af795e0e999e3304dd40c0222cf2ffe4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "96242768"
---
# <a name="dumps"></a>转储

转储是一种文件，其中包含创建进程时该进程的快照，可用于检查应用程序的状态。 当很难将调试程序附加到 .NET 应用程序（如生产或 CI 环境）时，可使用转储来调试该应用程序。 使用转储可以捕获有问题进程的状态，并且可以直接检查状态而无需停止应用程序。

## <a name="collect-dumps"></a>收集转储

可以通过多种方式收集转储，具体取决于运行应用的平台。

> [!NOTE]
> 在容器内收集转储需要 PTRACE 功能，可通过 `--cap-add=SYS_PTRACE` 或 `--privileged` 添加该功能。

> [!NOTE]
> 转储可能包含敏感信息，因为它们可以包含正在运行进程的全部内存。 处理它们时请考虑所有安全限制和指导。

### <a name="collecting-dumps-on-crash"></a>在发生故障时收集转储

可以使用环境变量将应用程序配置为在发生故障时收集转储。 如果想要了解故障原因，这将很有帮助。 例如，在引发异常时捕获转储有助于通过在应用发生故障时检查应用状态来确定问题。

下表显示了可用于将应用程序配置为在发生故障时收集转储的环境变量。

|环境变量|说明|默认值|
|-------|---------|---|
|`COMPlus_DbgEnableMiniDump`|如果设置为 1，则启用核心转储生成。|0|
|`COMPlus_DbgMiniDumpType`|要收集的转储类型。 有关详细信息，请参阅下表|2 (`MiniDumpWithPrivateReadWriteMemory`)|
|`COMPlus_DbgMiniDumpName`|写入转储的文件路径。|`/tmp/coredump.<pid>`|
|`COMPlus_CreateDumpDiagnostics`|如果设置为 1，则启用转储进程的诊断日志记录。|0|

下表显示了可用于 `COMPlus_DbgMiniDumpType`（可指定为值）的所有选项。 例如，将 `COMPlus_DbgMiniDumpType` 设置为 1 意味着在发生故障时将收集 `MiniDumpNormal` 类型的转储。

|值|名称|描述|
|-----|----|-----------|
|1|`MiniDumpNormal`|只包含为进程中的所有现有线程捕获堆栈跟踪所需的信息。 有限的 GC 堆内存和信息。|
|2|`MiniDumpWithPrivateReadWriteMemory`|包含 GC 堆以及为进程中的所有现有线程捕获堆栈跟踪所需的信息。|
|3|`MiniDumpFilterTriage`|只包含为进程中的所有现有线程捕获堆栈跟踪所需的信息。 有限的 GC 堆内存和信息。|
|4|`MiniDumpWithFullMemory`|包含进程中的所有可访问内存。 原始内存数据包含在末尾，因此无需原始内存信息即可直接映射初始结构。 此选项可能会导致文件非常大。|

### <a name="collecting-dumps-at-specific-point-in-time"></a>在特定时间点收集转储

你可能需要在应用尚未发生故障时收集转储。 例如，如果想要检查似乎处于死锁状态的应用程序的状态，则配置环境变量以在发生故障时收集转储将不起作用，因为应用仍在运行。

若要在自己的请求时收集转储，可以使用 `dotnet-dump`，这是一种用于收集和分析转储的 CLI 工具。 若要详细了解如何使用 `dotnet-dump` 来收集转储，请参阅[转储收集和分析实用工具](dotnet-dump.md)。

## <a name="analyze-dumps"></a>分析转储

可以使用 [`dotnet-dump`](dotnet-dump.md) 来分析转储。

## <a name="see-also"></a>请参阅

详细了解如何利用转储来帮助诊断 .NET 应用程序中的问题。

* [调试 Linux 转储](debug-linux-dumps.md)这一教程分步演示了如何调试在 Linux 中收集的转储。

* [调试死锁](debug-deadlock.md)这一教程分步演示了如何使用转储来调试 .NET 应用程序中的死锁。
