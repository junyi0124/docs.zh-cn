---
ms.openlocfilehash: 0ef39d67cd4335658658f5772b5bce49d827cad0
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614410"
---
### <a name="selector-selectionchanged-event-and-selectedvalue-property"></a>选择器 SelectionChanged 事件和 SelectedValue 属性

#### <a name="details"></a>详细信息

自 .NET Framework 4.7.1 起，在引发 <xref:System.Windows.Controls.Primitives.Selector.SelectionChanged> 事件之前，<xref:System.Windows.Controls.Primitives.Selector> 始终在选择更改时更新其 <xref:System.Windows.Controls.Primitives.Selector.SelectedValue%2A> 属性值。 这使得 SelectedValue 属性与其他选择属性（<xref:System.Windows.Controls.Primitives.Selector.SelectedItem%2A> 和 <xref:System.Windows.Controls.Primitives.Selector.SelectedIndex%2A>）一致，这些选择属性在引发事件之前更新。<p/>在 .NET Framework 4.7 和更早版本中，大多数情况下会在事件之前更新 SelectedValue，但如果选择更改是由 <xref:System.Windows.Controls.Primitives.Selector.SelectedValue%2A> 属性更改引起的，则在事件之后进行更新。

#### <a name="suggestion"></a>建议

对于面向 .NET Framework 4.7.1 或更高版本的应用，可以在应用程序配置文件的 `<runtime>` 部分添加以下内容，从而选择弃用此更改并启用旧版行为：

<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides&#13;&#10;value=&quot;Switch.System.Windows.Controls.TabControl.SelectionPropertiesCanLagBehindSelectionChangedEvent=true&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>

对于面向 .NET Framework 4.7 或更早版本，但在 .NET Framework 4.7.1 或更高版本上运行的应用，可以在应用程序配置文件的 `<runtime>` 部分中添加以下代码行，从而启用新的行为：

<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Controls.TabControl.SelectionPropertiesCanLagBehindSelectionChangedEvent=false&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.7.1       |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Controls.TabControl.SelectedContent?displayProperty=nameWithType>
- <xref:System.Windows.Controls.Primitives.Selector.SelectionChanged?displayProperty=nameWithType>
