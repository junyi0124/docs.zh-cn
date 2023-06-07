---
title: 选择集合类
description: 了解如何确定 .NET 中要选择的集合类。 使用错误的类型可能会限制集合的使用。
ms.date: 03/18/2019
helpviewer_keywords:
- last-in-first-out collections
- first-in-first-out collections
- collections [.NET], selecting collection class
- indexed collections
- Collections classes
- grouping data in collections, selecting collection class
ms.assetid: ba049f9a-ce87-4cc4-b319-3f75c8ddac8a
ms.openlocfilehash: 7af08949df999ab80fce1308927d87a8935e3b5d
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94818721"
---
# <a name="selecting-a-collection-class"></a>选择集合类

请务必仔细选择你的集合类。 使用错误的类型可能会限制集合的使用。

> [!IMPORTANT]
> 请避免使用 <xref:System.Collections> 命名空间中的类型。 推荐使用泛型版本和并发版本的集合，因为它们的类型安全性很高，并且还经过了其他改进。

请考虑下列问题：

- 是否需要顺序列表（其中通常在检索元素值后就将该元素丢弃）？

  - 在需要的情况下，如果需要先进先出 (FIFO) 行为，请考虑使用 <xref:System.Collections.Queue> 类或 <xref:System.Collections.Generic.Queue%601> 泛型类。 如果需要后进先出 (LIFO) 行为，请考虑使用 <xref:System.Collections.Stack> 类或 <xref:System.Collections.Generic.Stack%601> 泛型类。 若要从多个线程进行安全访问，请使用并发版本（<xref:System.Collections.Concurrent.ConcurrentQueue%601> 和 <xref:System.Collections.Concurrent.ConcurrentStack%601>）。 如果要获得不可变性，请考虑不可变版本 <xref:System.Collections.Immutable.ImmutableQueue%601> 和 <xref:System.Collections.Immutable.ImmutableStack%601>。

  - 如果不需要，请考虑使用其他集合。

- 是否需要以特定顺序（如先进先出、后进先出或随机）访问元素？

  - <xref:System.Collections.Queue> 类以及 <xref:System.Collections.Generic.Queue%601>、<xref:System.Collections.Concurrent.ConcurrentQueue%601> 和 <xref:System.Collections.Immutable.ImmutableQueue%601> 泛型类都提供 FIFO 访问权限。 有关详细信息，请参阅[何时使用线程安全集合](thread-safe/when-to-use-a-thread-safe-collection.md)。

  - <xref:System.Collections.Stack> 类以及 <xref:System.Collections.Generic.Stack%601>、<xref:System.Collections.Concurrent.ConcurrentStack%601> 和 <xref:System.Collections.Immutable.ImmutableStack%601> 泛型类都提供 LIFO 访问权限。 有关详细信息，请参阅[何时使用线程安全集合](thread-safe/when-to-use-a-thread-safe-collection.md)。

  - <xref:System.Collections.Generic.LinkedList%601> 泛型类允许从开头到末尾或从末尾到开头的顺序访问。

- 是否需要按索引访问每个元素？

  - <xref:System.Collections.ArrayList> 和 <xref:System.Collections.Specialized.StringCollection> 类以及 <xref:System.Collections.Generic.List%601> 泛型类按从零开始的元素索引提供对其元素的访问。 如果要获得不可变性，请考虑不可变泛型版本 <xref:System.Collections.Immutable.ImmutableArray%601> 和 <xref:System.Collections.Immutable.ImmutableList%601>。

  - <xref:System.Collections.Hashtable>、<xref:System.Collections.SortedList>、<xref:System.Collections.Specialized.ListDictionary> 和 <xref:System.Collections.Specialized.StringDictionary> 类以及 <xref:System.Collections.Generic.Dictionary%602> 和 <xref:System.Collections.Generic.SortedDictionary%602> 泛型类按元素的键提供对其元素的访问。 此外，还有几个相应类型的不可变版本：<xref:System.Collections.Immutable.ImmutableHashSet%601>、<xref:System.Collections.Immutable.ImmutableDictionary%602>、<xref:System.Collections.Immutable.ImmutableSortedSet%601> 和 <xref:System.Collections.Immutable.ImmutableSortedDictionary%602>。

  - <xref:System.Collections.Specialized.NameObjectCollectionBase> 和 <xref:System.Collections.Specialized.NameValueCollection> 类以及 <xref:System.Collections.ObjectModel.KeyedCollection%602> 和 <xref:System.Collections.Generic.SortedList%602> 泛型类按从零开始的元素索引或元素的键提供对其元素的访问。

