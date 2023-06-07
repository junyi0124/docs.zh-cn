---
title: 何时使用泛型集合
ms.date: 04/30/2020
helpviewer_keywords:
- collections [.NET], generic
- generic collections [.NET]
ms.assetid: e7b868b1-11fe-4ac5-bed3-de68aca47739
ms.openlocfilehash: 7b8b7b458a1667a1d3239ef378c729929678e8aa
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733069"
---
# <a name="when-to-use-generic-collections"></a>何时使用泛型集合

使用泛型集合可获得类型安全的自动化优点而无需从基集合类型派生和实现特定类型的成员。 当集合元素为值类型时，泛型集合类型也通常优于对应的非泛型集合类型（比从非泛型基集合类型派生的类型好），因为使用泛型时不必对元素进行装箱。

对于面向 .NET Standard 1.0 或更高版本的程序，请在多个线程可能会同时向集合添加项或从集合中删除项时使用 <xref:System.Collections.Concurrent> 命名空间中的泛型集合。 此外，当需要不可变性时，请考虑 <xref:System.Collections.Immutable> 命名空间中的泛型集合类。

以下泛型类型对应于现有集合类型：

- <xref:System.Collections.Generic.List%601> 泛型类对应于 <xref:System.Collections.ArrayList>。

- <xref:System.Collections.Generic.Dictionary%602> 和 <xref:System.Collections.Concurrent.ConcurrentDictionary%602> 泛型类对应 <xref:System.Collections.Hashtable>。

- <xref:System.Collections.ObjectModel.Collection%601> 泛型类对应于 <xref:System.Collections.CollectionBase>。 <xref:System.Collections.ObjectModel.Collection%601> 可以用作基类，但是与 <xref:System.Collections.CollectionBase> 不同，它不抽象，这大大降低了其使用难度。

- <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> 泛型类对应于 <xref:System.Collections.ReadOnlyCollectionBase>。 <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> 不是抽象的并且拥有可以轻松地公开现有的 <xref:System.Collections.Generic.List%601> 为只读集合的构造函数。

- <xref:System.Collections.Generic.Queue%601>、 <xref:System.Collections.Concurrent.ConcurrentQueue%601>、 <xref:System.Collections.Immutable.ImmutableQueue%601>、 <xref:System.Collections.Immutable.ImmutableArray%601>、<xref:System.Collections.Generic.SortedList%602> 和 <xref:System.Collections.Immutable.ImmutableSortedSet%601> 泛型类对应有着相应的相同名称的非泛型类。

## <a name="additional-types"></a>其他类型

几种泛型集合类型没有对应的非泛型集合类型。 它们包括以下类型：

- <xref:System.Collections.Generic.LinkedList%601> 是一个通用的链接列表，该列表提供 O(1) 插入和删除操作。

- <xref:System.Collections.Generic.SortedDictionary%602> 是一个有 O(log `n`) 插入和检索操作的已排序字典，这使它有效代替了 <xref:System.Collections.Generic.SortedList%602>。

- <xref:System.Collections.ObjectModel.KeyedCollection%602> 是列表和字典的结合，它提供了一种方法来存储包含自己的键的对象。

- <xref:System.Collections.Concurrent.BlockingCollection%601> 通过限制和阻止功能实现集合类。

- <xref:System.Collections.Concurrent.ConcurrentBag%601> 能快速插入和移除未排序元素。

### <a name="immutable-builders"></a>不可变生成器

如果需要在应用中使用不可变性功能，<xref:System.Collections.Immutable> 命名空间提供了可使用的泛型集合类型。 所有不可变的集合类型都提供 `Builder` 类，此类在执行多个突变时可以优化性能。 `Builder` 类在不可变状态下批处理操作。 所有突变都完成后，调用 `ToImmutable` 方法来“冻结”所有节点并创建一个不可变的泛型集合，例如 <xref:System.Collections.Immutable.ImmutableList%601>。

可以通过调用非泛型 `CreateBuilder()` 方法来创建 `Builder` 对象。 通过 `Builder` 实例，可以调用 `ToImmutable()`。 同样，通过 `Immutable*` 集合中，可以调用 `ToBuilder()` 从泛型不可变集合创建生成器实例。 以下是各种 `Builder` 类型。

- <xref:System.Collections.Immutable.ImmutableArray%601.Builder>
- <xref:System.Collections.Immutable.ImmutableDictionary%602.Builder>
- <xref:System.Collections.Immutable.ImmutableHashSet%601.Builder>
- <xref:System.Collections.Immutable.ImmutableList%601.Builder>
- <xref:System.Collections.Immutable.ImmutableSortedDictionary%602.Builder>
- <xref:System.Collections.Immutable.ImmutableSortedSet%601.Builder>

## <a name="linq-to-objects"></a>LINQ to Objects

你可以通过 LINQ to Objects 功能使用 LINQ 查询来访问内存中的对象，但条件是该对象类型要实现 <xref:System.Collections.IEnumerable?displayProperty=nameWithType> 或 <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> 接口。 LINQ 查询提供了一种通用的数据访问模式；与标准 `foreach` 循环相比，它通常更加简洁；可读性更高，并且可提供筛选、排序和分组功能。 LINQ 查询还可提高性能。 有关详细信息，请参阅 “[LINQ to Objects (C#)](../../csharp/programming-guide/concepts/linq/linq-to-objects.md)”、“[LINQ to Objects (Visual Basic)](../../visual-basic/programming-guide/concepts/linq/linq-to-objects.md)” 和 “[并行 LINQ (PLINQ)](../parallel-programming/introduction-to-plinq.md)”。

## <a name="additional-functionality"></a>其他功能

一些泛型类型具有非泛型集合类型中找不到的功能。 比如与非泛型 <xref:System.Collections.Generic.List%601> 类相对的 <xref:System.Collections.ArrayList> 类有大量接受泛型委托的方法，例如允许你指定搜索列表的方法的 <xref:System.Predicate%601> 委托、代表对列表中每个元素发挥作用的 <xref:System.Action%601> 委托和在类型间定义对话的 <xref:System.Converter%602> 委托。

<xref:System.Collections.Generic.List%601> 类使你可以指定你自己的用于排序和搜索列表的 <xref:System.Collections.Generic.IComparer%601> 泛型接口实现。 <xref:System.Collections.Generic.SortedDictionary%602> 和 <xref:System.Collections.Generic.SortedList%602> 类也有这个功能。 另外，这些类使你可以在创建集合时指定比较器。 同样地，<xref:System.Collections.Generic.Dictionary%602> 和 <xref:System.Collections.ObjectModel.KeyedCollection%602> 类让你指定自己的相等比较器。

## <a name="see-also"></a>请参阅

- [集合和数据结构](index.md)
- [常用的集合类型](commonly-used-collection-types.md)
- [泛型](../generics/index.md)
