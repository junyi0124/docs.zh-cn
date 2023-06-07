---
ms.openlocfilehash: 97fab784acac4331894547eea27fc21b485597fb
ms.sourcegitcommit: fcbe432482464b1639decad78cc4dc8387c6269e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/12/2020
ms.locfileid: "97366879"
---
### <a name="passing-groupcollection-to-extension-methods-taking-ienumerablet-requires-disambiguation"></a>将 GroupCollection 传递到采用 IEnumerable\<T> 的扩展方法需要消除歧义

调用在 <xref:System.Text.RegularExpressions.GroupCollection> 上采用 `IEnumerable<T>` 的扩展方法时，必须使用强制转换来消除该类型的歧义。

#### <a name="change-description"></a>更改说明

从 .NET Core 3.0 开始，<xref:System.Text.RegularExpressions.GroupCollection?displayProperty=nameWithType> 除了实现 `IEnumerable<Group>` 等类型之外，还会实现 `IEnumerable<KeyValuePair<String,Group>>`。 调用采用 <xref:System.Collections.Generic.IEnumerable%601> 的扩展方法时，这会导致歧义。 如果在 <xref:System.Text.RegularExpressions.GroupCollection> 实例上调用此类扩展方法，例如 <xref:System.Linq.Enumerable.Count%2A?displayProperty=nameWithType>，你将看到以下编译器错误：

CS1061：“GroupCollection”未包含“Count”的定义，并且找不到可接受第一个“GroupCollection”类型参数的可访问扩展方法“Count”（是否缺少 using 指令或程序集引用？）

在早期版本的 .NET 中，没有歧义，也没有编译器错误。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="reason-for-change"></a>更改原因

这是[意外的中断性变更](https://github.com/dotnet/corefx/pull/30077)。 由于此更改已经有一段时间了，所以我们不打算将其还原。 此外，此类更改本身就会造成中断。

#### <a name="recommended-action"></a>建议操作

对于 <xref:System.Text.RegularExpressions.GroupCollection> 实例，使用强制转换对接受 `IEnumerable<T>` 的扩展方法的调用消除歧义。

```csharp
// Without a cast - causes CS1061.
match.Groups.Count(_ => true)

// With a disambiguating cast.
((IEnumerable<Group>)m.Groups).Count(_ => true);
```

#### <a name="category"></a>类别

Core .NET 库

#### <a name="affected-apis"></a>受影响的 API

任何接受 <xref:System.Collections.Generic.IEnumerable%601> 的扩展方法都会受到影响。 例如：

- <xref:System.Collections.Immutable.ImmutableArray.ToImmutableArray%60%601(System.Collections.Generic.IEnumerable{%60%600})?displayProperty=fullName>
- <xref:System.Collections.Immutable.ImmutableDictionary.ToImmutableDictionary%2A?displayProperty=fullName>
- <xref:System.Collections.Immutable.ImmutableHashSet.ToImmutableHashSet%2A?displayProperty=fullName>
- <xref:System.Collections.Immutable.ImmutableList.ToImmutableList%60%601(System.Collections.Generic.IEnumerable{%60%600})?displayProperty=fullName>
- <xref:System.Collections.Immutable.ImmutableSortedDictionary.ToImmutableSortedDictionary%2A?displayProperty=fullName>
- <xref:System.Collections.Immutable.ImmutableSortedSet.ToImmutableSortedSet%2A?displayProperty=fullName>
- <xref:System.Data.DataTableExtensions.CopyToDataTable%2A?displayProperty=fullName>
- 大多数 `System.Linq.Enumerable` 方法，例如 <xref:System.Linq.Enumerable.Count%2A?displayProperty=fullName>
- <xref:System.Linq.ParallelEnumerable.AsParallel%2A?displayProperty=fullName>
- <xref:System.Linq.Queryable.AsQueryable%2A?displayProperty=fullName>

<!--

#### Affected APIs

- ``M:System.Collections.Immutable.ImmutableArray.ToImmutableArray``1(System.Collections.Generic.IEnumerable{``0})``
- `Overload:System.Collections.Immutable.ImmutableDictionary.ToImmutableDictionary`
- `Overload:System.Collections.Immutable.ImmutableHashSet.ToImmutableHashSet`
- ``M:System.Collections.Immutable.ImmutableList.ToImmutableList``1(System.Collections.Generic.IEnumerable{``0})``
- `Overload:System.Collections.Immutable.ImmutableSortedDictionary.ToImmutableSortedDictionary`
- `Overload:System.Collections.Immutable.ImmutableSortedSet.ToImmutableSortedSet`
- `Overload:System.Data.DataTableExtensions.CopyToDataTable`
- `Overload:System.Linq.Enumerable.Count`
- `Overload:System.Linq.ParallelEnumerable.AsParallel`
- `Overload:System.Linq.Queryable.AsQueryable`

-->
