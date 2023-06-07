---
ms.openlocfilehash: 05f60978f5380c406c43aa98ded0c812b1d50694
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496279"
---
### <a name="enumerableemptylttresultgt-always-returns-cached-instance"></a>Enumerable.Empty&lt;TResult&gt; 始终返回缓存的实例

#### <a name="details"></a>详细信息

从 .NET Framework 4.5 开始，<xref:System.Linq.Enumerable.Empty%60%601> 始终返回缓存的内部实例 <xref:System.Collections.Generic.IEnumerable%601>。以前，在调用 API 时，<xref:System.Linq.Enumerable.Empty%60%601> 将缓存空 <xref:System.Collections.Generic.IEnumerable%601>，这意味着在同时快速调用 <xref:System.Linq.Enumerable.Empty%60%601> 的某些情况下，针对 API 的不同调用将返回不同的类型实例。

#### <a name="suggestion"></a>建议

因为以前的行为不确定，所以代码不太可能依赖它。 但是，如果出现需要比较空枚举并希望它们有时不相等这一罕见情形，应创建显式空数组 (<code>new T[0]</code>)，而非使用 <xref:System.Linq.Enumerable.Empty%60%601>。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Linq.Enumerable.Empty%60%601?displayProperty=nameWithType>

<!--

#### Affected APIs

- ``M:System.Linq.Enumerable.Empty``1``

-->
