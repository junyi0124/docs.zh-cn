---
ms.openlocfilehash: 972601874d80d82ebae8b79779acfed82e5570cb
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496632"
---
### <a name="calling-itemsrefresh-on-a-wpf-listbox-listview-or-datagrid-with-items-selected-can-cause-duplicate-items-to-appear-in-the-element"></a>在选定项目的 WPF ListBox、ListView 或 DataGrid 上调用 Items.Refresh 可能会导致元素中出现重复项目

#### <a name="details"></a>详细信息

在 .NET Framework 4.5 中，在项目已在 <xref:System.Windows.Controls.ListBox?displayProperty=fullName> 中选定的同时从代码中调用 ListBox.Items.Refresh 可能会导致选定项目在列表中重复。 <xref:System.Windows.Controls.ListView?displayProperty=fullName> 和 <xref:System.Windows.Controls.DataGrid?displayProperty=fullName> 会出现类似问题。 此问题已在 .NET Framework 4.6 中解决。

#### <a name="suggestion"></a>建议

在调用 <xref:System.Windows.Data.CollectionView.Refresh?displayProperty=fullName> 前以编程方式取消选择项，然后在调用完成后重新选择它们，可以解决此问题。 此外，此问题已在 .NET Framework 4.6 中解决，因此升级到该版本的 .NET Framework 即可解决该问题。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |次要|
|Version|4.5|
|类型|运行时

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Data.CollectionView.Refresh?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Windows.Data.CollectionView.Refresh`

-->
