---
title: System.Buffers - .NET
ms.date: 12/05/2019
helpviewer_keywords:
- buffers [.NET]
- I/O [.NET], buffers
author: rick-anderson
ms.author: riande
ms.openlocfilehash: afcd6976e6220349fbec370c47b11596a35a81a2
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94823519"
---
# <a name="work-with-buffers-in-net"></a>使用 .NET 中的缓冲区

本文概述了有助于读取跨多个缓冲区运行的数据的类型。 它们主要用于支持 <xref:System.IO.Pipelines.PipeReader> 对象。

## <a name="ibufferwritert"></a>IBufferWriter\<T\>

<xref:System.Buffers.IBufferWriter%601?displayProperty=fullName> 是同步缓冲写入的协定。 在最低级别上，接口：

- 是基本的，不难使用。
- 允许访问 <xref:System.Memory%601> 或 <xref:System.Span%601>。 可以写入 `Memory<T>` 或 `Span<T>`，而且你可以确定写入了多少个 `T` 项。

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet)]

前面的方法：

- 使用 `GetSpan(5)` 从 `IBufferWriter<byte>` 请求至少 5 个字节的缓冲区。
- 将 ASCII 字符串“Hello”的字节写入返回的 `Span<byte>`。
- 调用 <xref:System.Buffers.IBufferWriter%601> 以指示写入缓冲区的字节数。

此写入方法使用 `IBufferWriter<T>` 提供的 `Memory<T>`/`Span<T>` 缓冲区。 或者，可以使用 <xref:System.Buffers.BuffersExtensions.Write%2A> 扩展方法将现有缓冲区复制到 `IBufferWriter<T>`。 `Write` 根据需要调用 `GetSpan`/`Advance`，因此在写入后无需调用 `Advance`：

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet2)]

<xref:System.Buffers.ArrayBufferWriter%601> 是 `IBufferWriter<T>` 的实现，其后备存储是单个连续数组。

### <a name="ibufferwriter-common-problems"></a>IBufferWriter 常见问题

- `GetSpan` 和 `GetMemory` 返回至少具有请求内存量的缓冲区。 请勿假设确切的缓冲区大小。
- 无法保证连续的调用将返回相同的缓冲区或相同大小的缓冲区。
- 在调用 `Advance` 之后，必须请求一个新的缓冲区来继续写入更多数据。 调用 `Advance` 后无法写入先前获取的缓冲区。

## <a name="readonlysequencet"></a>ReadOnlySequence\<T\>

![显示管道中的内存且低于只读内存序列位置的 ReadOnlySequence](media/buffers/ro-sequence.png)

<xref:System.Buffers.ReadOnlySequence%601> 是一个可以表示 `T` 的连续或非连续序列的结构。 它通过以下方法进行构造：

1. 一个 `T[]`
1. 一个 `ReadOnlyMemory<T>`
1. 一对链接列表节点 <xref:System.Buffers.ReadOnlySequenceSegment%601> 和索引，用于表示序列的开始位置和结束位置。

第三种表示形式最值得关注，因为它对 `ReadOnlySequence<T>` 上的各种操作有性能影响：

|表示形式|操作|复杂性|
---|---|---|
|`T[]`/`ReadOnlyMemory<T>`|`Length`|`O(1)`|
|`T[]`/`ReadOnlyMemory<T>`|`GetPosition(long)`|`O(1)`|
|`T[]`/`ReadOnlyMemory<T>`|`Slice(int, int)`|`O(1)`|
|`T[]`/`ReadOnlyMemory<T>`|`Slice(SequencePostion,  SequencePostion)`|`O(1)`|
|`ReadOnlySequenceSegment<T>`|`Length`|`O(1)`|
|`ReadOnlySequenceSegment<T>`|`GetPosition(long)`|`O(number of segments)`|
|`ReadOnlySequenceSegment<T>`|`Slice(int, int)`|`O(number of segments)`|
|`ReadOnlySequenceSegment<T>`|`Slice(SequencePostion, SequencePostion)`|`O(1)`|

由于这种混合表示形式，`ReadOnlySequence<T>` 将索引作为 `SequencePosition`（而不是整数）公开。 `SequencePosition`：

