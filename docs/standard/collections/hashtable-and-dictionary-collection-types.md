---
title: 哈希表和字典集合类型
ms.date: 03/30/2017
'\helpviewer_keywords':
- Hashtable class, grouping data in collections
- Hashtable collection type
- hash tables
- grouping data in collections, Hashtable collection type
- hash function
- collections [.NET], Hashtable collection type
ms.assetid: bfc20837-3d02-4fc7-8a8f-c5215b6b7913
ms.openlocfilehash: 56e8ceff05f8f9777af370301a96cd17d0286f44
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95733524"
---
# <a name="hashtable-and-dictionary-collection-types"></a>哈希表和字典集合类型

<xref:System.Collections.Hashtable?displayProperty=nameWithType> 类以及 <xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType> 和 <xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=nameWithType> 泛型类实现 <xref:System.Collections.IDictionary?displayProperty=nameWithType> 接口。 <xref:System.Collections.Generic.Dictionary%602> 泛型类还实现 <xref:System.Collections.Generic.IDictionary%602> 泛型接口。 因此，这些集合中的每个元素都是一个键值对。  
  
 <xref:System.Collections.Hashtable> 由包含集合元素的存储桶组成。 存储桶是 <xref:System.Collections.Hashtable> 中元素的虚拟子组，与在大多数集合中进行搜索和检索相比，其搜索和检索更加容易和快速。 每个存储桶都与一个哈希代码相关联，该哈希代码通过哈希函数生成并基于元素的键。  
  
 泛型 <xref:System.Collections.Generic.HashSet%601> 类是用于包含唯一元素的无序集合。  
  
 哈希函数是一种算法，返回基于键的数值哈希代码。 该键是所存储对象的某个属性的值。 哈希函数必须始终返回同一个键的同一哈希代码。 哈希函数有可能为两个不同的键生成相同的哈希代码，但从哈希表中检索元素时，为每个唯一的键生成唯一哈希代码的哈希函数具有更好的性能。  
  
 在 <xref:System.Collections.Hashtable> 中用作元素的每个对象必须能够通过使用 <xref:System.Object.GetHashCode%2A> 方法的实现为自身生成哈希代码。 但是，还可以为 <xref:System.Collections.Hashtable> 中的所有元素指定哈希函数，方法是使用接受 <xref:System.Collections.IHashCodeProvider> 实现作为其参数之一的 <xref:System.Collections.Hashtable> 构造函数。  
  
 当将对象添加到 <xref:System.Collections.Hashtable>时，其存储在与哈希代码相关联的存储桶中，此哈希代码匹配该对象的哈希代码。 当在 <xref:System.Collections.Hashtable> 中对一个值进行搜索时，则为该值生成哈希代码，并搜索与该哈希代码相关联的存储桶。  
  
 例如，用于字符串的哈希函数可能采用字符串中每个字符的 ASCII 代码，并将它们加总以生成哈希代码。 字符串“picnic”的哈希代码可能与字符串“basket”的哈希代码不同；因此，字符串“picnic”和“basket”可能在不同的存储桶中。 与此相反，“stressed”和“desserts”可能具有相同的哈希代码，并且位于同一个存储桶中。  
  
 <xref:System.Collections.Generic.Dictionary%602> 和 <xref:System.Collections.Concurrent.ConcurrentDictionary%602> 类具有与 <xref:System.Collections.Hashtable> 类相同的功能。 特定类型（不包括 <xref:System.Object>）的 <xref:System.Collections.Generic.Dictionary%602> 与 <xref:System.Collections.Hashtable> 相比可为值类型提供更好的性能。 这是因为 <xref:System.Collections.Hashtable> 的元素属于 <xref:System.Object> 类型；因此，装箱和取消装箱通常发生在存储或检索值类型时。 可能有多个线程同时访问该集合时，应使用 <xref:System.Collections.Concurrent.ConcurrentDictionary%602> 类。  
  
## <a name="see-also"></a>另请参阅

- <xref:System.Collections.Hashtable>
- <xref:System.Collections.IDictionary>
- <xref:System.Collections.IHashCodeProvider>
- <xref:System.Collections.Generic.Dictionary%602>
- <xref:System.Collections.Generic.IDictionary%602?displayProperty=nameWithType>
- <xref:System.Collections.Concurrent.ConcurrentDictionary%602?displayProperty=nameWithType>
- [常用的集合类型](commonly-used-collection-types.md)
