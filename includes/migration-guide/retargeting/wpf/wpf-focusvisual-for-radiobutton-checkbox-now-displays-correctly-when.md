---
ms.openlocfilehash: 9e70bcd95393a7ff24de43577d26869ce674067b
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614416"
---
### <a name="wpf-focusvisual-for-radiobutton-and-checkbox-now-displays-correctly-when-the-controls-have-no-content"></a>RadioButton 和 CheckBox 的 WPF FocusVisual 现可在控件无内容时正确显示

#### <a name="details"></a>详细信息

在 .NET Framework 4.7.1 和更早版本中，WPF <xref:System.Windows.Controls.CheckBox?displayProperty=nameWithType> 和 <xref:System.Windows.Controls.RadioButton?displayProperty=nameWithType> 不一致且在经典和高对比度主题中具有不正确的焦点视觉对象。  控件没有内容集时会出现这些问题。  这会使得主题间的转换变得混乱且难以看到焦点视觉对象。 现在，在 .NET Framework 4.7.2 中，主题间的这些视觉对象更加一致，并且在经典和高对比度主题中更轻松可见。

#### <a name="suggestion"></a>建议

如果开发人员面向 .NET Framework 4.7.2，但要还原到 .NET 4.7.1 行为，则需要设置以下 AppContext 标记。

<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.UseLegacyAccessibilityFeatures.2=true;&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

如果开发人员面向 .NET 4.7.2 以下的框架版本，但想要利用此更改，则必须设置以下 AppContext 标记。请注意，必须正确设置所有标记，并且安装的 .NET Framework 版本必须是 4.7.2 或更高版本。WPF 应用程序需选择启用所有的早期辅助功能改进，才能获取最新改进。 若要执行此操作，请确保将“Switch.UseLegacyAccessibilityFeatures”和“Switch.UseLegacyAccessibilityFeatures.2”这两个 AppContext 开关设置为 false。

<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.7.2       |
| 类型    | 重定目标 |
