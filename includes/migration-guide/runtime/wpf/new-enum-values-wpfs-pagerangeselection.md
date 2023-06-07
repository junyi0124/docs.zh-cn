---
ms.openlocfilehash: 61749f59f9379a6d18bb013b2612a07cb7822b3a
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497119"
---
### <a name="new-enum-values-in-wpfs-pagerangeselection"></a>WPF PageRangeSelection 中的新枚举值

#### <a name="details"></a>详细信息

两个新成员（<xref:System.Windows.Controls.PageRangeSelection.CurrentPage?displayProperty=fullName> 和 <xref:System.Windows.Controls.PageRangeSelection.SelectedPages?displayProperty=fullName>）已添加到 <xref:System.Windows.Controls.PageRangeSelection?displayProperty=fullName> 枚举。

#### <a name="suggestion"></a>建议

大多数情况下，这些更改不会影响用户代码。 但是，应该修改依赖于对 <xref:System.Windows.Controls.PageRangeSelection?displayProperty=fullName> 类型的 <xref:System.Enum.GetNames(System.Type)> 或 <xref:System.Enum.GetValues(System.Type)> 调用中所存在特定数量元素的代码。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Controls.PageRangeSelection?displayProperty=nameWithType>

<!--

#### Affected APIs

- `T:System.Windows.Controls.PageRangeSelection`

-->
