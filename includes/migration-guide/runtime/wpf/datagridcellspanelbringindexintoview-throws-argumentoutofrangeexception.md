---
ms.openlocfilehash: 961ca545560a53fc2c1d52722b68ae460de66877
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496336"
---
### <a name="datagridcellspanelbringindexintoview-throws-argumentoutofrangeexception"></a>DataGridCellsPanel.BringIndexIntoView 引发 ArgumentOutOfRangeException

#### <a name="details"></a>详细信息

启用列虚拟化但尚未确定列宽时，<xref:System.Windows.Controls.DataGrid.ScrollIntoView(System.Object)> 将以异步方式执行工作。  如果在异步工作执行之前删除列，可能会出现 <xref:System.ArgumentOutOfRangeException?displayProperty=fullName>。

#### <a name="suggestion"></a>建议

以下任一项：<ol><li>升级到 .NET Framework 4.7。</li><li>安装 .NET Framework 4.6.2 的最新服务修补程序。</li><li>在对 <xref:System.Windows.Controls.DataGrid.ScrollIntoView(System.Object)> 的异步响应完成前，避免删除列。</li></ol>

| 名称    | 值       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.6.2|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Controls.DataGrid.ScrollIntoView(System.Object)?displayProperty=nameWithType>
- <xref:System.Windows.Controls.DataGrid.ScrollIntoView(System.Object,System.Windows.Controls.DataGridColumn)?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Windows.Controls.DataGrid.ScrollIntoView(System.Object)`
- `M:System.Windows.Controls.DataGrid.ScrollIntoView(System.Object,System.Windows.Controls.DataGridColumn)`

-->
