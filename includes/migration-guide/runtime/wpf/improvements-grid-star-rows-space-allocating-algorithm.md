---
ms.openlocfilehash: 415eb1960c20fb662469e126560d6f366309eb0d
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496828"
---
### <a name="improvements-to-grid-star-rows-space-allocating-algorithm"></a>网格星型行空格分配算法的改进

#### <a name="details"></a>详细信息

修复了在 .NET Framework 4.7 中引入的 <xref:System.Windows.Controls.Grid> 中[用于分配大小的算法](https://github.com/Microsoft/dotnet/blob/master/Documentation/compatibility/wpf-grid-allocation-of-space-to-star-columns.md)的问题。  在某些情况下，例如，<code>Height=&quot;Auto&quot;</code> 包含空行的网格，行排列在错误的位置，可能完全在网格之外。

#### <a name="suggestion"></a>建议

为使应用程序从这些更改中获益，它必须在 .NET Framework 4.8 或更高版本上运行。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |主要|
|Version|4.8|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
