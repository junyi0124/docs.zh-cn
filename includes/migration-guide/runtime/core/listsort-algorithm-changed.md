---
ms.openlocfilehash: 09bd2c6312493f8b6369d05d8f1c4e88e4c05ece
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496484"
---
### <a name="listsort-algorithm-changed"></a>List.Sort 算法已更改

#### <a name="details"></a>详细信息

从 .NET Framework 4.5 开始，<xref:System.Collections.Generic.List%601?displayProperty=fullName> 的排序算法就已更改（为内省排序而不是快速排序）。 <xref:System.Collections.Generic.List%601?displayProperty=fullName> 的排序一直都不稳定，但此更改可能会导致各种方案以不稳定的方式进行排序。 这仅仅意味着等效项目可能会在随后的 API 调用中按不同顺序排序。

#### <a name="suggestion"></a>建议

由于旧的排序算法也不稳定（尽管方式略有不同），因此应该没有代码依赖于始终按特定顺序排序的等效项。 如果有代码实例依赖于此等效项，并且可以幸运地使用旧行为，则此代码应更新为使用按照所需顺序对项进行确定性排序的比较器。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |透明|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Collections.Generic.List%601.Sort?displayProperty=nameWithType>
- <xref:System.Collections.Generic.List%601.Sort(System.Collections.Generic.IComparer{%600})?displayProperty=nameWithType>
- <xref:System.Collections.Generic.List%601.Sort(System.Comparison{%600})?displayProperty=nameWithType>
- <xref:System.Collections.Generic.List%601.Sort(System.Int32,System.Int32,System.Collections.Generic.IComparer{%600})?displayProperty=nameWithType>

<!--

#### Affected APIs

- ``M:System.Collections.Generic.List`1.Sort``
- ``M:System.Collections.Generic.List`1.Sort(System.Collections.Generic.IComparer{`0})``
- ``M:System.Collections.Generic.List`1.Sort(System.Comparison{`0})``
- ``M:System.Collections.Generic.List`1.Sort(System.Int32,System.Int32,System.Collections.Generic.IComparer{`0})``

-->
