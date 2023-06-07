---
title: I/O 管道 - .NET
description: 了解如何在 .NET 中有效地使用 I/O 管道，并避免在代码中出现问题。
ms.date: 08/27/2020
helpviewer_keywords:
- Pipelines
- Pipelines I/O
- I/O [.NET], Pipelines
author: rick-anderson
ms.author: riande
ms.openlocfilehash: 508ae0e2b854f81ee639a63063a8f6d73ae84863
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94830631"
---
# <a name="systemiopipelines-in-net"></a>.NET 中的 System.IO.Pipelines

<xref:System.IO.Pipelines> 是一个新库，旨在使在 .NET 中执行高性能 I/O 更加容易。 该库的目标为适用于所有 .NET 实现的 .NET Standard。

<a name="solve"></a>

## <a name="what-problem-does-systemiopipelines-solve"></a>System.IO.Pipelines 解决什么问题

<!-- corner case doesn't MT (machine translate)   -->
分析流数据的应用由样板代码组成，后者由许多专门且不寻常的代码流组成。 样板代码和特殊情况代码很复杂且难以进行维护。

`System.IO.Pipelines` 已构建为：

* 具有高性能的流数据分析功能。
* 减少代码复杂性。

下面的代码是典型的 TCP 服务器，它从客户机接收行分隔的消息（由 `'\n'` 分隔）：

```csharp
async Task ProcessLinesAsync(NetworkStream stream)
{
    var buffer = new byte[1024];
    await stream.ReadAsync(buffer, 0, buffer.Length);

    // Process a single line from the buffer
    ProcessLine(buffer);
}
```

前面的代码有几个问题：

* 单次调用 `ReadAsync` 可能无法接收整条消息（行尾）。
* 忽略了 `stream.ReadAsync` 的结果。 `stream.ReadAsync` 返回读取的数据量。
* 它不能处理在单个 `ReadAsync` 调用中读取多行的情况。
* 它为每次读取分配一个 `byte` 数组。

要解决上述问题，需要进行以下更改：

* 缓冲传入的数据，直到找到新行。
* 分析缓冲区中返回的所有行。
* 该行可能大于 1KB（1024 字节）。 此代码需要调整输入缓冲区的大小，直到找到分隔符后，才能在缓冲区内容纳完整行。

  * 如果调整缓冲区的大小，当输入中出现较长的行时，将生成更多缓冲区副本。
  * 压缩用于读取行的缓冲区，以减少空余。

* 请考虑使用缓冲池来避免重复分配内存。
* 下面的代码解决了其中一些问题：

:::code language="csharp" source="~/samples/snippets/csharp/pipelines/ProcessLinesAsync.cs" id="snippet":::

前面的代码很复杂，不能解决所识别的所有问题。 高性能网络通常意味着编写非常复杂的代码以使性能最大化。 `System.IO.Pipelines` 的设计目的是使编写此类代码更容易。

[!INCLUDE [localized code comments](../../../includes/code-comments-loc.md)]

## <a name="pipe"></a>管道

<xref:System.IO.Pipelines.Pipe> 类可用于创建 `PipeWriter/PipeReader` 对。 写入 `PipeWriter` 的所有数据都可用于 `PipeReader`：

:::code language="csharp" source="~/samples/snippets/csharp/pipelines/Pipe.cs" id="snippet2":::

<a name="pbu"></a>

### <a name="pipe-basic-usage"></a>管道基本用法

:::code language="csharp" source="~/samples/snippets/csharp/pipelines/Pipe.cs" id="snippet":::

有两个循环：

* `FillPipeAsync` 从 `Socket` 读取并写入 `PipeWriter`。
* `ReadPipeAsync` 从 `PipeReader` 读取并分析传入的行。

没有分配显式缓冲区。 所有缓冲区管理都委托给 `PipeReader` 和 `PipeWriter` 实现。 委派缓冲区管理使使用代码更容易集中关注业务逻辑。

在第一个循环中：

* 调用 <xref:System.IO.Pipelines.PipeWriter.GetMemory(System.Int32)?displayProperty=nameWithType> 从基础编写器获取内存。
* 调用 <xref:System.IO.Pipelines.PipeWriter.Advance(System.Int32)?displayProperty=nameWithType> 以告知 `PipeWriter` 有多少数据已写入缓冲区。
* 调用 <xref:System.IO.Pipelines.PipeWriter.FlushAsync%2A?displayProperty=nameWithType> 以使数据可用于 `PipeReader`。

