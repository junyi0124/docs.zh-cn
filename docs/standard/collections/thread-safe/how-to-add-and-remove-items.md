---
title: 在 ConcurrentDictionary 中添加和移除项
description: 阅读有关如何在 .NET 中的 ConcurrentDictionary<TKey,TValue> 集合类中添加、检索、更新和删除项的示例。
ms.date: 05/04/2020
dev_langs:
- csharp
- vb
helpviewer_keywords:
- thread-safe collections, concurrent dictionary
ms.assetid: 81b64b95-13f7-4532-9249-ab532f629598
ms.openlocfilehash: 17d820aba564d467152c52c7a0352bbc860f548b
ms.sourcegitcommit: 965a5af7918acb0a3fd3baf342e15d511ef75188
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94823805"
---
# <a name="how-to-add-and-remove-items-from-a-concurrentdictionary"></a>如何在 ConcurrentDictionary 中添加和删除项

本示例演示如何在 <xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=nameWithType> 中添加、检索、更新和删除项。 此集合类是一个线程安全实现。 建议在多个线程可能同时尝试访问元素时使用此集合类。

<xref:System.Collections.Concurrent.ConcurrentDictionary%602> 提供了多个便捷的方法，这些方法使代码在尝试添加或移除数据之前无需先检查键是否存在。 下表列出了这些便捷的方法，并说明在何种情况下这些方法。

| 方法 | 何时使用… |
|--|--|
| <xref:System.Collections.Concurrent.ConcurrentDictionary%602.AddOrUpdate%2A> | 需要为指定键添加新值，如果此键已存在，则需要替换其值。 |
| <xref:System.Collections.Concurrent.ConcurrentDictionary%602.GetOrAdd%2A> | 需要检索指定键的现有值，如果此键不存在，则需要指定一个键/值对。 |
| <xref:System.Collections.Concurrent.ConcurrentDictionary%602.TryAdd%2A>, <xref:System.Collections.Concurrent.ConcurrentDictionary%602.TryGetValue%2A>, <xref:System.Collections.Concurrent.ConcurrentDictionary%602.TryUpdate%2A>, <xref:System.Collections.Concurrent.ConcurrentDictionary%602.TryRemove%2A> | 需要添加、获取、更新或移除键/值对，如果此键已存在或因任何其他原因导致尝试失败，则需执行某种备选操作。 |

## <a name="example"></a>示例

下面的示例使用两个 <xref:System.Threading.Tasks.Task> 实例将一些元素同时添加到 <xref:System.Collections.Concurrent.ConcurrentDictionary%602> 中，然后输出所有内容，指明元素已成功添加。 此示例还演示如何使用 <xref:System.Collections.Concurrent.ConcurrentDictionary%602.AddOrUpdate%2A>、<xref:System.Collections.Generic.Dictionary%602.TryGetValue%2A> 和 <xref:System.Collections.Concurrent.ConcurrentDictionary%602.GetOrAdd%2A> 方法在集合中添加、更新、检索和删除项目。

[!code-csharp[CDS#16](../../../../samples/snippets/csharp/VS_Snippets_Misc/cds/cs/cds_dictionaryhowto.cs#16)]
[!code-vb[CDS#16](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds/vb/cds_concdict.vb#16)]

<xref:System.Collections.Concurrent.ConcurrentDictionary%602> 专为多线程方案而设计。 无需在代码中使用锁定即可在集合中添加或移除项。 但始终可能出现以下情况：一个线程检索一个值，而另一线程通过为同一键赋予新值来立即更新集合。

此外，尽管 <xref:System.Collections.Concurrent.ConcurrentDictionary%602> 的所有方法都是线程安全的，但并非所有方法都是原子的，尤其是 <xref:System.Collections.Concurrent.ConcurrentDictionary%602.GetOrAdd%2A> 和 <xref:System.Collections.Concurrent.ConcurrentDictionary%602.AddOrUpdate%2A>。 为避免未知代码阻止所有线程，传递给这些方法的用户委托将在词典的内部锁之外调用。 因此，可能发生以下事件序列：

1. threadA 调用 <xref:System.Collections.Concurrent.ConcurrentDictionary%602.GetOrAdd%2A>，未找到项，通过调用 `valueFactory` 委托创建要添加的新项。

1. threadB 并发调用 <xref:System.Collections.Concurrent.ConcurrentDictionary%602.GetOrAdd%2A>，其 `valueFactory` 委托受到调用，并且它在 threadA 之前到达内部锁，并将其新键值对添加到词典中 。

1. threadA 的用户委托完成，此线程到达锁位置，但现在发现已有项存在。

1. threadA 执行“Get”，返回之前由 threadB 添加的数据 。

因此，无法保证 <xref:System.Collections.Concurrent.ConcurrentDictionary%602.GetOrAdd%2A> 返回的数据与线程的 `valueFactory` 创建的数据相同。 调用 <xref:System.Collections.Concurrent.ConcurrentDictionary%602.AddOrUpdate%2A> 时可能发生相似的事件序列。

## <a name="see-also"></a>请参阅

- <xref:System.Collections.Concurrent?displayProperty=nameWithType>
- [线程安全集合](index.md)