- 是否每个元素都包含一个值、一个键和一个值的组合或一个键和多个值的组合？

  - 一个值：使用任何基于 <xref:System.Collections.IList> 接口或 <xref:System.Collections.Generic.IList%601> 泛型接口的集合。 要获得不可变选项，请考虑 <xref:System.Collections.Immutable.IImmutableList%601> 泛型接口。

  - 一个键和一个值：使用任何基于 <xref:System.Collections.IDictionary> 接口或 <xref:System.Collections.Generic.IDictionary%602> 泛型接口的集合。 要获得不可变选项，请考虑 <xref:System.Collections.Immutable.IImmutableSet%601> 或 <xref:System.Collections.Immutable.IImmutableDictionary%602> 泛型接口。

  - 带有嵌入键的值：使用 <xref:System.Collections.ObjectModel.KeyedCollection%602> 泛型类。

  - 一个键和多个值：使用 <xref:System.Collections.Specialized.NameValueCollection> 类。

- 是否需要以与输入方式不同的方式对元素进行排序？

  - <xref:System.Collections.Hashtable> 类按其哈希代码对其元素进行排序。

  - <xref:System.Collections.SortedList> 类以及 <xref:System.Collections.Generic.SortedList%602> 和 <xref:System.Collections.Generic.SortedDictionary%602> 泛型类按键对元素进行排序。 排序顺序的依据为，实现 <xref:System.Collections.SortedList> 类的 <xref:System.Collections.IComparer> 接口和实现 <xref:System.Collections.Generic.SortedList%602> 和 <xref:System.Collections.Generic.SortedDictionary%602> 泛型类的 <xref:System.Collections.Generic.IComparer%601> 泛型接口。 在这两种泛型类型中，虽然 <xref:System.Collections.Generic.SortedDictionary%602> 的性能优于 <xref:System.Collections.Generic.SortedList%602>，但 <xref:System.Collections.Generic.SortedList%602> 占用的内存更少。

  - <xref:System.Collections.ArrayList> 提供了一种 <xref:System.Collections.ArrayList.Sort%2A> 方法，此方法采用 <xref:System.Collections.IComparer> 实现作为参数。 其泛型对应项（<xref:System.Collections.Generic.List%601> 泛型类）提供一种 <xref:System.Collections.Generic.List%601.Sort%2A> 方法，此方法采用 <xref:System.Collections.Generic.IComparer%601> 泛型接口的实现作为参数。

- 是否需要快速搜索和信息检索？

  - 对于小集合（10 项或更少），<xref:System.Collections.Specialized.ListDictionary> 速度比 <xref:System.Collections.Hashtable> 快。 <xref:System.Collections.Generic.Dictionary%602> 泛型类提供比 <xref:System.Collections.Generic.SortedDictionary%602> 泛型类更快的查找。 多线程的实现为 <xref:System.Collections.Concurrent.ConcurrentDictionary%602>。 <xref:System.Collections.Concurrent.ConcurrentBag%601> 为无序数据提供快速的多线程插入。 有关这两种多线程类型的详细信息，请参阅[何时使用线程安全集合](thread-safe/when-to-use-a-thread-safe-collection.md)。

- 是否需要只接受字符串的集合？

  - <xref:System.Collections.Specialized.StringCollection>（基于 <xref:System.Collections.IList>）和 <xref:System.Collections.Specialized.StringDictionary>（基于 <xref:System.Collections.IDictionary>）位于 <xref:System.Collections.Specialized> 命名空间。

  - 此外，通过指定其泛型类参数的 <xref:System.String> 类，可以使用 <xref:System.Collections.Generic> 命名空间中的任何泛型集合类作为强类型字符串集合。 例如，可以将变量声明为采用 [List\<String>](xref:System.Collections.Generic.List%601) 或 [Dictionary<String,String>](xref:System.Collections.Generic.Dictionary%602) 类型。

## <a name="linq-to-objects-and-plinq"></a>LINQ to Objects 与 PLINQ

LINQ to Objects 让开发人员能够使用 LINQ 查询访问内存中对象，条件是该对象类型实现 <xref:System.Collections.IEnumerable> 或 <xref:System.Collections.Generic.IEnumerable%601>。 LINQ 查询提供了一种通用的数据访问模式，与标准 `foreach` 循环相比，它通常更加简洁，可读性更高，并且可提供筛选、排序和分组功能。 有关详细信息，请参阅 [LINQ to Objects (C#)](../../csharp/programming-guide/concepts/linq/linq-to-objects.md) 和 [LINQ to Objects (Visual Basic)](../../visual-basic/programming-guide/concepts/linq/linq-to-objects.md)。

PLINQ 提供 LINQ to Objects 的并行实现，在许多情况下，可通过更有效地利用多核计算机提供更快的查询执行。 有关详细信息，请参阅[并行 LINQ (PLINQ)](../parallel-programming/introduction-to-plinq.md)。

## <a name="see-also"></a>请参阅

- <xref:System.Collections>
- <xref:System.Collections.Specialized>
- <xref:System.Collections.Generic>
- [线程安全集合](thread-safe/index.md)
