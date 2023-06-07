---
title: 集合和数据结构
description: 了解如何在 .NET 中使用集合和数据结构。 在线程安全操作中使用泛型和非泛型集合。
ms.date: 04/30/2020
helpviewer_keywords:
- grouping data in collections
- objects [.NET], grouping in collections
- Array class, grouping data in collections
- threading [.NET], safety
- Collections classes
- collections [.NET]
ms.assetid: 60cc581f-1db5-445b-ba04-a173396bf872
ms.openlocfilehash: 7400d460c4d1ebf5c02d8313f33a5a63de1734d4
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733511"
---
# <a name="collections-and-data-structures"></a>集合和数据结构

类似的数据在作为集合而存储和操作时通常可以得到更高效地处理。 可以使用 <xref:System.Array?displayProperty=nameWithType> 类或 <xref:System.Collections>、<xref:System.Collections.Generic>、<xref:System.Collections.Concurrent> 和 <xref:System.Collections.Immutable> 命名空间中的类来添加、删除和修改集合中的任一单个元素或某个范围的元素。

有两种主要的集合类型：泛型集合和非泛型集合。 泛型集合在编译时是类型安全的。 因此，泛型集合通常能提供更好的性能。 构造泛型集合时，它们接受类型形参；并在向该集合添加项或从该集合删除项时无需在 <xref:System.Object> 类型间来回转换。  此外，Windows 应用商店应用支持大多数泛型集合。 非泛型集合将项存储为 <xref:System.Object>，需要强制转换，并且大多数不支持 Windows 应用商店应用开发。 但是，你可能会看到在较旧的代码中有非泛型集合。

