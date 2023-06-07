---
ms.openlocfilehash: 7c2e80669d9ef27f87cde9442d8eeab0ee31a524
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614420"
---
### <a name="wpf-textboxpasswordbox-text-selection-does-not-follow-system-colors"></a>WPF TextBox/PasswordBox 文本选择不遵循系统颜色

#### <a name="details"></a>详细信息

在 .NET Framework 4.7.1 和更早版本中，WPF `System.Windows.Controls.TextBox` 和 `System.Windows.Controls.PasswordBox` 只能在装饰器层呈现文本选择。 在某些系统主题中，这会遮蔽文本，使其难以阅读。  在 .NET Framework 4.7.2 及更高版本中，开发人员可选择启用基于非装饰器的选择呈现方案，从而缓解此问题。

#### <a name="suggestion"></a>建议

开发人员若要利用此更改，必须正确设置以下 AppContext 标记。  若要利用此功能，已安装的 .NET Framework 必须是 4.7.2 或更高版本。若要启用基于非装饰器的选择，请使用以下 AppContext 标记。<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Controls.Text.UseAdornerForTextboxSelectionRendering=false&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.7.2       |
| 类型    | 重定目标 |
