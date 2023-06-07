---
title: 已排序的集合类型
ms.date: 04/30/2020
helpviewer_keywords:
- SortedDictionary collection type
- SortedList class, grouping data in collections
- grouping data in collections, SortedList collection type
- SortedList collection type
- collections [.NET], SortedList collection type
ms.assetid: 3db965b2-36a6-4b12-b76e-7f074ff7275a
ms.openlocfilehash: 62e0ffde31d91ea2b0bbe3b5c37cddb87349b5a3
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94823818"
---
# <a name="sorted-collection-types"></a>已排序的集合类型

<xref:System.Collections.SortedList?displayProperty=nameWithType> 类、<xref:System.Collections.Generic.SortedList%602?displayProperty=nameWithType> 泛型类和 <xref:System.Collections.Generic.SortedDictionary%602?displayProperty=nameWithType> 泛型类与 <xref:System.Collections.Hashtable> 类和 <xref:System.Collections.Generic.Dictionary%602> 泛型类的相似之处在于均实现 <xref:System.Collections.IDictionary> 接口，不同之处在于它们让元素一直按键的排序顺序排列，并且不具备哈希表的 O(1) 插入和检索特性。 这三个类具有若干共性：

- 这三个类全都实现 <xref:System.Collections.IDictionary?displayProperty=nameWithType> 接口。 两个泛型类还实现 <xref:System.Collections.Generic.IDictionary%602?displayProperty=nameWithType> 泛型接口。

- 每个元素都是用于枚举的键/值对。

   > [!NOTE]
   > 枚举时，非泛型 <xref:System.Collections.SortedList> 类返回 <xref:System.Collections.DictionaryEntry> 对象，尽管两个泛型类型返回 <xref:System.Collections.Generic.KeyValuePair%602> 对象。

- 元素按 <xref:System.Collections.IComparer?displayProperty=nameWithType> 实现代码（对于非泛型 <xref:System.Collections.SortedList>）或 <xref:System.Collections.Generic.IComparer%601?displayProperty=nameWithType> 实现代码（对于两个泛型类）进行排序。

- 每个类提供了返回仅包含键或仅包含值的集合的属性。

下表列出了两个已排序列表类与 <xref:System.Collections.Generic.SortedDictionary%602> 类之间的一些区别。

| <xref:System.Collections.SortedList> 非泛型类和 <xref:System.Collections.Generic.SortedList%602> 泛型类 | <xref:System.Collections.Generic.SortedDictionary%602> 泛型类 |
|--|--|
| 返回键和值的属性是有索引的，从而允许高效的索引检索。 | 无索引的检索。 |
| 检索属于 O(log `n`) 操作。 | 检索属于 O(log `n`) 操作。 |
| 插入和删除通常属于 O(`n`) 操作；不过，对于已按排序顺序排列的数据，插入属于 O(log `n`) 操作，这样每个元素都可以添加到列表的末尾。 （这假设不需要调整大小。） | 插入和删除属于 O(log `n`) 操作。 |
| 比 <xref:System.Collections.Generic.SortedDictionary%602> 使用更少的内存。 | 比 <xref:System.Collections.SortedList> 非泛型类和 <xref:System.Collections.Generic.SortedList%602> 泛型类使用更多内存。 |

对于必须可通过多个线程并发访问的已排序列表或字典，可以向派生自 <xref:System.Collections.Concurrent.ConcurrentDictionary%602> 的类添加排序逻辑。 考虑不可变性时，以下相应的不可变类型遵循类似的排序语义：<xref:System.Collections.Immutable.ImmutableSortedSet%601> 和 <xref:System.Collections.Immutable.ImmutableSortedDictionary%602>。

> [!NOTE]
> 对于包含自己的键的值（例如，包含雇员 ID 编号的雇员记录），可以通过派生自 <xref:System.Collections.ObjectModel.KeyedCollection%602> 泛型类，创建具有列表和字典的某些特性的键控集合。

从 .NET Framework 4 开始，<xref:System.Collections.Generic.SortedSet%601> 类提供在执行插入、删除和搜索操作之后让数据一直按排序顺序排列的自平衡树。 此类和 <xref:System.Collections.Generic.HashSet%601> 类实现 <xref:System.Collections.Generic.ISet%601> 接口。

## <a name="see-also"></a>请参阅

- <xref:System.Collections.IDictionary?displayProperty=nameWithType>
- <xref:System.Collections.Generic.IDictionary%602?displayProperty=nameWithType>
- <xref:System.Collections.Concurrent.ConcurrentDictionary%602>
- [常用的集合类型](commonly-used-collection-types.md)