自 .NET Framework 4 起，<xref:System.Collections.Concurrent> 命名空间中的集合可提供高效的线程安全操作，以便从多个线程访问集合项。 <xref:System.Collections.Immutable> 命名空间（[NuGet 包](https://www.nuget.org/packages/System.Collections.Immutable)）中的不可变集合类本质上就是线程安全的，因为操作在原始集合的副本上进行且不能修改原始集合。

<a name="BKMK_Commoncollectionfeatures"></a>

## <a name="common-collection-features"></a>常用集合功能

所有集合都提供用于在集合中添加、删除或查找项的方法。 此外，所有直接或间接实现 <xref:System.Collections.ICollection> 接口或 <xref:System.Collections.Generic.ICollection%601> 接口的集合均共享这些功能：

- **可枚举集合**

    .NET 集合实现 <xref:System.Collections.IEnumerable?displayProperty=nameWithType> 或 <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType>，以启用要循环访问的集合。 可将枚举器看作集合中可指向任何元素的可移动指针。 [foreach, in](../../csharp/language-reference/keywords/foreach-in.md) 语句和  [For Each...Next 语句](../../visual-basic/language-reference/statements/for-each-next-statement.md)使用 <xref:System.Collections.IEnumerable.GetEnumerator%2A> 方法公开的枚举器并隐藏操作枚举器的复杂性。 此外，任何实现 <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> 的集合均被认为是可查询类型，并可使用 LINQ 对其进行查询。 LINQ 查询提供数据访问的一个通用模式。 它们通常比标准 `foreach` 循环更简洁、更具可读性，并提供筛选、排序和分组功能。 LINQ 查询还可提高性能。 有关详细信息，请参阅 [LINQ to Objects (C#)](../../csharp/programming-guide/concepts/linq/linq-to-objects.md)、[LINQ to Objects (Visual Basic)](../../visual-basic/programming-guide/concepts/linq/linq-to-objects.md)、[并行 LINQ (PLINQ)](../parallel-programming/introduction-to-plinq.md)、[LINQ 查询 (C#) 简介](../../csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md)和[基本查询操作 (Visual Basic)](../../visual-basic/programming-guide/concepts/linq/basic-query-operations.md)。

- **可将集合内容复制到数组**

    可使用 **CopyTo** 方法将所有集合复制到数组中；但新数组中的元素顺序是以枚举器返回元素的顺序为依据。 得到的数组始终是一维的，下限为零。

此外，许多集合类包含下列功能：

- **容量和计数属性**

    集合的容量是它可包含的元素数。 集合的计数是它实际所含的元素数。 某些集合隐藏容量、计数或将这两者都隐藏。

    达到当前容量时，大多数集合会自动扩展容量。 重新分配内存并将元素从旧集合复制到新集合。 这减少了要求使用集合的代码；但集合的性能可能会受到不利影响。 例如，对 <xref:System.Collections.Generic.List%601> 来说，如果 <xref:System.Collections.Generic.List%601.Count%2A> 比 <xref:System.Collections.Generic.List%601.Capacity%2A> 少，那么添加项就是一项 O(1) 操作。 如需增加容量以容纳新元素，则添加项成为 O(`n`) 操作，其中 `n` 是 <xref:System.Collections.Generic.List%601.Count%2A>。 避免因多次重新分配而导致的性能较差的最佳方式是：将初始容量设置为集合的估计大小。

    <xref:System.Collections.BitArray> 是一种特殊情况；它的容量与其长度相同，而其长度与其计数相同。

- **下限一致**

    集合的下限是其第一个元素的索引。 <xref:System.Collections> 命名空间中的所有索引集合的下限均为零，这表示它们从 0 开始建立索引。 <xref:System.Array> 默认下限为零，但使用 <xref:System.Array.CreateInstance%2A?displayProperty=nameWithType> 创建 Array 类的实例时可定义其他下限。

- **同步以从多个线程进行访问**（仅 <xref:System.Collections> 类）。

    <xref:System.Collections> 命名空间中的非泛型集合类型通过同步提供一些线程安全性；通常通过 <xref:System.Collections.ICollection.SyncRoot%2A> 和 <xref:System.Collections.ICollection.IsSynchronized%2A> 成员公开。 这些集合不是默认为线程安全的。 如需对集合进行可扩展、高效的多线程访问，请使用 <xref:System.Collections.Concurrent> 命名空间中的一个类或考虑使用不可变集合。 有关详细信息，请参阅[线程安全集合](thread-safe/index.md)。

<a name="BKMK_Choosingacollection"></a>

## <a name="choose-a-collection"></a>选择集合

一般情况下，应使用泛型集合。 下表介绍了一些常用的集合方案和可用于这些方案的集合类。 如果你是使用泛型集合的新手，此表将帮助你选择最适合你的任务的泛型集合。

|我要……|泛型集合选项|非泛型集合选项|线程安全或不可变集合选项|
|-|-|-|-|
|将项存储为键/值对以通过键进行快速查找|<xref:System.Collections.Generic.Dictionary%602>|<xref:System.Collections.Hashtable><br /><br /> （根据键的哈希代码组织的键/值对的集合。）|<xref:System.Collections.Concurrent.ConcurrentDictionary%602><br /><br /> <xref:System.Collections.ObjectModel.ReadOnlyDictionary%602><br /><br /> <xref:System.Collections.Immutable.ImmutableDictionary%602>|
|按索引访问项|<xref:System.Collections.Generic.List%601>|<xref:System.Array><br /><br /> <xref:System.Collections.ArrayList>|<xref:System.Collections.Immutable.ImmutableList%601><br /><br /> <xref:System.Collections.Immutable.ImmutableArray>|
|使用项先进先出 (FIFO)|<xref:System.Collections.Generic.Queue%601>|<xref:System.Collections.Queue>|<xref:System.Collections.Concurrent.ConcurrentQueue%601><br /><br /> <xref:System.Collections.Immutable.ImmutableQueue%601>|
|使用数据后进先出 (LIFO)|<xref:System.Collections.Generic.Stack%601>|<xref:System.Collections.Stack>|<xref:System.Collections.Concurrent.ConcurrentStack%601><br /><br /> <xref:System.Collections.Immutable.ImmutableStack%601>|
|按顺序访问项|<xref:System.Collections.Generic.LinkedList%601>|无建议|无建议|
|删除集合中的项或向集合添加项时接收通知。 （实现 <xref:System.ComponentModel.INotifyPropertyChanged> 和 <xref:System.Collections.Specialized.INotifyCollectionChanged>）|<xref:System.Collections.ObjectModel.ObservableCollection%601>|无建议|无建议|
|已排序的集合|<xref:System.Collections.Generic.SortedList%602>|<xref:System.Collections.SortedList>|<xref:System.Collections.Immutable.ImmutableSortedDictionary%602><br /><br /> <xref:System.Collections.Immutable.ImmutableSortedSet%601>|
|数学函数的一个集|<xref:System.Collections.Generic.HashSet%601><br /><br /> <xref:System.Collections.Generic.SortedSet%601>|无建议|<xref:System.Collections.Immutable.ImmutableHashSet%601><br /><br /> <xref:System.Collections.Immutable.ImmutableSortedSet%601>|

### <a name="algorithmic-complexity-of-collections"></a>集合的算法复杂性

选择[集合类](selecting-a-collection-class.md)时，应该考虑可能牺牲的性能。 使用下表来参考各种可变集合类型在算法复杂性方面与其对应不可变对应项的比较情况。 不可变的集合类型通常性能较低，但却提供了不可变性，这通常是一种非常有效的优点。

| 可变                   | 分期  | 最坏情况                | 不可变                          | 复杂性 |
|---------------------------|------------|---------------------------|------------------------------------|------------|
| `Stack<T>.Push`           | O(1)       | O(`n`)                    | `ImmutableStack<T>.Push`           | O(1)       |
| `Queue<T>.Enqueue`        | O(1)       | O(`n`)                    | `ImmutableQueue<T>.Enqueue`        | O(1)       |
| `List<T>.Add`             | O(1)       | O(`n`)                    | `ImmutableList<T>.Add`             | O(log `n`) |
| `List<T>.Item[Int32]`     | O(1)       | O(1)                      | `ImmutableList<T>.Item[Int32]`     | O(log `n`) |
| `List<T>.Enumerator`      | O(`n`)     | O(`n`)                    | `ImmutableList<T>.Enumerator`      | O(`n`)     |
| `HashSet<T>.Add`, lookup  | O(1)       | O(`n`)                    | `ImmutableHashSet<T>.Add`          | O(log `n`) |
| `SortedSet<T>.Add`        | O(log `n`) | O(`n`)                    | `ImmutableSortedSet<T>.Add`        | O(log `n`) |
| `Dictionary<T>.Add`       | O(1)       | O(`n`)                    | `ImmutableDictionary<T>.Add`       | O(log `n`) |
| `Dictionary<T>` lookup    | O(1)       | O(1) - 或者从严格意义上说，O(`n`) | `ImmutableDictionary<T>` lookup    | O(log `n`) |
| `SortedDictionary<T>.Add` | O(log `n`) | O(`n` log `n`)            | `ImmutableSortedDictionary<T>.Add` | O(log `n`) |

可以使用 `for` 循环或 `foreach` 循环来有效地枚举 `List<T>`。 但是，由于其索引器的 O(log `n`) 时间，`ImmutableList<T>` 在 `for` 循环内的效果较差。 使用 `foreach` 循环枚举 `ImmutableList<T>` 很有效，因为 `ImmutableList<T>` 使用二进制树来存储其数据，而不是像 `List<T>` 那样使用简单数组。 数组可以非常快速地编入索引，但必须向下遍历二进制树，直到找到具有所需索引的节点。

此外，`SortedSet<T>` 与 `ImmutableSortedSet<T>` 的复杂性相同。 这是因为它们都使用了二进制树。 当然，显著的差异在于 `ImmutableSortedSet<T>` 使用不可变的二进制树。 由于 `ImmutableSortedSet<T>` 还提供了一个允许变化的 <xref:System.Collections.Immutable.ImmutableSortedSet%601.Builder?displayProperty=nameWithType> 类，因此可以同时实现不可变性和保障性能。

<a name="BKMK_RelatedTopics"></a>

## <a name="related-topics"></a>相关主题

|Title|描述|
|-----------|-----------------|
|[选择集合类](selecting-a-collection-class.md)|描述不同的集合并帮助你为你的方案选择一个集合。|
|[常用的集合类型](commonly-used-collection-types.md)|描述诸如 <xref:System.Array?displayProperty=nameWithType>、<xref:System.Collections.Generic.List%601?displayProperty=nameWithType> 和 <xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType> 等常用泛型和非泛型集合类型。|
|[何时使用泛型集合](when-to-use-generic-collections.md)|讨论泛型集合类型的使用。|
|[集合内的比较和排序](comparisons-and-sorts-within-collections.md)|讨论在集合中使用等同性比较和排序比较。|
|[已排序的集合类型](sorted-collection-types.md)|描述已排序集合的性能和特征|
|[哈希表和字典集合类型](hashtable-and-dictionary-collection-types.md)|描述泛型和非泛型基于哈希的字典类型的功能。|
|[线程安全集合](thread-safe/index.md)|介绍支持从多个线程进行安全有效的并发访问的集合类型，例如 <xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=nameWithType> 和 <xref:System.Collections.Concurrent.ConcurrentBag%601?displayProperty=nameWithType>。|
|System.Collections.Immutable|介绍不可变集合并提供各集合类型的链接。|

<a name="BKMK_Reference"></a>

## <a name="reference"></a>参考

<xref:System.Array?displayProperty=nameWithType>
<xref:System.Collections?displayProperty=nameWithType>
<xref:System.Collections.Concurrent?displayProperty=nameWithType>
<xref:System.Collections.Generic?displayProperty=nameWithType>
<xref:System.Collections.Specialized?displayProperty=nameWithType>
<xref:System.Linq?displayProperty=nameWithType>
<xref:System.Collections.Immutable>