- 是一个不透明的值，该值将索引表示为其起源的 `ReadOnlySequence<T>`。
- 由两个部分组成：整数和对象。 这两个值的含义与 `ReadOnlySequence<T>` 的实现相关联。

### <a name="access-data"></a>访问数据

`ReadOnlySequence<T>` 将数据作为 `ReadOnlyMemory<T>` 的枚举公开。 可以使用基本 foreach 来枚举每个段：

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet3)]

前面的方法搜索特定字节的每个段。 如果需要跟踪每个段的 `SequencePosition`，则 <xref:System.Buffers.ReadOnlySequence%601.TryGet%2A?displayProperty=nameWithType> 更为合适。 下一个示例更改前面的代码，以返回 `SequencePosition` 而不是整数。 返回 `SequencePosition` 的好处是使调用方能够避免第二次扫描以获取特定索引处的数据。

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet4)]

`SequencePosition` 和 `TryGet` 的组合类似于枚举器。 将在每次迭代开始时修改位置字段，使其为 `ReadOnlySequence<T>` 中的每个段的开头。

前面的方法作为 `ReadOnlySequence<T>` 的扩展方法存在。 <xref:System.Buffers.BuffersExtensions.PositionOf%2A> 可用于简化前面的代码：

```csharp
SequencePosition? FindIndexOf(in ReadOnlySequence<byte> buffer, byte data) => buffer.PositionOf(data);
```

#### <a name="process-a-readonlysequencet"></a>处理 ReadOnlySequence\<T\>

处理 `ReadOnlySequence<T>` 可能比较困难，因为数据可能会拆分到序列中的多个段。 为了获得最佳性能，请将代码拆分为两个路径：

- 处理单段情况的快速路径。
- 处理拆分到各个段中的数据的慢速路径。

可以使用多种方法来处理多段序列中的数据：

- 使用 [`SequenceReader<T>`](#sequencereadert)。
- 逐段分析数据，同时跟踪已分析的段内的 `SequencePosition` 和索引。 这样可以避免不必要的分配，但可能效率会很低，尤其是对于小型缓冲区。
- 将 `ReadOnlySequence<T>` 复制到连续数组，并将其视为单个缓冲区：
  - 如果 `ReadOnlySequence<T>` 的大小较小，则可以使用 [stackalloc](../../csharp/language-reference/operators/stackalloc.md) 运算符将数据复制到堆栈分配的缓冲区。
  - 使用 <xref:System.Buffers.ArrayPool%601.Shared%2A?displayProperty=nameWithType> 将 `ReadOnlySequence<T>` 复制到共用的数组。
  - 使用 [`ReadOnlySequence<T>.ToArray()`](xref:System.Buffers.BuffersExtensions.ToArray%2A)。 不建议在热路径中使用这种方法，因为它会在堆上分配新的 `T[]`。

以下示例演示了处理 `ReadOnlySequence<byte>` 的一些常见情况：

##### <a name="process-binary-data"></a>处理二进制数据

以下示例从 `ReadOnlySequence<byte>` 的开头分析 4 字节的大端整数长度。

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet5)]

[!INCLUDE [localized code comments](../../../includes/code-comments-loc.md)]

##### <a name="process-text-data"></a>处理文本数据

如下示例中：

- 查找 `ReadOnlySequence<byte>` 中的第一个换行符 (`\r\n`)，并通过 out 'line' 参数返回该符号。
- 剪裁该行，从输入缓冲区中排除 `\r\n`。

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet6)]

##### <a name="empty-segments"></a>空段

将空段存储在 `ReadOnlySequence<T>` 中是有效的。 显式枚举段时，可能会出现空段：

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet7)]

前面的代码将创建一个包含空段的 `ReadOnlySequence<byte>`，并显示这些空段对各种 API 的影响：

- 包含指向空段的 `SequencePosition` 的 `ReadOnlySequence<T>.Slice` 会保留该段。
- 包含 int 的 `ReadOnlySequence<T>.Slice` 会跳过空段。
- 枚举 `ReadOnlySequence<T>` 会枚举空段。

