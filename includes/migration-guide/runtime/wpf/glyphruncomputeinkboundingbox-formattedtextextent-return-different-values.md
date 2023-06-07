---
ms.openlocfilehash: d9e1624bbeb91db63bc284b8eb52643938eb42e5
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496992"
---
### <a name="glyphruncomputeinkboundingbox-and-formattedtextextent-return-different-values-beginning-in-net-framework-45"></a>从 .NET Framework 4.5 开始，GlyphRun.ComputeInkBoundingBox() 和 FormattedText.Extent 返回不同的值

#### <a name="details"></a>详细信息

.NET Framework 4.5 改进了 <xref:System.Windows.Media.GlyphRun.ComputeInkBoundingBox> 和 <xref:System.Windows.Media.FormattedText.Extent>，以解决在 .NET Framework 4.0 中，框在某些情况下因过小而无法包含字形的问题。 因此，从 .NET Framework 4.5 开始，某些范围框将更大，从而使 UI 布局产生细微差异。

#### <a name="suggestion"></a>建议

请注意，某些字形范围框大小已增加。 这些更改通常会改进展示和点击框测试，但如果需要使用旧（.NET 4.5 之前的）行为，可通过以下条目添加到 app.config 文件进行选择：<pre><code class="lang-xml">&lt;appsettings&gt;&#13;&#10;&lt;add key=&quot;IncludeAllInkInBoundingBox&quot; value=&quot;false&quot;&gt;&#13;&#10;&lt;/appsettings&gt;&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Media.GlyphRun.ComputeInkBoundingBox?displayProperty=nameWithType>
- <xref:System.Windows.Media.FormattedText.Extent?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Windows.Media.GlyphRun.ComputeInkBoundingBox`
- `P:System.Windows.Media.FormattedText.Extent`

-->
