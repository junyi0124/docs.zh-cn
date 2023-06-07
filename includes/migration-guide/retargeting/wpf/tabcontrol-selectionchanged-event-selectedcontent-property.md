---
ms.openlocfilehash: dfdb62e8dd6db67d4fd12aba93928f4615e3f284
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614412"
---
### <a name="tabcontrol-selectionchanged-event-and-selectedcontent-property"></a>TabControl SelectionChanged 事件和 SelectedContent 属性

#### <a name="details"></a>详细信息

从 .NET Framework 4.7.1 开始，当选择发生变化时，<xref:System.Windows.Controls.TabControl> 在引发 <xref:System.Windows.Controls.Primitives.Selector.SelectionChanged> 事件之前更新其 <xref:System.Windows.Controls.TabControl.SelectedContent> 属性的值。而在 .NET Framework 4.7 和更早版本中，在事件发生之后才更新到 SelectedContent。

#### <a name="suggestion"></a>建议

对于面向 .NET Framework 4.7.1 或更高版本的应用，可以在应用程序配置文件的 `<runtime>` 部分添加以下内容，从而选择弃用此更改并启用旧版行为：

<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Controls.TabControl.SelectionPropertiesCanLagBehindSelectionChangedEvent=true&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>

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
