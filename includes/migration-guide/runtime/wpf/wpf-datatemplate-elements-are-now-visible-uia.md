---
ms.openlocfilehash: 4394e69dafeb6cce2d7719a67bbce396d3bc1086
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497722"
---
### <a name="wpf-datatemplate-elements-are-now-visible-to-uia"></a>WPF DataTemplate 元素现在对 UIA 可见

#### <a name="details"></a>详细信息

以前 <xref:System.Windows.DataTemplate?displayProperty=fullName> 元素对 UI 自动化不可见。 从 4.5 开始，UI 自动化将能检测到这些元素。 这在许多情况下很有用，但可能会中断依赖于不包含 <xref:System.Windows.DataTemplate?displayProperty=fullName> 元素的 UIA 树的测试。

#### <a name="suggestion"></a>建议

此应用的 UI 自动化测试需要更新，以便考虑到 UIA 树现在所包含的以前不可见的 <xref:System.Windows.DataTemplate?displayProperty=fullName> 元素。 例如，对于预计某些元素彼此相邻的测试，现在需要考虑到其间之前不可见的 UIA 元素。 否则，依赖 UIA 元素的特定计数或索引的测试可能需要使用新值进行更新。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.DataTemplate.%23ctor>
- <xref:System.Windows.DataTemplate.%23ctor(System.Object)>

<!--

#### Affected APIs

- `M:System.Windows.DataTemplate.#ctor`
- `M:System.Windows.DataTemplate.#ctor(System.Object)`

-->
