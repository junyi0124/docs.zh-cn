---
title: 集合准则
ms.date: 10/22/2008
ms.assetid: 297b8f1d-b11f-4dc6-960a-8e990817304e
ms.openlocfilehash: a143e88be01bf2c8f45e25f26498d2d3ccbd98da
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95706666"
---
# <a name="guidelines-for-collections"></a>集合准则

专门设计为操作一组具有某种常见特征的对象的任何类型都可以被视为集合。 对于这种类型的实现，几乎总是适当 <xref:System.Collections.IEnumerable> 的 <xref:System.Collections.Generic.IEnumerable%601> ，因此在本部分中，我们只考虑实现其中一个或两个接口都为集合的类型。

 ❌ 不要在公共 Api 中使用弱类型集合。

 表示集合项的所有返回值和参数的类型应为确切项类型，而不是其任何基类型 (仅适用于集合) 的公共成员。

 ❌ 不要 <xref:System.Collections.ArrayList> <xref:System.Collections.Generic.List%601> 在公共 api 中使用或。

 这些类型是设计用于内部实现的数据结构，而不是公共 Api。 `List<T>` 针对性能和功能进行了优化，但代价是 cleanness Api 和灵活性。 例如，如果你返回 `List<T>` ，则在客户端代码修改集合时，你将不会收到通知。 此外，还 `List<T>` 公开了许多成员（例如 <xref:System.Collections.Generic.List%601.BinarySearch%2A> ）在许多情况下都不起作用或适用的成员。 以下两节介绍了专门设计用于公共 Api 的)  (抽象类型。

 ❌ 不要 `Hashtable` `Dictionary<TKey,TValue>` 在公共 api 中使用或。

 这些类型是设计用于内部实现的数据结构。 公共 Api 应使用 <xref:System.Collections.IDictionary> 、 `IDictionary <TKey, TValue>` 或实现一个或两个接口的自定义类型。

 ❌<xref:System.Collections.Generic.IEnumerator%601> <xref:System.Collections.IEnumerator> 除了作为方法的返回类型之外，请不要使用、或任何其他实现这些接口的类型 `GetEnumerator` 。

 从以外的方法返回枚举器的类型 `GetEnumerator` 不能与 `foreach` 语句一起使用。

 ❌ 不要 `IEnumerator<T>` `IEnumerable<T>` 在同一类型上实现和。 这同样适用于非泛型接口 `IEnumerator` 和 `IEnumerable` 。

## <a name="collection-parameters"></a>集合参数

 ✔️确实使用最小专用类型作为参数类型。 将集合作为参数的大多数成员使用 `IEnumerable<T>` 接口。

 ❌<xref:System.Collections.Generic.ICollection%601> <xref:System.Collections.ICollection> 只需使用或作为参数即可访问 `Count` 属性。

 相反，请考虑使用 `IEnumerable<T>` 或 `IEnumerable` 并动态检查该对象是否实现 `ICollection<T>` 或 `ICollection` 。

## <a name="collection-properties-and-return-values"></a>集合属性和返回值

 ❌ 不要提供可设置的集合属性。

 用户可以通过先清除集合，然后添加新内容来替换集合的内容。 如果替换整个集合是一个常见方案，请考虑 `AddRange` 在集合上提供方法。

 ✔️ `Collection<T>` `Collection<T>` 对表示读/写集合的属性或返回值使用或的子类。

 如果不 `Collection<T>` 满足某些要求 (例如，集合不得实现 <xref:System.Collections.IList>) ，请通过实现、或来使用自定义集合 `IEnumerable<T>` `ICollection<T>` <xref:System.Collections.Generic.IList%601> 。

 ✔️使用 <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> 、的子类 `ReadOnlyCollection<T>` 或在极少数情况下 `IEnumerable<T>` 表示只读集合的属性或返回值。

 通常情况下，首选 `ReadOnlyCollection<T>` 。 如果它不满足某些要求 (例如，集合不得实现 `IList`) ，请通过实现、或来使用自定义集合 `IEnumerable<T>` `ICollection<T>` `IList<T>` 。 如果实现自定义的只读集合，则实现 `ICollection<T>.IsReadOnly` 以返回 `true` 。

 如果你确定要支持的唯一方案是只进迭代，只需使用即可 `IEnumerable<T>` 。

 ✔️考虑使用泛型基集合的子类，而不是直接使用集合。

 这样，就可以更好地命名并添加基集合类型中不存在的帮助器成员。 这尤其适用于高级 Api。

 ✔️考虑 `Collection<T>` `ReadOnlyCollection<T>` 从非常常用的方法和属性返回或的子类。

 这将使你可以在将来添加帮助器方法或更改集合实现。

 ✔️如果集合中存储的项具有唯一键 (名称、Id 等 ) ，则请考虑使用键控集合。 键控集合是可以由整数和键编制索引的集合，通常是通过从继承来实现的 `KeyedCollection<TKey,TItem>` 。

 键控集合通常具有较大的内存占用量，因此，如果内存开销超过具有密钥的优点，则不应使用。

 ❌ 不要从集合属性或返回集合的方法中返回 null 值。 改为返回空集合或空数组。

 一般规则是 (0 项) 集合或数组应视为相同。

### <a name="snapshots-versus-live-collections"></a>快照与活动集合

 表示某个时间点的状态的集合称为快照集合。 例如，包含从数据库查询返回的行的集合将为快照。 始终表示当前状态的集合称为活动集合。 例如，项的集合 `ComboBox` 为活动集合。

 ❌ 不要从属性返回快照集合。 属性应返回活动集合。

 属性 getter 应为非常轻量的操作。 返回快照需要在 O (n) 操作中创建内部集合的副本。

 ✔️使用快照集合或实时 `IEnumerable<T>` (或其子类型) 来表示 (可变的集合，而不是显式修改集合) ，则可以更改这些集合。

 通常，表示共享资源的所有集合 (例如，目录) 中的文件是可变的。 此类集合非常困难，或者无法实现为活动集合，除非实现只是一个只进枚举器。

## <a name="choosing-between-arrays-and-collections"></a>在数组和集合之间进行选择

 ✔️是否比数组更倾向于集合。

 集合可以更好地控制内容、随时间推移而变化，并且更易于使用。 此外，不建议对只读方案使用数组，因为克隆数组的成本是不高的。 可用性研究表明，某些开发人员更喜欢使用基于集合的 Api。

 但是，如果要开发低级别 Api，则最好使用数组进行读写方案。 数组的内存占用量较小，有助于减少工作集，并且对数组中的元素的访问速度更快，因为它已由运行时进行优化。

 ✔️考虑在低级别 Api 中使用数组，以最大程度地减少内存占用和最大化性能。

 ✔️使用字节数组而不是字节集合。

 ❌ 如果属性必须返回新数组 (例如，每次调用属性 getter 时) 内部数组的副本，请不要使用属性的数组。

## <a name="implementing-custom-collections"></a>实现自定义集合

 ✔️考虑 `Collection<T>` `ReadOnlyCollection<T>` `KeyedCollection<TKey,TItem>` 在设计新集合时从、或继承。

 ✔️ `IEnumerable<T>` 在设计新集合时实现。 请考虑实现甚至 `ICollection<T>` `IList<T>` 有意义的地方。

 实现此类自定义集合时，请尽可能地遵循和建立的 API 模式 `Collection<T>` `ReadOnlyCollection<T>` 。 也就是说，显式实现相同的成员，将这些参数命名为这两个集合，将它们命名为，依此类推。

 ✔️考虑实现非泛型集合 (接口 `IList` ， `ICollection` 如果集合经常传递到采用这些接口作为输入的 api，则) 。

 ❌ 避免在具有与集合概念无关的复杂 Api 的类型上实现集合接口。

 ❌ 不要从非泛型基集合（如）继承 `CollectionBase` 。 请改用 `Collection<T>` 、 `ReadOnlyCollection<T>` 和 `KeyedCollection<TKey,TItem>` 。

### <a name="naming-custom-collections"></a>命名自定义集合

 用于实现) 的集合 (类型 `IEnumerable` 主要有两个原因： (1) 创建具有特定结构的操作的新数据结构，并且通常与现有数据结构的性能特征不同 (例如，、、  <xref:System.Collections.Generic.List%601> <xref:System.Collections.Generic.LinkedList%601> <xref:System.Collections.Generic.Stack%601>) 和 (2) 创建专用集合来保存一组特定的项 (例如  <xref:System.Collections.Specialized.StringCollection>) 。 数据结构通常用于应用程序和库的内部实现。 专用集合主要在) 的属性和参数类型 (中公开。

 ✔️在实现或的抽象名称中使用 "Dictionary" 后缀 `IDictionary` `IDictionary<TKey,TValue>` 。

 ✔️在实现 (的类型名称 `IEnumerable` 或其任意后代) ，并表示项列表时使用 "Collection" 后缀。

 ✔️为自定义数据结构使用适当的数据结构名称。

 ❌ 避免在集合抽象化的名称中使用任何后缀来暗指特定实现，如 "LinkedList" 或 "哈希表"。

 ✔️考虑为集合名称加上项类型的名称。 例如，存储 (实现) 类型的项的集合 `Address` `IEnumerable<Address>` 应命名为 `AddressCollection` 。 如果项类型为接口，则可以省略项类型的 "I" 前缀。 因此， <xref:System.IDisposable> 可以调用项的集合 `DisposableCollection` 。

 如果可在框架中添加或已存在相应的可写集合，则✔️考虑在只读集合的名称中使用 "ReadOnly" 前缀。

 例如，应调用字符串的只读集合 `ReadOnlyStringCollection` 。

 *部分©2005，2009 Microsoft Corporation。保留所有权利。*

 *经许可重印皮尔逊教育，Inc. 的作者 [：从框架设计指导原则：用于可重复使用的 .Net 库的约定、惯例和模式; 第2版](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619) By Krzysztof Cwalina，Brad Abrams，通过 Addison-Wesley Professional 作为 Microsoft Windows 开发系列的一部分2008发布。*

## <a name="see-also"></a>另请参阅

- [框架设计准则](index.md)
- [使用准则](usage-guidelines.md)