在第二个循环中，`PipeReader` 使用由 `PipeWriter` 写入的缓冲区。 缓冲区来自套接字。 对 `PipeReader.ReadAsync` 的调用：

* 返回包含两条重要信息的 <xref:System.IO.Pipelines.ReadResult>：

  * 以 `ReadOnlySequence<byte>` 形式读取的数据。
  * 布尔值 `IsCompleted`，指示是否已到达数据结尾 (EOF)。

找到行尾 (EOL) 分隔符并分析该行后：

* 该逻辑处理缓冲区以跳过已处理的内容。
* 调用 `PipeReader.AdvanceTo` 以告知 `PipeReader` 已消耗和检查了多少数据。

读取器和编写器循环通过调用 `Complete` 结束。 `Complete` 使基础管道释放其分配的内存。

### <a name="backpressure-and-flow-control"></a>反压和流量控制

理想情况下，读取和分析可协同工作：

* 写入线程使用来自网络的数据并将其放入缓冲区。
* 分析线程负责构造适当的数据结构。

通常，分析所花费的时间比仅从网络复制数据块所用时间更长：

* 读取线程领先于分析线程。
* 读取线程必须减缓或分配更多内存来存储用于分析线程的数据。

为了获得最佳性能，需要在频繁暂停和分配更多内存之间取得平衡。

为解决上述问题，`Pipe` 提供了两个设置来控制数据流：

* <xref:System.IO.Pipelines.PipeOptions.PauseWriterThreshold>：确定在调用 <xref:System.IO.Pipelines.PipeWriter.FlushAsync%2A> 暂停之前应缓冲多少数据。
* <xref:System.IO.Pipelines.PipeOptions.ResumeWriterThreshold>：确定在恢复对 `PipeWriter.FlushAsync` 的调用之前，读取器必须观察多少数据。

![具有 ResumeWriterThreshold 和 PauseWriterThreshold 的图](media/pipelines/resume-pause.png)

<xref:System.IO.Pipelines.PipeWriter.FlushAsync%2A?displayProperty=nameWithType>：

* 当 `Pipe` 中的数据量超过 `PauseWriterThreshold` 时，返回不完整的 `ValueTask<FlushResult>`。
* 低于 `ResumeWriterThreshold` 时，返回完整的 `ValueTask<FlushResult>`。

使用两个值可防止快速循环，如果只使用一个值，则可能发生这种循环。

### <a name="examples"></a>示例

```csharp
// The Pipe will start returning incomplete tasks from FlushAsync until
// the reader examines at least 5 bytes.
var options = new PipeOptions(pauseWriterThreshold: 10, resumeWriterThreshold: 5);
var pipe = new Pipe(options);
```

### <a name="pipescheduler"></a>PipeScheduler

通常在使用 `async` 和 `await` 时，异步代码会在 <xref:System.Threading.Tasks.TaskScheduler> 或当前 <xref:System.Threading.SynchronizationContext> 上恢复。

在执行 I/O 时，对执行 I/O 的位置进行细粒度控制非常重要。 此控件允许高效利用 CPU 缓存。 高效的缓存对于 Web 服务器等高性能应用至关重要。 <xref:System.IO.Pipelines.PipeScheduler> 提供对异步回调运行位置的控制。 默认情况下：

* 使用当前的 <xref:System.Threading.SynchronizationContext>。
* 如果没有 `SynchronizationContext`，它将使用线程池运行回调。

:::code language="csharp" source="~/samples/snippets/csharp/pipelines/Program.cs" id="snippet":::

[PipeScheduler.ThreadPool](xref:System.IO.Pipelines.PipeScheduler.ThreadPool) 是 <xref:System.IO.Pipelines.PipeScheduler> 实现，用于对线程池的回调进行排队。 `PipeScheduler.ThreadPool` 是默认选项，通常也是最佳选项。 [PipeScheduler.Inline](xref:System.IO.Pipelines.PipeScheduler.Inline) 可能会导致意外后果，如死锁。

### <a name="pipe-reset"></a>管道重置

通常重用 `Pipe` 对象即可重置。 若要重置管道，请在 `PipeReader` 和 `PipeWriter` 完成时调用 <xref:System.IO.Pipelines.PipeReader> <xref:System.IO.Pipelines.Pipe.Reset%2A>。

