---
ms.openlocfilehash: 31ada197db36be192317e32a37a353d375d9cc65
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497445"
---
### <a name="scrolling-a-wpf-treeview-or-grouped-listbox-in-a-virtualizingstackpanel-can-cause-a-hang"></a>在 VirtualizingStackPanel 中滚动 WPF TreeView 或分组的 ListBox 可能导致暂停

#### <a name="details"></a>详细信息

在 .NET Framework v4.5 中，如果视区中存在边距（例如在 <xref:System.Windows.Controls.TreeView?displayProperty=fullName> 的项目之间或在 ItemsPresenter 元素上），则在虚拟化堆叠面板中滚动 WPF <xref:System.Windows.Controls.TreeView?displayProperty=fullName> 可能会导致暂停。 此外，在某些情况下，即使没有边距，视图中不同大小的项目也可能会导致不稳定性。

#### <a name="suggestion"></a>建议

升级到 .NET Framework 4.5.1 可避免此 bug 出现。 或者，如果包含的所有项目大小相同，可从虚拟化堆叠面板的视图集合（例如 <xref:System.Windows.Controls.TreeView?displayProperty=fullName>）中删除边距。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |主要|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Controls.VirtualizingStackPanel.SetIsVirtualizing(System.Windows.DependencyObject,System.Boolean)?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Windows.Controls.VirtualizingStackPanel.SetIsVirtualizing(System.Windows.DependencyObject,System.Boolean)`

-->
