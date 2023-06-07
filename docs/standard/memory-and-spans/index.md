---
title: 内存和跨度
ms.date: 10/03/2018
helpviewer_keywords:
- Memory<T>
- Span<T>
- buffers"
- pipeline processing
ms.openlocfilehash: 4b0464cc81cf0908a907f8305ea4e35b716c18fb
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94830657"
---
# <a name="memory--and-span-related-types"></a>内存和跨度相关类型

从 .NET Core 2.1 开始，.NET 包含多个相互关联的类型，它们表示任意内存的相邻强类型区域。 这些方法包括：

- <xref:System.Span%601?displayProperty=nameWithType>，用于访问连续内存区域的类型。 <xref:System.Span%601> 实例可由一组 `T` 类型、一个 <xref:System.String>、一个使用 [stackalloc](../../csharp/language-reference/operators/stackalloc.md) 分配的缓冲区或一个指向非托管内存的指针提供支持。 由于它必须在堆栈上进行分配，因此存在诸多限制。 例如，类中的字段不能是 <xref:System.Span%601> 类型，跨度类型也不能在异步操作中使用。

- <xref:System.ReadOnlySpan%601?displayProperty=nameWithType>，<xref:System.Span%601> 结构的不可变版本。

- <xref:System.Memory%601?displayProperty=nameWithType>，在托管堆而不是堆栈上分配的内存的相邻区域。 <xref:System.Memory%601> 实例可以由一组 `T` 类型或一个 <xref:System.String> 提供支持。 因为它可以存储在托管堆上，所以 <xref:System.Memory%601> 没有任何 <xref:System.Span%601> 限制。

- <xref:System.ReadOnlyMemory%601?displayProperty=nameWithType>，<xref:System.Memory%601> 结构的不可变版本。

- <xref:System.Buffers.MemoryPool%601?displayProperty=nameWithType>，它将强类型内存块从内存池分配给所有者。 <xref:System.Buffers.IMemoryOwner%601> 实例可以通过调用 <xref:System.Buffers.MemoryPool%601.Rent%2A?displayProperty=nameWithType> 从池中租用，并通过调用 <xref:System.Buffers.MemoryPool%601.Dispose?displayProperty=nameWithType> 将其释放回池中。

- <xref:System.Buffers.IMemoryOwner%601?displayProperty=nameWithType>，表示内存块的所有者并控制其生存期管理。

- <xref:System.Buffers.MemoryManager%601>，一个抽象基类，可用于替换 <xref:System.Memory%601> 的实现，以便 <xref:System.Memory%601> 可以由其他类型（如安全句柄）提供支持。 <xref:System.Buffers.MemoryManager%601> 适用于高级方案。

- <xref:System.ArraySegment%601>，从特定索引开始的特定数量数组元素的包装器。

- <xref:System.MemoryExtensions?displayProperty=nameWithType>，用于将字符串、数组和数组段转换为 <xref:System.Memory%601> 块的扩展方法集合。

> [!NOTE]
> 对于早期框架，<xref:System.Span%601> 和 <xref:System.Memory%601> 在 [System.Memory NuGet 包](https://www.nuget.org/packages/System.Memory/)中提供。

有关更多信息，请参见 <xref:System.Buffers?displayProperty=nameWithType> 命名空间。

## <a name="working-with-memory-and-span"></a>使用内存和跨度

由于内存和跨度相关类型通常用于在处理管道中存储数据，因此开发人员在使用 <xref:System.Span%601>、<xref:System.Memory%601> 和相关类型时要务必遵循一套最佳做法。 [内存\<T>> 和跨度\<T>> 使用准则](memory-t-usage-guidelines.md)中介绍了这些最佳做法。

## <a name="see-also"></a>请参阅

- <xref:System.Memory%601?displayProperty=nameWithType>
- <xref:System.ReadOnlyMemory%601?displayProperty=nameWithType>
- <xref:System.Span%601?displayProperty=nameWithType>
- <xref:System.ReadOnlySpan%601?displayProperty=nameWithType>
- <xref:System.Buffers?displayProperty=nameWithType>
