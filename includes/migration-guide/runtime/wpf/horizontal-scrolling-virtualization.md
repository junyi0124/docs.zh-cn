---
ms.openlocfilehash: 2ae17e0823ec2fa064c948d9ea7bd19cbd34cb6a
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497632"
---
### <a name="horizontal-scrolling-and-virtualization"></a>水平滚动和虚拟化

#### <a name="details"></a>详细信息

此更改适用于按与主滚动方向正交的方向自行执行虚拟化的 <xref:System.Windows.Controls.ItemsControl?displayProperty=fullName>（主要示例是 EnableColumnVirtualization=&quot;True&quot; 的 <xref:System.Windows.Controls.DataGrid?displayProperty=fullName>）。  已将特定水平滚动操作更改为生成更直观且更类似于可类比垂直滚动操作的结果。<p/>操作包括&quot;滚动到此处&quot;和&quot;滚动到右边缘&quot;（使用右键单击水平滚动条所获得的菜单中的名称）。  这两种方法都能计算候选偏移和调用 <xref:System.Windows.Controls.Primitives.IScrollInfo.SetHorizontalOffset(System.Double)>。<p/>滚动到新偏移后，&quot;滚动到此处&quot;或&quot;滚动到右边缘&quot;的概念可能会发生变化，因为反虚拟化的新内容已更改 <xref:System.Windows.Controls.Primitives.IScrollInfo.ExtentWidth?displayProperty=fullName> 的值。<p/>在低于 .NET Framework 4.6.2 的版本中，滚动操作仅使用候选偏移，即使不再位于&quot;此处&quot;或&quot;右边缘&quot;，也不例外。  这会导致滚动翻阅出现“退回”等效果，最好是用举例来说明。 假设 <xref:System.Windows.Controls.DataGrid?displayProperty=fullName> 有 ExtentWidth=1000 和 Width=200。  “滚动到右边缘”使用候选偏移 1000 - 200 = 800。  在滚动到此偏移时，新列会进行反虚拟化；假设这些列都非常宽，这样就可以将 <xref:System.Windows.Controls.Primitives.IScrollInfo.ExtentWidth?displayProperty=fullName> 更改为 2000。  滚动偏移最终为 HorizontalOffset=800，而翻阅&quot;退回&quot;到滚动条中间位置附近，精确位置是 800/2000 = 40%。<p/>此更改是为了在发生此情况时重新计算新的候选偏移，并进行重试。 （垂直滚动已采用这样的做法。） <p/>此更改为最终用户提供了更易于预测的直观体验，但还可能会影响依赖水平滚动后的 <xref:System.Windows.Controls.Primitives.IScrollInfo.HorizontalOffset?displayProperty=fullName> 确切值的所有应用，不论水平滚动是由最终用户触发，还是由显式调用 <xref:System.Windows.Controls.Primitives.IScrollInfo.SetHorizontalOffset(System.Double)> 所触发。

#### <a name="suggestion"></a>建议

应将使用 <xref:System.Windows.Controls.Primitives.IScrollInfo.HorizontalOffset?displayProperty=fullName> 预测值的应用更改为：在由于反虚拟化而可能更改 <xref:System.Windows.Controls.Primitives.IScrollInfo.ExtentWidth?displayProperty=fullName> 的任何水平滚动发生后提取实际值（和 <xref:System.Windows.Controls.Primitives.IScrollInfo.ExtentWidth?displayProperty=fullName> 值）。

| 名称    | 值       |
|:--------|:------------|
| 范围   |次要|
|Version|4.6.2|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Controls.Primitives.IScrollInfo?displayProperty=nameWithType>

<!--

#### Affected APIs

- `T:System.Windows.Controls.Primitives.IScrollInfo`

-->