## <a name="pipereader"></a>PipeReader

<xref:System.IO.Pipelines.PipeReader> 代表调用方管理内存。 在调用 <xref:System.IO.Pipelines.PipeReader.ReadAsync%2A?displayProperty=nameWithType> 之后始终调用 <xref:System.IO.Pipelines.PipeReader.AdvanceTo%2A?displayProperty=nameWithType>  。 这使 `PipeReader` 知道调用方何时用完内存，以便可以对其进行跟踪。 从 `PipeReader.ReadAsync` 返回的 `ReadOnlySequence<byte>` 仅在调用 `PipeReader.AdvanceTo` 之前有效。 调用 `PipeReader.AdvanceTo` 后，不能使用 `ReadOnlySequence<byte>`。

`PipeReader.AdvanceTo` 采用两个 <xref:System.SequencePosition> 参数：

* 第一个参数确定消耗的内存量。
* 第二个参数确定观察到的缓冲区数。

将数据标记为“已使用”意味着管道可以将内存返回到底层缓冲池。 将数据标记为“已观察”可控制对 `PipeReader.ReadAsync` 的下一个调用的操作。 将所有内容都标记为“已观察”意味着下次对 `PipeReader.ReadAsync` 的调用将不会返回，直到有更多数据写入管道。 任何其他值都将使对 `PipeReader.ReadAsync` 的下一次调用立即返回并包含已观察到的和未观察到的数据，但不是已被使用的数据  。

### <a name="read-streaming-data-scenarios"></a>读取流数据方案

尝试读取流数据时会出现以下几种典型模式：

* 给定数据流时，分析单条消息。
* 给定数据流时，分析所有可用消息。

以下示例使用 `TryParseMessage` 方法分析来自 `ReadOnlySequence<byte>` 的消息。 `TryParseMessage` 分析单条消息并更新输入缓冲区，以从缓冲区中剪裁已分析的消息。 `TryParseMessage` 不是 .NET 的一部分，它是在以下部分中使用的用户编写的方法。

```csharp
bool TryParseMessage(ref ReadOnlySequence<byte> buffer, out Message message);
```

### <a name="read-a-single-message"></a>读取单条消息

下面的代码从 `PipeReader` 读取一条消息并将其返回给调用方。

:::code language="csharp" source="~/samples/snippets/csharp/pipelines/ReadSingleMsg.cs" id="snippet":::

前面的代码：

* 分析单条消息。
* 更新已使用的 `SequencePosition` 并检查 `SequencePosition` 以指向已剪裁的输入缓冲区的开始。

因为 `TryParseMessage` 从输入缓冲区中删除了已分析的消息，所以更新了两个 `SequencePosition` 参数。 通常，分析来自缓冲区的单条消息时，检查的位置应为以下位置之一：

* 消息的结尾。
* 如果未找到消息，则返回接收缓冲区的结尾。

