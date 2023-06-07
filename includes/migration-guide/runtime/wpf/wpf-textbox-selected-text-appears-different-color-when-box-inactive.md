---
ms.openlocfilehash: c01705002ad87a12389078d75ffd0f91f934d36e
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497654"
---
### <a name="wpf-textbox-selected-text-appears-a-different-color-when-the-text-box-is-inactive"></a>WPF 文本框选定文本在文本框处于非活动状态时，将显示其他颜色

#### <a name="details"></a>详细信息

在 .NET Framework 4.5 中，当 WPF 文本框控件处于非活动状态（该控件没有焦点）时，文本框中选定文本所显示的颜色与该控件处于活动状态时所显示的颜色不同。

#### <a name="suggestion"></a>建议

可通过将 <xref:System.Windows.FrameworkCompatibilityPreferences.AreInactiveSelectionHighlightBrushKeysSupported> 属性设置为 <code>false</code> 来还原以前的 (.NET Framework 4.0) 行为。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Controls.TextBox?displayProperty=nameWithType>

<!--

#### Affected APIs

- `T:System.Windows.Controls.TextBox`

-->