### <a name="potential-problems-with-readonlysequencet-and-sequenceposition"></a>ReadOnlySequence\<T> 和 SequencePosition 的潜在问题

处理 `ReadOnlySequence<T>`/`SequencePosition` 与常规 `ReadOnlySpan<T>`/`ReadOnlyMemory<T>`/`T[]`/`int` 时，有几个异常的结果：

- `SequencePosition` 是特定 `ReadOnlySequence<T>` 的位置标记，而不是绝对位置。 由于它是相对于特定 `ReadOnlySequence<T>` 的，因此如果在其起源的 `ReadOnlySequence<T>` 之外使用，则没有意义。
- 不能对没有 `ReadOnlySequence<T>` 的 `SequencePosition` 执行算术运算。 这意味着，执行 `position++` 等基本操作将以 `ReadOnlySequence<T>.GetPosition(position, 1)` 的形式写入。
- `GetPosition(long)` 不支持负索引。 这意味着，如果没有遍历所有段，就无法获取倒数第二个字符。
- 无法比较两个 `SequencePosition`，这使得难以：
  - 了解一个位置是否大于或小于另一个位置。
  - 编写一些分析算法。
- `ReadOnlySequence<T>` 大于对象引用，并且应尽可能通过 [in](../../csharp/language-reference/keywords/in-parameter-modifier.md) 或 [ref](../../csharp/language-reference/keywords/ref.md) 进行传递。 通过 `in` 或 `ref` 传递 `ReadOnlySequence<T>` 可减少[结构](../../csharp/language-reference/builtin-types/struct.md)的复制。
- 空段：
  - 在 `ReadOnlySequence<T>` 中有效。
  - 可能会在使用 `ReadOnlySequence<T>.TryGet` 方法进行循环访问时出现。
  - 可能会在结合使用 `ReadOnlySequence<T>.Slice()` 方法与 `SequencePosition` 对象来对序列进行切片时出现。

## <a name="sequencereadert"></a>SequenceReader\<T\>

<xref:System.Buffers.SequenceReader%601>：

- 是 .NET Core 3.0 中引入的一种新类型，用于简化 `ReadOnlySequence<T>` 的处理。
- 统一了单段 `ReadOnlySequence<T>` 和多段 `ReadOnlySequence<T>` 之间的差异。
- 提供用于读取二进制数据和文本数据（`byte` 和 `char`，不一定拆分到各个段）的帮助程序。

提供用于处理二进制数据和带分隔符的数据的内置方法。 以下部分演示了与 `SequenceReader<T>` 相同的方法：

### <a name="access-data"></a>访问数据

`SequenceReader<T>` 具有用于直接枚举 `ReadOnlySequence<T>` 内的数据的方法。 以下代码是一次处理 `ReadOnlySequence<byte>` 和 `byte` 的示例：

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet8)]

`CurrentSpan` 公开了当前段的 `Span`，这类似于在方法中手动完成的操作。

### <a name="use-position"></a>使用位置

以下代码是使用 `SequenceReader<T>` 实现 `FindIndexOf` 的示例：

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet9)]

### <a name="process-binary-data"></a>处理二进制数据

以下示例从 `ReadOnlySequence<byte>` 的开头分析 4 字节的大端整数长度。

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet11)]

### <a name="process-text-data"></a>处理文本数据

[!code-csharp[](~/samples/snippets/csharp/buffers/MyClass.cs?name=snippet10)]

### <a name="sequencereadert-common-problems"></a>SequenceReader\<T\> 常见问题

- 由于 `SequenceReader<T>` 是可变结构，因此应始终通过[引用](../../csharp/language-reference/keywords/ref.md)进行传递。
- `SequenceReader<T>` 是[引用结构](../../csharp/language-reference/builtin-types/struct.md#ref-struct)，因此只能在同步方法中使用，不能存储在字段中。 有关详细信息，请参阅[编写安全有效的 C# 代码](../../csharp/write-safe-efficient-code.md)。
- `SequenceReader<T>` 已进行了优化，可用作只进读取器。 `Rewind` 适用于无法利用其他 `Read`、`Peek` 和 `IsNext` API 来解决的小型备份。