单条消息案例最有可能出现错误。 将错误的值传递给“已检查”可能会导致内存不足异常或无限循环  。 有关详细信息，请参阅本文中的 [PipeReader 常见问题](#gotchas)部分。

### <a name="reading-multiple-messages"></a>读取多条消息

以下代码从 `PipeReader` 读取所有消息，并在每条消息上调用 `ProcessMessageAsync`。

:::code language="csharp" source="~/samples/snippets/csharp/pipelines/MyConnection1.cs" id="snippet":::

### <a name="cancellation"></a>取消

`PipeReader.ReadAsync`：

* 支持传递 <xref:System.Threading.CancellationToken>。
* 如果在读取挂起期间取消了 `CancellationToken`，则会引发 <xref:System.OperationCanceledException>。
* 支持通过 <xref:System.IO.Pipelines.PipeReader.CancelPendingRead%2A?displayProperty=nameWithType> 取消当前读取操作的方法，这样可以避免引发异常。 调用 `PipeReader.CancelPendingRead` 将导致对 `PipeReader.ReadAsync` 的当前或下次调用返回 <xref:System.IO.Pipelines.ReadResult>，并将 `IsCanceled` 设置为 `true`。 这对于以非破坏性和非异常的方式停止现有的读取循环非常有用。

:::code language="csharp" source="~/samples/snippets/csharp/pipelines/MyConnection.cs" id="snippet":::

<a name="gotchas"></a>

### <a name="pipereader-common-problems"></a>PipeReader 常见问题

* 将错误的值传递给 `consumed` 或 `examined` 可能会导致读取已读取的数据。
* 传递 `buffer.End` 作为检查对象可能会导致以下问题：

  * 数据停止
  * 如果数据未使用，可能最终会出现内存不足 (OOM) 异常。 例如，当一次处理来自缓冲区的单条消息时，可能会出现 `PipeReader.AdvanceTo(position, buffer.End)`。

* 将错误的值传递给 `consumed` 或 `examined` 可能会导致无限循环。 例如，如果 `buffer.Start` 没有更改，则 `PipeReader.AdvanceTo(buffer.Start)` 将导致在下一个对 `PipeReader.ReadAsync` 的调用在新数据到来之前立即返回。
* 将错误的值传递给 `consumed` 或 `examined` 可能会导致无限缓冲（最终导致 OOM）。
* 在调用 `PipeReader.AdvanceTo` 之后使用 `ReadOnlySequence<byte>` 可能会导致内存损坏（在释放之后使用）。
* 未能调用 `PipeReader.Complete/CompleteAsync` 可能会导致内存泄漏。
* 在处理缓冲区之前检查 <xref:System.IO.Pipelines.ReadResult.IsCompleted?displayProperty=nameWithType> 并退出读取逻辑会导致数据丢失。 循环退出条件应基于 `ReadResult.Buffer.IsEmpty` 和 `ReadResult.IsCompleted`。 如果错误执行此操作，可能会导致无限循环。

#### <a name="problematic-code"></a>有问题的代码

❌ **数据丢失**

当 `IsCompleted` 被设置为 `true` 时，`ReadResult` 可能会返回最后一段数据。 在退出读循环之前不读取该数据将导致数据丢失。

[!INCLUDE [pipelines-do-not-use-1](../../../includes/pipelines-do-not-use-1.md)]

:::code language="csharp" source="~/samples/snippets/csharp/pipelines/DoNotUse.cs" id="snippet":::

[!INCLUDE [pipelines-do-not-use-2](../../../includes/pipelines-do-not-use-2.md)]

❌ **无限循环**

如果 `Result.IsCompleted` 是 `true`，则以下逻辑可能会导致无限循环，但缓冲区中永远不会有完整的消息。

[!INCLUDE [pipelines-do-not-use-1](../../../includes/pipelines-do-not-use-1.md)]

:::code language="csharp" source="~/samples/snippets/csharp/pipelines/DoNotUse.cs" id="snippet2":::

[!INCLUDE [pipelines-do-not-use-2](../../../includes/pipelines-do-not-use-2.md)]

下面是另一段具有相同问题的代码。 该代码在检查 `ReadResult.IsCompleted` 之前检查非空缓冲区。 由于该代码位于 `else if` 中，如果缓冲区中没有完整的消息，它将永远循环。

[!INCLUDE [pipelines-do-not-use-1](../../../includes/pipelines-do-not-use-1.md)]

:::code language="csharp" source="~/samples/snippets/csharp/pipelines/DoNotUse.cs" id="snippet3":::

[!INCLUDE [pipelines-do-not-use-2](../../../includes/pipelines-do-not-use-2.md)]

❌ **意外挂起**

在分析单条消息时，如果无条件调用 `PipeReader.AdvanceTo` 而 `buffer.End` 位于 `examined` 位置，则可能导致挂起。 对 `PipeReader.AdvanceTo` 的下次调用将在以下情况下返回：

* 有更多数据写入管道。
* 以及之前未检查过新数据。

[!INCLUDE [pipelines-do-not-use-1](../../../includes/pipelines-do-not-use-1.md)]

:::code language="csharp" source="~/samples/snippets/csharp/pipelines/DoNotUse.cs" id="snippet4":::

[!INCLUDE [pipelines-do-not-use-2](../../../includes/pipelines-do-not-use-2.md)]

❌ **内存不足 (OOM)**

在满足以下条件的情况下，以下代码将保持缓冲，直到发生 <xref:System.OutOfMemoryException>：

* 没有最大消息大小。
* 从 `PipeReader` 返回的数据不会生成完整的消息。 例如，它不会生成完整的消息，因为另一端正在编写一条大消息（例如，一条为 4GB 的消息）。

[!INCLUDE [pipelines-do-not-use-1](../../../includes/pipelines-do-not-use-1.md)]

:::code language="csharp" source="~/samples/snippets/csharp/pipelines/DoNotUse.cs" id="snippet5":::

[!INCLUDE [pipelines-do-not-use-2](../../../includes/pipelines-do-not-use-2.md)]

❌ **内存损坏**

当写入读取缓冲区的帮助程序时，应在调用 `Advance` 之前复制任何返回的有效负载。 下面的示例将返回 `Pipe` 已丢弃的内存，并可能将其重新用于下一个操作（读/写）。

[!INCLUDE [pipelines-do-not-use-1](../../../includes/pipelines-do-not-use-1.md)]

:::code language="csharp" source="~/samples/snippets/csharp/pipelines/DoNotUse.cs" id="snippetMessage":::

:::code language="csharp" source="~/samples/snippets/csharp/pipelines/DoNotUse.cs" id="snippet6":::

[!INCLUDE [pipelines-do-not-use-2](../../../includes/pipelines-do-not-use-2.md)]

## <a name="pipewriter"></a>PipeWriter

<xref:System.IO.Pipelines.PipeWriter> 管理用于代表调用方写入的缓冲区。 `PipeWriter` 实现[`IBufferWriter<byte>`](xref:System.Buffers.IBufferWriter%601)。 `IBufferWriter<byte>` 使得无需额外的缓冲区副本就可以访问缓冲区来执行写入操作。

:::code language="csharp" source="~/samples/snippets/csharp/pipelines/MyPipeWriter.cs" id="snippet":::

之前的代码：

* 使用 <xref:System.IO.Pipelines.PipeWriter.GetMemory%2A> 从 `PipeWriter` 请求至少 5 个字节的缓冲区。
* 将 ASCII 字符串 `"Hello"` 的字节写入返回的 `Memory<byte>`。
* 调用 <xref:System.IO.Pipelines.PipeWriter.Advance%2A> 以指示写入缓冲区的字节数。
* 刷新 `PipeWriter`，以便将字节发送到基础设备。

以前的写入方法使用 `PipeWriter` 提供的缓冲区。 或者，<xref:System.IO.Pipelines.PipeWriter.WriteAsync%2A?displayProperty=nameWithType>：

* 将现有缓冲区复制到 `PipeWriter`。
* 根据需要调用 `GetSpan``Advance`，然后调用 <xref:System.IO.Pipelines.PipeWriter.FlushAsync%2A>。

:::code language="csharp" source="~/samples/snippets/csharp/pipelines/MyPipeWriter.cs" id="snippet2":::

### <a name="cancellation"></a>取消

<xref:System.IO.Pipelines.PipeWriter.FlushAsync%2A> 支持传递 <xref:System.Threading.CancellationToken>。 如果令牌在刷新挂起时被取消，则传递 `CancellationToken` 将导致 `OperationCanceledException`。 `PipeWriter.FlushAsync` 支持通过 <xref:System.IO.Pipelines.PipeWriter.CancelPendingFlush%2A?displayProperty=nameWithType> 取消当前刷新操作而不引发异常的方法。 调用 `PipeWriter.CancelPendingFlush` 将导致对 `PipeWriter.FlushAsync` 或 `PipeWriter.WriteAsync` 的当前或下次调用返回 <xref:System.IO.Pipelines.FlushResult>，并将 `IsCanceled` 设置为 `true`。 这对于以非破坏性和非异常的方式停止暂停刷新非常有用。

<a name="pwcp"></a>

### <a name="pipewriter-common-problems"></a>PipeWriter 常见问题

* <xref:System.IO.Pipelines.PipeWriter.GetSpan%2A> 和 <xref:System.IO.Pipelines.PipeWriter.GetMemory%2A> 返回至少具有请求内存量的缓冲区。 请勿假设确切的缓冲区大小  。
* 无法保证连续的调用将返回相同的缓冲区或相同大小的缓冲区。
* 在调用 <xref:System.IO.Pipelines.PipeWriter.Advance%2A> 之后，必须请求一个新的缓冲区来继续写入更多数据。 不能写入先前获得的缓冲区。
* 如果未完成对 `FlushAsync` 的调用，则调用 `GetMemory` 或 `GetSpan` 将不安全。
* 如果未刷新数据，则调用 `Complete` 或 `CompleteAsync` 可能导致内存损坏。

## <a name="iduplexpipe"></a>IDuplexPipe

<xref:System.IO.Pipelines.IDuplexPipe> 是支持读写的类型的协定。 例如，网络连接将由 `IDuplexPipe` 表示。

 与包含 `PipeReader` 和 `PipeWriter` 的 `Pipe` 不同，`IDuplexPipe` 表示全双工连接的一侧。 这意味着写入 `PipeWriter` 的内容不会从 `PipeReader` 中读取。

## <a name="streams"></a>流

在读取或写入流数据时，通常使用反序列化程序读取数据，并使用序列化程序写入数据。 大多数读取和写入流 API 都有一个 `Stream` 参数。 为了更轻松地与这些现有 API 集成，`PipeReader` 和 `PipeWriter` 公开了一个 <xref:System.IO.Pipelines.PipeReader.AsStream%2A> 方法。 <xref:System.IO.Pipelines.PipeWriter.AsStream%2A> 返回围绕 `PipeReader` 或 `PipeWriter` 的 `Stream` 实现。

### <a name="stream-example"></a>流示例

可使用给定了 <xref:System.IO.Stream> 对象和可选的相应创建选项的静态 `Create` 方法创建 `PipeReader` 和 `PipeWriter` 实例。

<xref:System.IO.Pipelines.StreamPipeReaderOptions> 允许使用以下参数控制 `PipeReader` 实例的创建：

- <xref:System.IO.Pipelines.StreamPipeReaderOptions.BufferSize?displayProperty=nameWithType> 是从池中租用内存时使用的最小缓冲区大小（以字节为单位），默认值为 `4096`。
- <xref:System.IO.Pipelines.StreamPipeReaderOptions.LeaveOpen?displayProperty=nameWithType> 标志确定在 `PipeReader` 完成之后基础流是否保持打开状态，默认值为 `false`。
- <xref:System.IO.Pipelines.StreamPipeReaderOptions.MinimumReadSize?displayProperty=nameWithType> 表示分配新缓冲区之前缓冲区中剩余字节的阈值，默认值为 `1024`。
- <xref:System.IO.Pipelines.StreamPipeReaderOptions.Pool?displayProperty=nameWithType> 是分配内存时使用的 `MemoryPool<byte>`，默认值为 `null`。

<xref:System.IO.Pipelines.StreamPipeWriterOptions> 允许使用以下参数控制 `PipeWriter` 实例的创建：

- <xref:System.IO.Pipelines.StreamPipeWriterOptions.LeaveOpen?displayProperty=nameWithType> 标志确定在 `PipeWriter` 完成之后基础流是否保持打开状态，默认值为 `false`。
- <xref:System.IO.Pipelines.StreamPipeWriterOptions.MinimumBufferSize?displayProperty=nameWithType> 表示从 <xref:System.IO.Pipelines.StreamPipeWriterOptions.Pool> 租用内存时要使用的最小缓冲区大小，默认值为 `4096`。
- <xref:System.IO.Pipelines.StreamPipeWriterOptions.Pool?displayProperty=nameWithType> 是分配内存时使用的 `MemoryPool<byte>`，默认值为 `null`。

> [!IMPORTANT]
> 使用 `Create` 方法创建 `PipeReader` 和 `PipeWriter` 实例时，需要考虑 `Stream` 对象的生存期。 如果在读取器或编写器使用该方法完成操作后，你需要访问流，则需要在创建选项上将 `LeaveOpen` 标志设置为 `true`。 否则，流将关闭。

以下代码演示了使用 `Create` 方法从流中创建 `PipeReader` 和 `PipeWriter` 实例。

:::code language="csharp" source="snippets/pipelines/Program.cs":::

应用程序使用 <xref:System.IO.StreamReader> 将 lorem-ipsum.txt 文件作为流进行读取。 <xref:System.IO.FileStream> 传递给 <xref:System.IO.Pipelines.PipeReader.Create%2A?displayProperty=nameWithType>，后者实例化 `PipeReader` 对象。 然后，控制台应用程序使用 <xref:System.Console.OpenStandardOutput?displayProperty=nameWithType> 将其标准输出流传递到 <xref:System.IO.Pipelines.PipeWriter.Create%2A?displayProperty=nameWithType>。 示例支持[取消](#cancellation)。
