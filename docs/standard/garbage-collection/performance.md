---
title: 垃圾回收和性能
description: 阅读与垃圾回收和内存使用情况相关的问题。 了解如何最大程度地降低垃圾回收对应用程序的影响。
ms.date: 03/30/2017
helpviewer_keywords:
- garbage collection, troubleshooting
- garbage collection, performance
ms.assetid: c203467b-e95c-4ccf-b30b-953eb3463134
ms.openlocfilehash: 15ca3fd06bb607a4f0257b4c5cd62f9c935c6913
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94827537"
---
# <a name="garbage-collection-and-performance"></a>垃圾回收和性能

本主题介绍与垃圾回收和内存使用情况相关的问题。 它解决了关于托管堆的问题，并解释了如何最小化垃圾回收对应用程序的影响。 每个问题具有访问可用来调查问题的过程的链接。

## <a name="performance-analysis-tools"></a>性能分析工具

以下各节介绍了可用于调查内存使用情况和垃圾回收问题的工具。 本主题中稍后提供的[过程](#performance-check-procedures)将引用这些工具。

### <a name="memory-performance-counters"></a>内存性能计数器

可以使用性能计数器来收集性能数据。 有关说明，请参阅[运行时分析](../../framework/debug-trace-profile/runtime-profiling.md)。 如 [.NET 中的性能计数器](../../framework/debug-trace-profile/performance-counters.md)中所述，性能计数器的 .NET CLR 内存类别提供有关垃圾回收器的信息。

### <a name="debugging-with-sos"></a>用 SOS 调试

可以使用 [Windows 调试器 (WinDbg)](/windows-hardware/drivers/debugger/index) 检查托管堆上的对象。

若要安装 WinDbg，请从[下载 Windows 调试工具](/windows-hardware/drivers/debugger/debugger-download-tools)页安装 Windows 调试工具。

### <a name="garbage-collection-etw-events"></a>垃圾回收 ETW 事件

Windows 事件跟踪 (ETW) 是一个跟踪系统，对由 .NET 提供的分析和调试支持提供补充。 从 .NET Framework 4 开始，[垃圾回收 ETW 事件](../../framework/performance/garbage-collection-etw-events.md)将捕获有用信息，用于从统计的角度来分析托管堆。 例如，在将要发生垃圾回收时引发的 `GCStart_V1` 事件提供了以下信息：

- 正在收集哪一代对象。

- 是什么触发了垃圾回收。

- 垃圾回收的类型（并发或非并发）。

ETW 事件日志有效，且不会掩盖与垃圾回收相关的任何性能问题。 一个进程可以通过结合 ETW 事件来提供其自身的事件。 登录后，可以关联应用程序事件和垃圾回收事件，以确定如何以及何时出现堆问题。 例如，服务器应用程序可以在客户端请求开始和结束时提供事件。

### <a name="the-profiling-api"></a>分析 API

公共语言运行时 (CLR) 分析接口将提供有关垃圾回收期间受影响对象的详细信息。 垃圾回收开始和结束时，可以通知探查器。 它可以提供有关托管堆上对象的报告，其中包括每一代对象的标识。 有关详细信息，请参阅[分析概述](../../framework/unmanaged-api/profiling/profiling-overview.md)。

探查器可以提供全面的信息。 但是，复杂的探查器可能会修改应用程序的行为。

### <a name="application-domain-resource-monitoring"></a>应用程序域资源监控

从 .NET Framework 4 开始，应用程序域资源监视 (ARM) 使主机可以通过应用程序域监视 CPU 和内存使用情况。 有关详细信息，请参阅[应用程序域资源监控](app-domain-resource-monitoring.md)。

## <a name="troubleshooting-performance-issues"></a>故障排除性能问题

第一步是[确定问题是否确实为垃圾回收](#IsGC)。 如果确定是，则从以下列表进行选择，以解决该问题。

- [引发内存不足异常](#Issue_OOM)

- [进程占用过多内存](#Issue_TooMuchMemory)

- [垃圾回收器回收对象的速度不够快](#Issue_NotFastEnough)

- [托管堆太零碎](#Issue_Fragmentation)

- [垃圾回收暂停时间太长](#Issue_LongPauses)

- [第 0 代太大](#Issue_Gen0)

- [垃圾回收期间的 CPU 使用率太高](#Issue_HighCPU)

<a name="Issue_OOM"></a>

### <a name="issue-an-out-of-memory-exception-is-thrown"></a>问题：抛出内存不足异常

对于引发的托管 <xref:System.OutOfMemoryException>，存在以下两种合理的情况：

- 虚拟内存不足。

  垃圾回收器按预先确定大小的分段来分配系统内存。 如果分配需要其他段，但在进程的虚拟内存空间中没有剩余的连续可用块了，则托管堆的分配将失败。

- 没有足够的物理内存来分配。

|性能检查|
|------------------------|
|[确定是否已托管内存不足异常。](#OOMIsManaged)<br /><br /> [确定可保留的虚拟内存量。](#GetVM)<br /><br /> [确定是否有足够的物理内存。](#Physical)|

如果确定异常不合法，请使用以下信息与 Microsoft 客户服务和支持联系：

- 带有托管内存不足异常的堆栈。

- 完整内存转储。

- 证明这不是合法内存不足异常的数据包括显示虚拟或物理内存不是问题的数据。

<a name="Issue_TooMuchMemory"></a>

### <a name="issue-the-process-uses-too-much-memory"></a>问题：进程占用过多内存

通常会假设 Windows 任务管理器“性能”选项卡上的内存使用量显示可以指示何时使用了太多内存。 然而，该显示与工作集相关；它不提供有关虚拟内存使用量的信息。

如果确定问题是托管堆引发的，必须测量一段时间的托管堆，以确定模式。

如果确定问题不是托管堆引发的，则必须使用本地调试。

|性能检查|
|------------------------|
|[确定可保留的虚拟内存量。](#GetVM)<br /><br /> [确定托管堆的内存提交量。](#ManagedHeapCommit)<br /><br /> [确定托管堆的内存保留量。](#ManagedHeapReserve)<br /><br /> [确定第 2 代中的大型对象。](#ExamineGen2)<br /><br /> [确定对对象的引用。](#ObjRef)|

<a name="Issue_NotFastEnough"></a>

### <a name="issue-the-garbage-collector-does-not-reclaim-objects-fast-enough"></a>问题：垃圾回收器回收对象的速度不够快

当出现对象好像未按垃圾回收的预期进行回收的情况时，必须确定是否存在任何对这些对象的强引用。

如果没有对包含死对象的一代进行垃圾回收，这表示尚未运行死对象的终结器，你也可能会遇到以上问题。 例如，当正在运行一个单线程单元 (STA) 应用程序并且服务终结器队列的线程不能调用至其中时，可能发生这种问题。

|性能检查|
|------------------------|
|[检查对对象的引用。](#ObjRef)<br /><br /> [确定是否已运行终结器。](#Induce)<br /><br /> [确定是否存在等待终结的对象。](#Finalize)|

<a name="Issue_Fragmentation"></a>

### <a name="issue-the-managed-heap-is-too-fragmented"></a>问题：托管堆太零碎

碎片级别将计算为可用空间占这一代已分配的总内存的比率。 对于第 2 代，可接受的碎片级别不能超过 20%。 因为第 2 代可以变得很大，所以碎片的比率比绝对值更重要。

第 0 代中存在大量可用空间，这不是问题，因为新的对象将在其中进行分配。

碎片始终出现在大型对象堆中，因为它没有进行压缩。 相邻的可用对象会自然地折叠至一个单个的空间，以满足大型对象的分配请求。

在第 1 代和第 2 代中，碎片可能会成为问题。 如果它们在垃圾回收后还有大量的可用空间，则应用程序对象的使用可能需要进行修改，并且应考虑重新评估长期对象的生存期。

固定对象过多可能会增加碎片。 如果碎片太多，则可以固定许多对象。

如果虚拟内存的碎片阻止垃圾回收器添加段，原因可能是下列之一：

- 频繁加载和卸载许多小的程序集。

- 与非托管代码互操作时，保留了太多对 COM 对象的引用。

- 大型暂时性对象的创建会导致大型对象堆频繁分配和释放堆段。

  当承载 CLR 时，应用程序可以请求垃圾回收器保留其片段。 这将减少段分配的频率。 通过使用 [STARTUP_FLAGS 枚举](../../framework/unmanaged-api/hosting/startup-flags-enumeration.md)中的 STARTUP_HOARD_GC_VM 标志来完成。

|性能检查|
|------------------------|
|[确定托管堆中的可用空间量。](#Fragmented)<br /><br /> [确定固定对象的数目。](#Pinned)|

如果认为没有出现碎片的合理原因，请与 Microsoft 客户服务和支持联系。

<a name="Issue_LongPauses"></a>

### <a name="issue-garbage-collection-pauses-are-too-long"></a>问题：垃圾回收暂停时间太长

由于垃圾回收软实时操作，因此应用程序必须能够容忍暂停。 软实时的一个衡量标准是 95% 的操作必须按时完成。

在并发垃圾回收中，允许托管线程在一个回收过程中运行，这意味着暂停时间会非常短。

短暂的垃圾回收（第 0 代和第 1 代）只会持续几毫秒，所以减少暂停时间通常是不可行的。 然而，你可以通过更改应用程序的分配请求的模式，在第 2 代回收中减少暂停。

另一个更准确的方法是使用[垃圾回收 ETW 事件](../../framework/performance/garbage-collection-etw-events.md)。 可以通过为某个事件序列添加时间戳的差异来查找回收的计时。 整个集合序列包括暂停执行引擎、垃圾回收本身以及恢复执行引擎。

可以使用[垃圾回收通知](notifications.md)，确定服务器是否将要进行第 2 代回收，以及将请求重新路由到另一个服务器是否可以减轻任何暂停问题。

|性能检查|
|------------------------|
|[确定垃圾回收中的时长。](#TimeInGC)<br /><br /> [确定导致垃圾回收的原因。](#Triggered)|

<a name="Issue_Gen0"></a>

### <a name="issue-generation-0-is-too-big"></a>问题：第 0 代太大

第 0 代可能在 64 位系统上有更多的对象，尤其是当使用服务器垃圾回收而不是工作站垃圾回收时。 这是因为触发 0 代垃圾回收的阈值在这些环境中更高，且 0 代回收可以变得更大。 触发垃圾回收之前，当应用程序分配更多的内存时，性能将会提高。

<a name="Issue_HighCPU"></a>

### <a name="issue-cpu-usage-during-a-garbage-collection-is-too-high"></a>问题：垃圾回收期间的 CPU 使用率太高

在垃圾回收期间，CPU 的使用率会很高。 如果在垃圾回收中花费大量的处理时间，则回收的数量将过于频繁或回收的持续时间将过长。 托管堆上增加的对象分配率将导致垃圾回收更频繁地发生。 减少分配速率可减少垃圾回收的频率。

可以通过使用 `Allocated Bytes/second` 性能计数器来监视分配速率。 有关更多信息，请参阅 [.NET 中的性能计数器](../../framework/debug-trace-profile/performance-counters.md)。

收集的持续时间是分配后幸存对象数量的主要因素。 如果有许多对象仍需收集，则垃圾回收器必须要检查大量的内存。 压缩幸存对象的工作很耗时。 若要确定回收期间处理对象的数量，请在指定代的垃圾回收结束时，在调试器中设置一个断点。

|性能检查|
|------------------------|
|[确定 CPU 的使用率过高是否由垃圾回收引起。](#HighCPU)<br /><br /> [在垃圾回收结束时，设置一个断点。](#GenBreak)|

## <a name="troubleshooting-guidelines"></a>故障排除指南

本部分介绍在开始调查时应考虑的准则。

### <a name="workstation-or-server-garbage-collection"></a>工作站或服务器垃圾回收

确定是否正在使用正确的垃圾回收类型。 如果应用程序使用多个线程和对象实例，则使用服务器垃圾回收，而不是工作站垃圾回收。 服务器垃圾回收在多个线程上进行操作，而工作站垃圾回收则需要应用程序的多个实例运行它们自己的垃圾回收线程并争取 CPU 时间。

低负载且不常在后台（如服务）执行任务的应用程序，可以在禁用并发垃圾回收的情况下使用工作站垃圾回收。

### <a name="when-to-measure-the-managed-heap-size"></a>何时衡量托管堆的大小

除非使用探查器，否则必须建立一致的测量模式，以有效地诊断性能问题。 若要建立一个计划，请考虑以下几点：

- 如果在第 2 代垃圾回收后测量，则整个托管的堆将不再存在垃圾（死对象）。

- 如果在一个 0 代垃圾回收后立即进行测量，则尚不会收集第 1 代和 2 中的对象。

- 如果在垃圾回收之前立即进行测量，则你将在垃圾回收启动之前，测量尽可能多的分配。

- 在垃圾回收期间进行测量会出现问题，因为垃圾回收器的数据结构对于遍历是无效状态，可能不能提供完整的结果。 这是设计使然。

- 当与并发垃圾回收一起使用工作站垃圾回收时，回收的对象不会进行压缩，因此，堆的大小可能会相同或更大（碎片可以使它看起来更大）。

- 第 2 代上的并发垃圾回收在物理内存加载过高时，将被延迟。

以下过程介绍如何设置一个断点，以便测量托管堆。

<a name="GenBreak"></a>

#### <a name="to-set-a-breakpoint-at-the-end-of-garbage-collection"></a>若要在垃圾回收结束时设置一个断点

- 在加载了 SOS 调试器扩展的 WinDbg 中，键入以下命令：

  **bp mscorwks!WKS::GCHeap::RestartEE "j (dwo(mscorwks!WKS::GCHeap::GcCondemnedGeneration)==2) 'kb';'g'"**

  其中，**GcCondemnedGeneration** 设置为所需的代。 此命令要求私有符号。

  如果在已回收第 2 代对象以进行垃圾回收后执行 **RestartEE**，则此命令会强制中断。

  在服务器垃圾回收中，只有一个线程调用 **RestartEE**，因此在第 2 代垃圾回收期间，此断点只会出现一次。

## <a name="performance-check-procedures"></a>性能检查过程

本部分将介绍下列过程，以避免造成性能问题的原因：

- [确定问题是否由垃圾回收引起。](#IsGC)

- [确定是否已托管内存不足异常。](#OOMIsManaged)

- [确定可保留的虚拟内存量。](#GetVM)

- [确定是否有足够的物理内存。](#Physical)

- [确定托管堆的内存提交量。](#ManagedHeapCommit)

- [确定托管堆的内存保留量。](#ManagedHeapReserve)

- [确定第 2 代中的大型对象。](#ExamineGen2)

- [确定对对象的引用。](#ObjRef)

- [确定是否已运行终结器。](#Induce)

- [确定是否存在等待终结的对象。](#Finalize)

- [确定托管堆中的可用空间量。](#Fragmented)

- [确定固定对象的数目。](#Pinned)

- [确定垃圾回收中的时长。](#TimeInGC)

- [确定触发垃圾回收的原因。](#Triggered)

- [确定 CPU 的使用率过高是否由垃圾回收引起。](#HighCPU)

<a name="IsGC"></a>

### <a name="to-determine-whether-the-problem-is-caused-by-garbage-collection"></a>若要确定问题是否是垃圾回收引起

- 请检查以下两个内存性能计数器：

  - **GC 所占时间百分比**。 显示执行最后一个垃圾回收周期后，执行垃圾回收所用运行时间的百分比。 使用此计数器确定垃圾回收器是否花费太多时间来使托管堆空间可用。 如果垃圾回收所用的时间相对较短，这可能表示托管堆之外存在资源问题。 当涉及并发或后台垃圾回收时，此计数器可能不准确。

  - **已提交的字节总数**。 显示垃圾回收器当前已提交的虚拟内存量。 使用此计数器确定垃圾回收器所占用的内存是否是应用程序所使用的内存的过多部分。

  大多数的内存性能计数器会在每次垃圾回收结束时进行更新。 因此，它们可能不会反映你希望了解的当前情况。

<a name="OOMIsManaged"></a>

### <a name="to-determine-whether-the-out-of-memory-exception-is-managed"></a>若要确定是否已托管内存不足异常

1. 在加载了 SOS 调试器扩展的 WinDbg 或 Visual Studio 调试器中，键入打印异常 (**pe**) 命令：

    **!pe**

    如果已托管异常，<xref:System.OutOfMemoryException> 将显示为异常类型，如以下示例中所示。

    ```console
    Exception object: 39594518
    Exception type: System.OutOfMemoryException
    Message: <none>
    InnerException: <none>
    StackTrace (generated):
    ```

2. 如果输出没有指定异常，则必须确定内存不足异常来自哪个线程。 在调试器中键入以下命令，以显示所有带调用堆栈的线程：

    **~\*kb**

    具有存在异常调用的堆栈的线程会由 `RaiseTheException` 参数进行指示。 这是托管异常对象。

    ```console
    28adfb44 7923918f 5b61f2b4 00000000 5b61f2b4 mscorwks!RaiseTheException+0xa0
    ```

3. 可以使用以下命令来转储嵌套的异常。

    **!pe -nested**

    如果找不到任何异常，则非托管代码将产生内存不足异常。

<a name="GetVM"></a>

### <a name="to-determine-how-much-virtual-memory-can-be-reserved"></a>若要确定可保留的虚拟内存量

- 在加载了 SOS 调试器扩展的 WinDbg 中键入以下命令，以获取最大的可用区域：

  **!address -summary**

  最大可用区域将如以下输出所示进行显示。

  ```console
  Largest free region: Base 54000000 - Size 0003A980
  ```

  在此示例中，最大可用区域的大小大约为 24000 KB（按十六进制形式则为 3A980）。 此区域比垃圾回收器对分段所需的大小要小得多。

  \- 或 -

- 使用 **vmstat** 命令：

  **!vmstat**

  最大可用区域是 MAXIMUM 列中的最大值，如以下输出所示。

  ```console
  TYPE        MINIMUM   MAXIMUM     AVERAGE   BLK COUNT   TOTAL
  ~~~~        ~~~~~~~   ~~~~~~~     ~~~~~~~   ~~~~~~~~~~  ~~~~
  Free:
  Small       8K        64K         46K       36          1,671K
  Medium      80K       864K        349K      3           1,047K
  Large       1,384K    1,278,848K  151,834K  12          1,822,015K
  Summary     8K        1,278,848K  35,779K   51          1,824,735K
  ```

<a name="Physical"></a>

### <a name="to-determine-whether-there-is-enough-physical-memory"></a>若要确定是否有足够的物理内存

1. 则启动 Windows 任务管理器。

2. 在“性能”选项卡上，查看已提交的值。 （在 Windows 7 中，查看“系统组”中的“提交 (KB)”。）

    如果“总数”接近“限值”，则物理内存将不足。

<a name="ManagedHeapCommit"></a>

### <a name="to-determine-how-much-memory-the-managed-heap-is-committing"></a>若要确定托管堆的内存提交量

- 使用 `# Total committed bytes` 内存性能计数器获取托管堆提交的字节数。 垃圾回收器根据需要在某个段上提交区块，但不会全部在同一时间进行。

  > [!NOTE]
  > 请不要使用 `# Bytes in all Heaps` 性能计数器，因为它不表示托管堆的实际内存使用情况。 代的大小包括在此值中，且实际上是其阈值大小，即如果代以对象进行填充，将引发垃圾回收的大小。 因此，此值通常为零。

<a name="ManagedHeapReserve"></a>

### <a name="to-determine-how-much-memory-the-managed-heap-reserves"></a>若要确定托管堆的内存保留量

- 使用 `# Total reserved bytes`内存性能计数器。

  垃圾回收器在段中保留内存，你可以使用 **eeheap** 命令确定段的开始位置。

  > [!IMPORTANT]
  > 尽管可以确定垃圾回收器为每个段分配的内存量，但是段的大小是特定于实现的，并可能会在任何时间（包括在定期更新中）进行更改。 应用程序不应假设特定段的大小或依赖于此大小，也不应尝试配置段分配可用的内存量。

- 在加载了 SOS 调试器扩展的 WinDbg 或 Visual Studio 调试器中，键入以下命令：

  **!eeheap -gc**

  结果如下所示：

  ```console
  Number of GC Heaps: 2
  ------------------------------
  Heap 0 (002db550)
  generation 0 starts at 0x02abe29c
  generation 1 starts at 0x02abdd08
  generation 2 starts at 0x02ab0038
  ephemeral segment allocation context: none
    segment    begin allocated     size
  02ab0000 02ab0038  02aceff4 0x0001efbc(126908)
  Large object heap starts at 0x0aab0038
    segment    begin allocated     size
  0aab0000 0aab0038  0aab2278 0x00002240(8768)
  Heap Size   0x211fc(135676)
  ------------------------------
  Heap 1 (002dc958)
  generation 0 starts at 0x06ab1bd8
  generation 1 starts at 0x06ab1bcc
  generation 2 starts at 0x06ab0038
  ephemeral segment allocation context: none
    segment    begin allocated     size
  06ab0000 06ab0038  06ab3be4 0x00003bac(15276)
  Large object heap starts at 0x0cab0038
    segment    begin allocated     size
  0cab0000 0cab0038  0cab0048 0x00000010(16)
  Heap Size    0x3bbc(15292)
  ------------------------------
  GC Heap Size   0x24db8(150968)
  ```

  由“段”指示的地址是段的起始地址。

<a name="ExamineGen2"></a>

### <a name="to-determine-large-objects-in-generation-2"></a>若要确定第 2 代中的大型对象

- 在加载了 SOS 调试器扩展的 WinDbg 或 Visual Studio 调试器中，键入以下命令：

  **!dumpheap –stat**

  如果托管堆很大，则 **dumpheap** 可能需要一段时间才能完成。

  你可以从输出的最后几行开始分析，因为它们列出了占用了大多数空间的对象。 例如：

  ```console
  2c6108d4   173712     14591808 DevExpress.XtraGrid.Views.Grid.ViewInfo.GridCellInfo
  00155f80      533     15216804      Free
  7a747c78   791070     15821400 System.Collections.Specialized.ListDictionary+DictionaryNode
  7a747bac   700930     19626040 System.Collections.Specialized.ListDictionary
  2c64e36c    78644     20762016 DevExpress.XtraEditors.ViewInfo.TextEditViewInfo
  79124228   121143     29064120 System.Object[]
  035f0ee4    81626     35588936 Toolkit.TlkOrder
  00fcae40     6193     44911636 WaveBasedStrategy.Tick_Snap[]
  791242ec    40182     90664128 System.Collections.Hashtable+bucket[]
  790fa3e0  3154024    137881448 System.String
  Total 8454945 objects
  ```

  所列出的最后一个对象是一个字符串，且占用的空间最多。 可以检查应用程序，以查看如何优化字符串对象。 若要查看 150 到 200 个字节之间的字符串，请键入以下命令：

  **!dumpheap -type System.String -min 150 -max 200**

  如下所示是结果的一个示例。

  ```console
  Address  MT           Size  Gen
  1875d2c0 790fa3e0      152    2 System.String HighlightNullStyle_Blotter_PendingOrder-11_Blotter_PendingOrder-11
  …
  ```

  对 ID 使用整数而非字符串，这样可能会更有效。 如果数千次重复相同的字符串，请考虑字符串暂留。 有关字符串暂留的详细信息，请参阅 <xref:System.String.Intern%2A?displayProperty=nameWithType> 方法的参考主题。

<a name="ObjRef"></a>

### <a name="to-determine-references-to-objects"></a>若要确定对对象的引用

- 在加载了 SOS 调试器扩展的 WinDbg 中，键入以下命令，以列出对对象的引用：

  **!gcroot**

  `-or-`

- 若要确定对特定对象的引用，包括地址：

  **!gcroot 1c37b2ac**

  在堆栈上找到的根可能是误报。 有关详细信息，请参阅命令 `!help gcroot`。

  ```console
  ebx:Root:19011c5c(System.Windows.Forms.Application+ThreadContext)->
  19010b78(DemoApp.FormDemoApp)->
  19011158(System.Windows.Forms.PropertyStore)->
  … [omitted]
  1c3745ec(System.Data.DataTable)->
  1c3747a8(System.Data.DataColumnCollection)->
  1c3747f8(System.Collections.Hashtable)->
  1c376590(System.Collections.Hashtable+bucket[])->
  1c376c98(System.Data.DataColumn)->
  1c37b270(System.Data.Common.DoubleStorage)->
  1c37b2ac(System.Double[])
  Scan Thread 0 OSTHread 99c
  Scan Thread 6 OSTHread 484
  ```

  **gcroot** 命令可能需要很长时间才能完成。 任何不通过垃圾回收进行回收的对象是活动对象。 这意味着，某些根直接或间接地保留于该对象，因此 **gcroot** 应将路径信息返回到该对象。 应检查返回的关系图，并查看仍然引用这些对象的原因。

<a name="Induce"></a>

### <a name="to-determine-whether-a-finalizer-has-been-run"></a>若要确定是否已运行终结器

- 则运行包含以下代码的测试程序：

  ```csharp
  GC.Collect();
  GC.WaitForPendingFinalizers();
  GC.Collect();
  ```

  如果测试解决了此问题，这意味着垃圾回收器未回收对象，因为这些对象的终结器已被挂起。 <xref:System.GC.WaitForPendingFinalizers%2A?displayProperty=nameWithType> 方法将启用这些终结器来完成其任务，并解决问题。

<a name="Finalize"></a>

### <a name="to-determine-whether-there-are-objects-waiting-to-be-finalized"></a>若要确定是否存在等待被终结的对象

1. 在加载了 SOS 调试器扩展的 WinDbg 或 Visual Studio 调试器中，键入以下命令：

    **!finalizequeue**

    查看已准备好进行终结的对象的数目。 如果数目很多，则必须检查这些终结器完全没有进展或进展速度不够快的原因。

2. 若要获取线程的输出，请键入以下命令：

    **threads -special**

    此命令提供如下所示的输出。

    ```console
       OSID     Special thread type
    2    cd0    DbgHelper
    3    c18    Finalizer
    4    df0    GC SuspendEE
    ```

    终结器线程将指示当前正在运行的终结器（如果存在）。 当终结器线程没有运行任何终结器时，则它正在等待一个事件告诉它进行工作。 大多数情况下，你将看到此状态中的终结器线程，因为它在 THREAD_HIGHEST_PRIORITY 处运行，并应快速完成运行终结器（如果存在）。

<a name="Fragmented"></a>

### <a name="to-determine-the-amount-of-free-space-in-the-managed-heap"></a>若要确定托管堆中的可用空间量

- 在加载了 SOS 调试器扩展的 WinDbg 或 Visual Studio 调试器中，键入以下命令：

  **!dumpheap -type Free -stat**

  此命令将显示托管堆上所有可用对象的总大小，如以下示例中所示。

  ```console
  total 230 objects
  Statistics:
        MT    Count    TotalSize Class Name
  00152b18      230     40958584      Free
  Total 230 objects
  ```

- 若要确定第 0 代中的可用空间，请键入以下命令以获取代的内存使用信息：

  **!eeheap -gc**

  该命令将显示类似以下所示的输出。 最后一行将显示暂时段。

  ```console
  Heap 0 (0015ad08)
  generation 0 starts at 0x49521f8c
  generation 1 starts at 0x494d7f64
  generation 2 starts at 0x007f0038
  ephemeral segment allocation context: none
  segment  begin     allocated  size
  00178250 7a80d84c  7a82f1cc   0x00021980(137600)
  00161918 78c50e40  78c7056c   0x0001f72c(128812)
  007f0000 007f0038  047eed28   0x03ffecf0(67103984)
  3a120000 3a120038  3a3e84f8   0x002c84c0(2917568)
  46120000 46120038  49e05d04   0x03ce5ccc(63855820)
  ```

- 计算 0 代使用的空间：

  **?49e05d04-0x49521f8c**

  结果如下所示： 0 代大约为 9 MB。

  ```console
  Evaluate expression: 9321848 = 008e3d78
  ```

- 以下命令将转储 0 代范围内的可用空间：

  **!dumpheap -type Free -stat 0x49521f8c 49e05d04**

  结果如下所示：

  ```console
  ------------------------------
  Heap 0
  total 409 objects
  ------------------------------
  Heap 1
  total 0 objects
  ------------------------------
  Heap 2
  total 0 objects
  ------------------------------
  Heap 3
  total 0 objects
  ------------------------------
  total 409 objects
  Statistics:
        MT    Count TotalSize Class Name
  0015a498      409   7296540      Free
  Total 409 objects
  ```

  此输出显示堆的 0 代部分正在对对象使用 9 MB 的空间并且有 7 MB 可用。 此分析显示了 0 代对碎片的贡献程度。 此堆的使用量应从总量中扣除，作为长期对象所产生的碎片的原因。

<a name="Pinned"></a>

### <a name="to-determine-the-number-of-pinned-objects"></a>若要确定固定对象的数目

- 在加载了 SOS 调试器扩展的 WinDbg 或 Visual Studio 调试器中，键入以下命令：

  **!gchandles**

  显示的统计信息包括固定句柄的数量，如以下示例所示。

  ```console
  GC Handle Statistics:
  Strong Handles:      29
  Pinned Handles:      10
  ```

<a name="TimeInGC"></a>

### <a name="to-determine-the-length-of-time-in-a-garbage-collection"></a>若要确定垃圾回收中的时间

- 检查 `% Time in GC` 内存性能计数器。

  通过使用采样间隔时间来计算值。 因为该计数器在每次垃圾回收结束时进行更新，所以如果在间隔期间没有产生任何回收，则当前的示例将具有与之前的示例相同的值。

  回收时间是通过将采样间隔时间乘以百分比值获取的。

  以下数据显示了为时 8 秒的研究的 4 个采样，彼此间隔 2 秒。 `Gen0`、`Gen1` 和 `Gen2` 列显示了该代的间隔期间发生的垃圾回收数。

  ```console
  Interval    Gen0    Gen1    Gen2    % Time in GC
          1       9       3       1              10
          2      10       3       1               1
          3      11       3       1               3
          4      11       3       1               3
  ```

  当垃圾回收发生时，将不会显示此信息，但可以确定时间间隔中发生的垃圾回收数。 假设最坏的情况下，第 10 个 0 代垃圾回收在第 2 个间隔开始时完成，且第 11 个 0 代垃圾回收在第 5 个间隔结束时完成。 第 10 个和第 11 个垃圾回收结束时之间的时间约为 2 秒钟，并且性能计数器显示为 3%，因此第 11 个 0 代垃圾回收的持续时间为（2 秒 * 3%= 60 毫秒）。

  在此示例中，存在 5 个周期。

  ```console
  Interval    Gen0    Gen1    Gen2     % Time in GC
          1       9       3       1                3
          2      10       3       1                1
          3      11       4       2                1
          4      11       4       2                1
          5      11       4       2               20
  ```

  第 2 个第 2代垃圾回收在第 3 个间隔期间开始并在第 5 个间隔处完成。 假设最坏情况下，最后一次垃圾回收是针对在第 2 个间隔开始时完成的 0 代回收，且第 2 代垃圾回收在第 5 个间隔结束时完成。 因此，第 0 代垃圾回收结束和第 2 代垃圾回收结束之间的时间是 4 秒。 因为 `% Time in GC` 计数器为 20%，所以第 2 代垃圾回收可能使用的最长时间为（4 秒 * 20%= 800 毫秒）。

- 或者，可以通过使用[垃圾回收 ETW 事件](../../framework/performance/garbage-collection-etw-events.md)，确定垃圾回收的时长，并分析此信息以确定垃圾回收的持续时间。

  例如，以下数据显示了一个发生在非并发垃圾回收期间的事件序列。

  ```console
  Timestamp    Event name
  513052        GCSuspendEEBegin_V1
  513078        GCSuspendEEEnd
  513090        GCStart_V1
  517890        GCEnd_V1
  517894        GCHeapStats
  517897        GCRestartEEBegin
  517918        GCRestartEEEnd
  ```

  挂起托管线程花费了 26us (`GCSuspendEEEnd` – `GCSuspendEEBegin_V1`)。

  实际的垃圾回收花费了 4.8 毫秒 (`GCEnd_V1` – `GCStart_V1`)。

  回复执行托管线程花费了 21us (`GCRestartEEEnd` – `GCRestartEEBegin`)。

  以下输出为后台垃圾回收提供了一个示例，并包括进程、线程和事件字段。 （没有显示所有数据。）

  ```console
  timestamp(us)    event name            process    thread    event field
  42504385        GCSuspendEEBegin_V1    Test.exe    4372             1
  42504648        GCSuspendEEEnd         Test.exe    4372
  42504816        GCStart_V1             Test.exe    4372        102019
  42504907        GCStart_V1             Test.exe    4372        102020
  42514170        GCEnd_V1               Test.exe    4372
  42514204        GCHeapStats            Test.exe    4372        102020
  42832052        GCRestartEEBegin       Test.exe    4372
  42832136        GCRestartEEEnd         Test.exe    4372
  63685394        GCSuspendEEBegin_V1    Test.exe    4744             6
  63686347        GCSuspendEEEnd         Test.exe    4744
  63784294        GCRestartEEBegin       Test.exe    4744
  63784407        GCRestartEEEnd         Test.exe    4744
  89931423        GCEnd_V1               Test.exe    4372        102019
  89931464        GCHeapStats            Test.exe    4372
  ```

  42504816 处的 `GCStart_V1` 事件指示此为一个后台垃圾回收，因为最后一个字段是 `1`。 这将变为垃圾回收 No. 102019。

  将发生 `GCStart` 事件，因为在开始后台垃圾回收之前，需要一个暂时垃圾回收。 这将变为垃圾回收 No. 102020。

  在 42514170 处，垃圾回收 No. 102020 结束。 此时，将重新启动托管线程。 这将在触发此后台垃圾回收的线程 4372 上完成。

  在线程 4744 上，发生了一个挂起。 这是唯一一次后台垃圾回收不得不挂起托管线程。 此持续时间为大约 99 毫秒 ((63784407-63685394)/1000)。

  后台垃圾回收的 `GCEnd` 事件位于 89931423。 这意味着后台垃圾回收持续了大约 47 秒 ((89931423-42504816)/1000)。

  托管线程运行时，可以查看发生的任意数量的暂时垃圾回收。

<a name="Triggered"></a>

### <a name="to-determine-what-triggered-a-garbage-collection"></a>若要确定触发垃圾回收的原因

- 在加载了 SOS 调试器扩展的 WinDbg 或 Visual Studio 调试器中，键入以下命令，以显示所有带调用堆栈的线程：

  **~\*kb**

  该命令将显示类似以下所示的输出。

  ```console
  0012f3b0 79ff0bf8 mscorwks!WKS::GCHeap::GarbageCollect
  0012f454 30002894 mscorwks!GCInterface::CollectGeneration+0xa4
  0012f490 79fa22bd fragment_ni!request.Main(System.String[])+0x48
  ```

  如果垃圾回收是操作系统的内存不足通知引起的，则调用堆栈会非常相似，除了线程是终结器线程之外。 终结器线程将获取异步内存不足的通知，并引发垃圾回收。

  如果垃圾回收是内存分配引起的，则堆栈显示如下：

  ```console
  0012f230 7a07c551 mscorwks!WKS::GCHeap::GarbageCollectGeneration
  0012f2b8 7a07cba8 mscorwks!WKS::gc_heap::try_allocate_more_space+0x1a1
  0012f2d4 7a07cefb mscorwks!WKS::gc_heap::allocate_more_space+0x18
  0012f2f4 7a02a51b mscorwks!WKS::GCHeap::Alloc+0x4b
  0012f310 7a02ae4c mscorwks!Alloc+0x60
  0012f364 7a030e46 mscorwks!FastAllocatePrimitiveArray+0xbd
  0012f424 300027f4 mscorwks!JIT_NewArr1+0x148
  000af70f 3000299f fragment_ni!request..ctor(Int32, Single)+0x20c
  0000002a 79fa22bd fragment_ni!request.Main(System.String[])+0x153
  ```

  实时帮助程序 (`JIT_New*`) 最终调用 `GCHeap::GarbageCollectGeneration`。 如果确定第 2 代垃圾回收是分配引起的，则必须确定第 2 代垃圾回收所分配的对象以及如何避免它们。 也就是说，想要确定第 2 代垃圾回收的开始和结束之间的差异，以及引发第 2 代回收的对象。

  例如，在调试器中键入以下命令，以显示第 2 代回收的开始：

  **!dumpheap –stat**

  输出示例（经过删减以显示使用的最多空间的对象）：

  ```console
  79124228    31857      9862328 System.Object[]
  035f0384    25668     11601936 Toolkit.TlkPosition
  00155f80    21248     12256296      Free
  79103b6c   297003     13068132 System.Threading.ReaderWriterLock
  7a747ad4   708732     14174640 System.Collections.Specialized.HybridDictionary
  7a747c78   786498     15729960 System.Collections.Specialized.ListDictionary+DictionaryNode
  7a747bac   700298     19608344 System.Collections.Specialized.ListDictionary
  035f0ee4    89192     38887712 Toolkit.TlkOrder
  00fcae40     6193     44911636 WaveBasedStrategy.Tick_Snap[]
  7912c444    91616     71887080 System.Double[]
  791242ec    32451     82462728 System.Collections.Hashtable+bucket[]
  790fa3e0  2459154    112128436 System.String
  Total 6471774 objects
  ```

  在第 2 代结束时，重复该命令：

  **!dumpheap –stat**

  输出示例（经过删减以显示使用的最多空间的对象）：

  ```console
  79124228    26648      9314256 System.Object[]
  035f0384    25668     11601936 Toolkit.TlkPosition
  79103b6c   296770     13057880 System.Threading.ReaderWriterLock
  7a747ad4   708730     14174600 System.Collections.Specialized.HybridDictionary
  7a747c78   786497     15729940 System.Collections.Specialized.ListDictionary+DictionaryNode
  7a747bac   700298     19608344 System.Collections.Specialized.ListDictionary
  00155f80    13806     34007212      Free
  035f0ee4    89187     38885532 Toolkit.TlkOrder
  00fcae40     6193     44911636 WaveBasedStrategy.Tick_Snap[]
  791242ec    32370     82359768 System.Collections.Hashtable+bucket[]
  790fa3e0  2440020    111341808 System.String
  Total 6417525 objects
  ```

  `double[]` 对象从输出的末尾消失，这意味着它们被回收了。 这些对象大约占 70 MB。 剩余的对象没有太多变化。 因此，这些 `double[]` 对象是第 2 代垃圾回收发生的原因。 下一步是确定 `double[]` 对象存在以及他们最后死亡的原因。 可以询问代码开发人员这些对象的来源，或使用 **gcroot** 命令。

<a name="HighCPU"></a>

### <a name="to-determine-whether-high-cpu-usage-is-caused-by-garbage-collection"></a>若要确定 CPU 的使用率高是否是垃圾回收引起的

- 将 `% Time in GC` 内存性能计数器的值与处理时间相关联。

  如果 `% Time in GC` 值在与处理时间同时达到峰值，则垃圾回收将造成 CPU 使用率过高。 否则，配置应用程序，以查找发生使用率过高的位置。

## <a name="see-also"></a>请参阅

- [垃圾回收](index.md)
