---
ms.openlocfilehash: c78122a2fe69c78625d6cb7fa9ddf41c49c2e737
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497175"
---
### <a name="calling-datagridcommitedit-from-a-celleditending-handler-drops-focus"></a>从 CellEditEnding 处理程序调用 DataGrid.CommitEdit 将丢失焦点

#### <a name="details"></a>详细信息

从 <xref:System.Windows.Controls.DataGrid?displayProperty=fullName> 的一个 <xref:System.Windows.Controls.DataGrid.CellEditEnding?displayProperty=fullName> 事件处理程序中调用 <xref:System.Windows.Controls.DataGrid.CommitEdit> 导致 <xref:System.Windows.Controls.DataGrid?displayProperty=fullName> 失去焦点。

#### <a name="suggestion"></a>建议

此 bug 已在 .NET Framework 4.5.2 中得到修复，因此升级 .NET Framework 可避免出现此问题。 或者，可通过在调用 <xref:System.Windows.Controls.DataGrid.CommitEdit?displayProperty=fullName> 后显式重新选择 <xref:System.Windows.Controls.DataGrid?displayProperty=fullName> 避免出现此问题。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Controls.DataGrid.CommitEdit?displayProperty=nameWithType>
- <xref:System.Windows.Controls.DataGrid.CommitEdit(System.Windows.Controls.DataGridEditingUnit,System.Boolean)?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:System.Windows.Controls.DataGrid.CommitEdit`
- `M:System.Windows.Controls.DataGrid.CommitEdit(System.Windows.Controls.DataGridEditingUnit,System.Boolean)`

-->
