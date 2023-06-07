---
title: 泛型接口
ms.date: 03/30/2017
helpviewer_keywords:
- generic interfaces [.NET]
- equality comparisons [.NET]
- generics [.NET], interfaces
- ordering comparisons [.NET]
ms.assetid: 88bf5b04-d371-4edb-ba38-01ec7cabaacf
ms.openlocfilehash: f8cdd7ea71e68f73871a606c6f9b754d675ca7eb
ms.sourcegitcommit: d8020797a6657d0fbbdff362b80300815f682f94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/24/2020
ms.locfileid: "95722630"
---
# <a name="generic-interfaces"></a>泛型接口

本文概述跨泛型类型系列提供通用功能的泛型接口。  
  
泛型接口提供与非泛型接口对应的类型安全接口，用于实现排序比较、相等比较以及泛型集合类型所共享的功能。  
  
> [!NOTE]
> 多个泛型接口的类型参数标记为协变或逆变，这为分配和使用实现这些接口的类型提供了更好的灵活性。 请参阅 [协变和逆变](covariance-and-contravariance.md)。  
  
## <a name="equality-and-ordering-comparisons"></a>相等比较和排序比较  

 在 <xref:System> 命名空间中，<xref:System.IComparable%601?displayProperty=nameWithType> 和 <xref:System.IEquatable%601?displayProperty=nameWithType> 泛型接口与它们对应的非泛型接口一样，各自定义了用于排序比较和相等比较的方法。 类型通过实现这些接口来提供执行这些比较的能力。  
  
 在 <xref:System.Collections.Generic> 命名空间中，<xref:System.Collections.Generic.IComparer%601> 和 <xref:System.Collections.Generic.IEqualityComparer%601> 泛型接口为没有实现 <xref:System.IComparable%601?displayProperty=nameWithType> 或 <xref:System.IEquatable%601?displayProperty=nameWithType> 泛型接口的类型提供一种定义排序比较和相等比较的方式，并为实现了上述泛型接口的类型提供了重新定义这些关系的方式。 这些接口由许多泛型集合类的方法和构造函数使用。 例如，可以将泛型 <xref:System.Collections.Generic.IComparer%601> 对象传递至 <xref:System.Collections.Generic.SortedDictionary%602> 类的构造函数，以便为没有实现泛型 <xref:System.IComparable%601?displayProperty=nameWithType> 的类型指定排列顺序。 存在 <xref:System.Array.Sort%2A?displayProperty=nameWithType> 泛型静态方法与通过泛型 <xref:System.Collections.Generic.IComparer%601> 实现对数组和列表进行排序的 <xref:System.Collections.Generic.List%601.Sort%2A?displayProperty=nameWithType> 实例方法的重载。  
  
 <xref:System.Collections.Generic.Comparer%601> 和 <xref:System.Collections.Generic.EqualityComparer%601> 泛型类为 <xref:System.Collections.Generic.IComparer%601> 和 <xref:System.Collections.Generic.IEqualityComparer%601> 泛型接口的实现提供基类，同时还通过其各自的 <xref:System.Collections.Generic.Comparer%601.Default%2A?displayProperty=nameWithType> 和 <xref:System.Collections.Generic.EqualityComparer%601.Default%2A?displayProperty=nameWithType> 属性提供默认排序比较和相等比较。  
  
## <a name="collection-functionality"></a>集合功能  

 <xref:System.Collections.Generic.ICollection%601> 泛型接口是泛型集合类型的基本接口。 它提供添加、删除、复制和枚举元素的基本功能。 <xref:System.Collections.Generic.ICollection%601> 继承自泛型 <xref:System.Collections.Generic.IEnumerable%601> 和非泛型 <xref:System.Collections.IEnumerable>。  
  
 <xref:System.Collections.Generic.IList%601> 泛型接口使用索引检索的方法扩展 <xref:System.Collections.Generic.ICollection%601> 泛型接口。  
  
 <xref:System.Collections.Generic.IDictionary%602> 泛型接口使用键控检索的方法扩展 <xref:System.Collections.Generic.ICollection%601> 泛型接口。 .NET 基类库中的泛型字典类型还实现非泛型 <xref:System.Collections.IDictionary> 接口。  
  
 <xref:System.Collections.Generic.IEnumerable%601> 泛型接口提供泛型枚举器结构。 泛型枚举器实现的 <xref:System.Collections.Generic.IEnumerator%601> 泛型接口继承自非泛型 <xref:System.Collections.IEnumerator> 接口；<xref:System.Collections.IEnumerator.MoveNext%2A> 和 <xref:System.Collections.IEnumerator.Reset%2A> 成员（不依赖于类型参数 `T`）仅出现在非泛型接口中。 这意味着非泛型接口的任何使用者还可以使用泛型接口。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Collections.Generic?displayProperty=nameWithType>
- <xref:System.Collections.ObjectModel?displayProperty=nameWithType>
- [泛型](index.md)
- [.NET 中的泛型集合](collections.md)
- [用于控制数组和列表的泛型委托](delegates-for-manipulating-arrays-and-lists.md)
- [协变和逆变](covariance-and-contravariance.md)
