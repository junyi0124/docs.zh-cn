---
ms.openlocfilehash: a82b720fd4e771481ea1142a88a095443afa0d5b
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614411"
---
### <a name="keyboard-focus-now-moves-correctly-across-multiple-layers-of-winformswpf-hosting"></a>现在，键盘焦点可在 WinForms/WPF 承载的多个层之间正确移动

#### <a name="details"></a>详细信息

请考虑 WPF 应用程序，其承载的 WinForms 控件会反过来承载 WPF 控件。 如果该层的第一个或最后一个控件是 WPF `System.Windows.Forms.Integration.ElementHost`，那么用户可能不能按 Tab 离开 WinForms 层。 此更改修复了这个问题，现在，用户能按 Tab 离开 WinForms layer.Automated 应用程序。这些应用程序依赖于绝不转义 WinForms 层的焦点可能不再按预期工作。

#### <a name="suggestion"></a>建议

如果开发人员想要利用此更改，但面向 .NET 4.7.2 以下的框架版本，他可将下列一组 AppContext 标记设置为 false，从而启用更改。

<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

WPF 应用程序必须选择启用所有早期的可访问性改进，才能使用之后的改进。 换言之，必须同时设置 `Switch.UseLegacyAccessibilityFeatures` 和 `Switch.UseLegacyAccessibilityFeatures.2` 开关。如果开发人员需要以前的功能，且面向 .NET 4.7.2 或更高版本，他可将以下 AppContext 标记设置为 false，从而禁用更改。

<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.UseLegacyAccessibilityFeatures.2=true&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.7.2       |
| 类型    | 重定目标 |
