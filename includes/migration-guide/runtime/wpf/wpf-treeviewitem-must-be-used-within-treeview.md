---
ms.openlocfilehash: e9f769af6d85403c2a6f085cbc3462cf549adae9
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496898"
---
### <a name="wpf-treeviewitem-must-be-used-within-a-treeview"></a>WPF TreeViewItem 必须在 TreeView 内使用

#### <a name="details"></a>详细信息

4\.5 中引入了一项更改，即限制在 <xref:System.Windows.Controls.TreeView?displayProperty=fullName> 外使用 <xref:System.Windows.Controls.TreeViewItem?displayProperty=fullName> 元素。 在下列条件下，可能会出现这种情况：<ul><li><xref:System.Windows.Controls.TreeViewItem?displayProperty=fullName> 的视觉父级不是面板。 （为 <xref:System.Windows.Controls.TreeView?displayProperty=fullName> 生成的 <xref:System.Windows.Controls.TreeViewItem?displayProperty=fullName> 将有一个面板作为其父级）</li><li><xref:System.Windows.Controls.TreeViewItem?displayProperty=fullName> 是充当列表控件（ListBox、DataGrid、ListView 等）“项目主机”的 <xref:System.Windows.Controls.VirtualizingStackPanel?displayProperty=fullName> 的后代。 无需启用虚拟化。</li><li><xref:System.Windows.Controls.VirtualizingStackPanel?displayProperty=fullName> 进行项滚动 (<code>ScrollUnit=&quot;Item&quot;</code>)。</li><li>有些用户调用 <code>VirtualizingStackPanel.MakeVisible(v)</code> 以将元素 <code>v</code> 滚动至视图。 这可通过许多方法显式或隐式实现；或许最常见的方法是，只需单击 <code>v</code> 即可为其提供键盘焦点。</li><li>视觉父链贯穿整个 <xref:System.Windows.Controls.TreeViewItem?displayProperty=fullName>，范围从 <code>v</code> 到 <xref:System.Windows.Controls.VirtualizingStackPanel?displayProperty=fullName>。</li></ul>换言之，在 <xref:System.Windows.Controls.TreeView?displayProperty=fullName> 之外使用 <xref:System.Windows.Controls.TreeViewItem?displayProperty=fullName>，并且用户单击 <xref:System.Windows.Controls.TreeViewItem?displayProperty=fullName> 的某个后代以显示它后，便可显示。 如果 <xref:System.Windows.Controls.TreeViewItem?displayProperty=fullName> 的后代都没有焦点，将永远不会遇到此问题。 例如，如果 <xref:System.Windows.Controls.TreeViewItem?displayProperty=fullName> 是 DataTemplate 的根，则会出现此问题。 发生此问题时，WPF 框架中将引发 InvalidCastException。

#### <a name="suggestion"></a>建议

将向此问题提供修补程序。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |次要|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
