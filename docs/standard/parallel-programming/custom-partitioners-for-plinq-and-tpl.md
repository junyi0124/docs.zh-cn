---
title: PLINQ 和 TPL 的自定义分区程序
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, partitioners
ms.assetid: 96153688-9a01-47c4-8430-909cee9a2887
ms.openlocfilehash: 2268df2eb5cae4dcd7adde491b42c86c546aa1fc
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94829227"
---
# <a name="custom-partitioners-for-plinq-and-tpl"></a>PLINQ 和 TPL 的自定义分区程序

若要并行执行对数据源的操作，关键步骤之一是，将数据源分区  成多个部分，以供多个线程同时访问。 PLINQ 和任务并行库 (TPL) 提供了默认分区程序，在用户编写并行查询或 <xref:System.Threading.Tasks.Parallel.ForEach%2A> 循环时透明运行。 对于更高级的方案，可以插入自己的分区程序。

## <a name="kinds-of-partitioning"></a>分区的种类

对数据源进行分区的方法有很多种。 最高效的方法是，多个线程一起协作，共同处理原始源序列，而不是将数据源实际分割成多个子序列。 对于长度提前已知的数组和其他索引源（如 <xref:System.Collections.IList> 集合），范围分区  是最简单的分区种类。 每个线程都会收到唯一起始和结束索引，这样就可以处理范围内的数据源，而又不会覆盖其他任何线程或被其他任何线程覆盖。 范围分区涉及的唯一开销是，创建范围这项初始工作；之后就无需执行其他任何同步工作了。 因此，只要工作负载是均分的，就可以确保实现良好性能。 范围分区的缺点是，即使某线程提前完成，也无法帮助其他线程完成工作。

对于长度未知的链接列表或其他集合，可以使用区块分区  。 在区块分区中，并行循环或查询中的每个线程或任务都会使用并处理一个区块中的若干源元素，再返回检索其他元素。 分区程序可确保所有元素均已分发，且没有重复项。 区块可为任意大小。 例如，[如何：实现动态分区](how-to-implement-dynamic-partitions.md)中展示的分区程序创建的区块就只包含一个元素。 只要区块不是太大，这类分区就一定会执行负载均衡，因为向线程分配的元素不是预先确定的。 不过，每当线程需要获取其他区块时，分区程序就要承担一次同步开销。 在这种情况下产生的同步量与区块大小成反比。

通常情况下，范围分区只有在以下情况下才会更快：委托的执行时间为小到中等，数据源有大量元素，且每个分区的工作总量大致相等。 因此，大多数情况下，区块分区通常更快。 如果数据源有少量元素或委托的执行时间较长，那么区块分区和范围分区的性能大致相同。

TPL 分区程序还支持动态数量的分区。 也就是说，可以在 <xref:System.Threading.Tasks.Parallel.ForEach%2A> 循环生成新任务时（举个例子）快速创建分区。 借助此功能，分区程序可以与循环本身一起缩放。 动态分区程序也一定会执行负载均衡。 创建自定义分区程序时，必须支持可通过 <xref:System.Threading.Tasks.Parallel.ForEach%2A> 循环使用的动态分区。

### <a name="configuring-load-balancing-partitioners-for-plinq"></a>配置 PLINQ 负载均衡分区程序

借助 <xref:System.Collections.Concurrent.Partitioner.Create%2A?displayProperty=nameWithType> 方法的一些重载，可以为数组或 <xref:System.Collections.IList> 源创建分区程序，并指定是否应尝试均衡各线程的工作负载。 如果分区程序被配置为执行负载均衡，那么使用的就是区块分区，元素会根据请求以小区块的形式分配到每个分区。 这种方法有助于确保在整个循环或查询完成前，所有分区都有元素可供处理。 附加重载可用于提供任何 <xref:System.Collections.IEnumerable> 源的负载均衡分区。

