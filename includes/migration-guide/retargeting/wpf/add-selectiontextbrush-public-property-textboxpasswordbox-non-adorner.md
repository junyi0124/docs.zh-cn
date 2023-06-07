---
ms.openlocfilehash: aade03c29ff16053a97683499cf719a98ae33f3f
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617790"
---
### <a name="add-selectiontextbrush-public-property-to-textboxpasswordbox-non-adorner-selection"></a>将 SelectionTextBrush 公共属性添加到 TextBox/PasswordBox 非装饰器选择功能

#### <a name="details"></a>详细信息

在使用 <xref:System.Windows.Controls.TextBox> 和 <xref:System.Windows.Controls.PasswordBox> 的[基于非装饰器的文本选择功能](https://github.com/Microsoft/dotnet/blob/master/Documentation/compatibility/wpf-TextBox-PasswordBox-text-selection-does-not-follow-system-colors.md)的 WPF 应用程序中，开发人员现在可以设置新添加的 SelectionTextBrush 属性，以便更改所选文本的呈现。  默认情况下，通过 <xref:System.Windows.SystemColors.HighlightTextBrushKey> 更改此颜色。  如果未启用基于非修饰器的文本选择功能，则此属性不执行任何操作。

#### <a name="suggestion"></a>建议

启用基于非装饰器的文本选择功能后，可以使用 <xref:System.Windows.Controls.PasswordBox.SelectionTextBrush?displayProperty=nameWithType> 和 <xref:System.Windows.Controls.Primitives.TextBoxBase.SelectionTextBrush> 属性更改所选文本的外观。 可以使用 XAML 实现此操作：

<pre><code class="lang-xaml">&lt;TextBox SelectionBrush=&quot;Red&quot; SelectionTextBrush=&quot;White&quot;  SelectionOpacity=&quot;0.5&quot;&#13;&#10;Foreground=&quot;Blue&quot; CaretBrush=&quot;Blue&quot;&gt;&#13;&#10;This is some text.&#13;&#10;&lt;/TextBox&gt;&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 主要       |
| Version | 4.8         |
| 类型    | 重定目标 |

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Controls.Primitives.TextBoxBase.SelectionTextBrushProperty?displayProperty=nameWithType>
- <xref:System.Windows.Controls.Primitives.TextBoxBase.SelectionTextBrush?displayProperty=nameWithType>
- [System.Windows.Controls.TextBox](xref:System.Windows.Controls.TextBox)
- [System.Windows.Controls.PasswordBox](xref:System.Windows.Controls.PasswordBox)