负载均衡通常要求分区相对频繁地从分区程序请求获取元素。 相比之下，执行静态分区的分区程序可以使用范围分区或区块分区，将元素一次性全部分配给每个分区程序。 虽然这样做产生的开销少于负载均衡，但如果一个线程的工作量最终大大多于其他线程，那么执行时间可能就会变长。 默认情况下，如果传入的是 IList 或数组，PLINQ 始终都会使用不执行负载均衡的范围分区。 若要为 PLINQ 启用负载均衡，请使用 `Partitioner.Create` 方法，如下面的示例所示。

[!code-csharp[TPL_Partitioners#02](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_partitioners/cs/partitioners.cs#02)]
[!code-vb[TPL_Partitioners#02](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_partitioners/vb/partitionsnippets_vb.vb#02)]

在任何给定方案中确定是否使用负载均衡的最佳方式是，在有代表性的负载和计算机配置下，试验并度量操作需要多长时间才能完成。 例如，如果是只有几个内核的多核计算机，静态分区可以让速度显著提升；但如果是内核相对较多的计算机，静态分区可能会导致速度降低。

下表列出了 <xref:System.Collections.Concurrent.Partitioner.Create%2A> 方法的可用重载。 这些分区程序不仅限于在 PLINQ 或 <xref:System.Threading.Tasks.Task> 中使用。 还可用于任何自定义并行构造。

|重载|使用负载均衡|
|--------------|-------------------------|
|<xref:System.Collections.Concurrent.Partitioner.Create%60%601%28System.Collections.Generic.IEnumerable%7B%60%600%7D%29>|Always|
|<xref:System.Collections.Concurrent.Partitioner.Create%60%601%28%60%600%5B%5D%2CSystem.Boolean%29>|将布尔参数指定为 true 时|
|<xref:System.Collections.Concurrent.Partitioner.Create%60%601%28System.Collections.Generic.IList%7B%60%600%7D%2CSystem.Boolean%29>|将布尔参数指定为 true 时|
|<xref:System.Collections.Concurrent.Partitioner.Create%28System.Int32%2CSystem.Int32%29>|Never|
|<xref:System.Collections.Concurrent.Partitioner.Create%28System.Int32%2CSystem.Int32%2CSystem.Int32%29>|Never|
|<xref:System.Collections.Concurrent.Partitioner.Create%28System.Int64%2CSystem.Int64%29>|Never|
|<xref:System.Collections.Concurrent.Partitioner.Create%28System.Int64%2CSystem.Int64%2CSystem.Int64%29>|Never|

### <a name="configuring-static-range-partitioners-for-parallelforeach"></a>配置 Parallel.ForEach 静态范围分区程序

在 <xref:System.Threading.Tasks.Parallel.For%2A> 循环中，循环的主体作为委托提供给方法。 调用此委托的成本与调用虚拟方法大致相同。 在某些情况下，并行循环的主体可能非常小，这就会导致对每个循环迭代调用委托的成本变得十分高昂。 在这种情况下，可以使用 <xref:System.Collections.Concurrent.Partitioner.Create%2A> 重载之一，对数据源元素创建范围分区的 <xref:System.Collections.Generic.IEnumerable%601>。 然后，可以将此范围集合传递到主体包含常规 `for` 循环的 <xref:System.Threading.Tasks.Parallel.ForEach%2A> 方法。 这种方法的优势在于，委托调用成本在每个范围内只产生一次，而不是每个元素都产生一次。 下面的示例展示了基本模式。

[!code-csharp[TPL_Partitioners#01](../../../samples/snippets/csharp/VS_Snippets_Misc/tpl_partitioners/cs/partitioner01.cs#01)]
[!code-vb[TPL_Partitioners#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/tpl_partitioners/vb/partitionercreate01.vb#01)]

循环中的每个线程都会收到自己的 <xref:System.Tuple%602>，其中包含指定子范围中的起始和结束索引值。 内循环 `for` 使用 `fromInclusive` 和 `toExclusive` 值直接循环访问数组或 <xref:System.Collections.IList>。

借助 <xref:System.Collections.Concurrent.Partitioner.Create%2A> 重载之一，可以指定分区大小和分区数量。 此重载适用于以下情况：每个元素的工作量很少，甚至每个元素调用一个虚拟方法都会对性能产生显著影响。

## <a name="custom-partitioners"></a>自定义分区程序

在某些情况下，实现自己的分区程序可能是值得尝试的，或甚至有必要这样做。 例如，假设有一个自定义集合类，可以根据对类内部结构的了解情况进行更有效的分区（与默认分区程序相比）。 或者，不妨根据对源集合中不同位置的元素处理时长的了解情况，创建不同大小的范围分区。

若要创建基本自定义分区程序，请从 <xref:System.Collections.Concurrent.Partitioner%601?displayProperty=nameWithType> 派生类，并重写虚拟方法，如下表所述。

|||
|-|-|
|<xref:System.Collections.Concurrent.Partitioner%601.GetPartitions%2A>|此方法由主线程调用一次，并返回 IList(IEnumerator(TSource))。 循环或查询中的每个工作线程都可以对列表调用 `GetEnumerator`，以在不同的分区中检索 <xref:System.Collections.Generic.IEnumerator%601>。|
|<xref:System.Collections.Concurrent.Partitioner%601.SupportsDynamicPartitions%2A>|如果实现 <xref:System.Collections.Concurrent.Partitioner%601.GetDynamicPartitions%2A>，返回 `true`；否则，返回 `false`。|
|<xref:System.Collections.Concurrent.Partitioner%601.GetDynamicPartitions%2A>|如果 <xref:System.Collections.Concurrent.Partitioner%601.SupportsDynamicPartitions%2A> 是 `true`，可以视需要选择调用此方法（而不是 <xref:System.Collections.Concurrent.Partitioner%601.GetPartitions%2A>）。|

如果结果必须可排序，或需要对元素进行索引访问，请从 <xref:System.Collections.Concurrent.OrderablePartitioner%601?displayProperty=nameWithType> 派生类，并重写虚拟方法，如下表所述。

|||
|-|-|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.GetPartitions%2A>|此方法由主线程调用一次，并返回 `IList(IEnumerator(TSource))`。 循环或查询中的每个工作线程都可以对列表调用 `GetEnumerator`，以在不同的分区中检索 <xref:System.Collections.Generic.IEnumerator%601>。|
|<xref:System.Collections.Concurrent.Partitioner%601.SupportsDynamicPartitions%2A>|如果实现 <xref:System.Collections.Concurrent.OrderablePartitioner%601.GetDynamicPartitions%2A>，返回 `true`；否则，返回 false。|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.GetDynamicPartitions%2A>|这通常直接调用 <xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderableDynamicPartitions%2A>。|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderableDynamicPartitions%2A>|如果 <xref:System.Collections.Concurrent.Partitioner%601.SupportsDynamicPartitions%2A> 是 `true`，可以视需要选择调用此方法（而不是 <xref:System.Collections.Concurrent.Partitioner%601.GetPartitions%2A>）。|

下表详细介绍了三种负载均衡分区程序是如何实现 <xref:System.Collections.Concurrent.OrderablePartitioner%601> 类的。

|方法/属性|IList/数组（不执行负载均衡）|IList/数组（执行负载均衡）|IEnumerable|
|----------------------|-------------------------------------------|----------------------------------------|-----------------|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderablePartitions%2A>|使用范围分区|使用更适合列表的区块分区（partitionCount 已指定）|通过创建静态数量的分区来使用区块分区。|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderableDynamicPartitions%2A?displayProperty=nameWithType>|抛出不支持异常|使用更适合列表和动态分区的区块分区|通过创建动态数量的分区来使用区块分区。|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.KeysOrderedInEachPartition%2A>|返回 `true`|返回 `true`|返回 `true`|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.KeysOrderedAcrossPartitions%2A>|返回 `true`|返回 `false`|返回 `false`|
|<xref:System.Collections.Concurrent.OrderablePartitioner%601.KeysNormalized%2A>|返回 `true`|返回 `true`|返回 `true`|
|<xref:System.Collections.Concurrent.Partitioner%601.SupportsDynamicPartitions%2A>|返回 `false`|返回 `true`|返回 `true`|

### <a name="dynamic-partitions"></a>动态分区

若要在 <xref:System.Threading.Tasks.Parallel.ForEach%2A> 方法中使用分区程序，必须能够返回动态数量的分区。 也就是说，分区程序可以在循环执行期间随时按需提供新分区的枚举器。 这基本上意味着，每当循环添加新并行任务时，就会请求获取此任务的新分区。 如果要求数据必须可排序，请从 <xref:System.Collections.Concurrent.OrderablePartitioner%601?displayProperty=nameWithType> 派生类，这样就可以为所有分区中的每个项都分配一个唯一索引。

有关详细信息及示例，请参阅[如何：实现动态分区](how-to-implement-dynamic-partitions.md)。

### <a name="contract-for-partitioners"></a>分区程序合同

实现自定义分区程序时，请遵循以下指南，它们有助于确保与 PLINQ 和 TPL 中的 <xref:System.Threading.Tasks.Parallel.ForEach%2A> 进行正确交互：

- 如果调用 <xref:System.Collections.Concurrent.Partitioner%601.GetPartitions%2A> 时 `partitionsCount` 参数值等于或小于零，抛出 <xref:System.ArgumentOutOfRangeException>。 虽然 PLINQ 和 TPL 绝不会传入等于 0 的 `partitionCount`，但仍建议防范这种可能性。

- <xref:System.Collections.Concurrent.Partitioner%601.GetPartitions%2A> 和 <xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderablePartitions%2A> 应始终返回分区的 `partitionsCount` 数。 如果分区程序用尽数据，且无法根据请求创建任意多个分区，方法应为剩余的每个分区返回空枚举器。 否则，PLINQ 和 TPL 都会抛出 <xref:System.InvalidOperationException>。

- <xref:System.Collections.Concurrent.Partitioner%601.GetPartitions%2A>、<xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderablePartitions%2A>、<xref:System.Collections.Concurrent.Partitioner%601.GetDynamicPartitions%2A> 和 <xref:System.Collections.Concurrent.OrderablePartitioner%601.GetOrderableDynamicPartitions%2A> 不得返回 `null`（在 Visual Basic 中为 `Nothing`）。 如果返回，PLINQ/TPL 会抛出 <xref:System.InvalidOperationException>。

- 返回分区的方法应始终返回可完全且唯一枚举数据源的分区。 除非分区程序在设计上有特别要求，否则数据源或跳过的项不得有重复项。 如果未遵循此规则，输出顺序可能会出现混乱。

- 为了让输出顺序不出现混乱，下面的布尔 Getter 必须始终准确返回以下值：

  - `KeysOrderedInEachPartition`：每个分区返回密钥索引递增的元素。

  - `KeysOrderedAcrossPartitions`：对于返回的所有分区，分区 i  中的密钥索引大于分区 i  -1 中的密钥索引。

  - `KeysNormalized`：所有密钥索引从零开始不间断单调递增。

- 所有索引都必须是唯一的。 不得有重复索引。 如果未遵循此规则，输出顺序可能会出现混乱。

- 所有索引都必须为非负索引。 如果未遵循此规则，PLINQ/TPL 可能会抛出异常。

## <a name="see-also"></a>另请参阅

- [并行编程](index.md)
- [如何：实现动态分区](how-to-implement-dynamic-partitions.md)
- [如何：实现静态分区程序](how-to-implement-a-partitioner-for-static-partitioning.md)
