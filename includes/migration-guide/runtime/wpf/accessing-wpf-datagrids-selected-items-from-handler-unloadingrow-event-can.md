---
ms.openlocfilehash: 1487e32ffca7b4bbebb5edac7efc8961ac05723b
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497541"
---
### <a name="accessing-a-wpf-datagrids-selected-items-from-a-handler-of-the-datagrids-unloadingrow-event-can-cause-a-nullreferenceexception"></a>从 DataGrid 的 UnloadingRow 事件的处理程序访问 WPF DataGrid 的选定项可能会导致 NullReferenceException

#### <a name="details"></a>详细信息

由于 .NET Framework 4.5 中的 bug，涉及删除行的 <xref:System.Windows.Controls.DataGrid> 事件的事件处理程序如果访问 <xref:System.Windows.Controls.DataGrid> 的 <xref:System.Windows.Controls.Primitives.Selector.SelectedItem?displayProperty=fullName> 或 <xref:System.Windows.Controls.Primitives.MultiSelector.SelectedItems?displayProperty=fullName> 属性，可能会导致引发 <xref:System.NullReferenceException?displayProperty=fullName>。

#### <a name="suggestion"></a>建议

此问题已在 .NET Framework 4.6 中解决，因此升级到该版本的 .NET Framework 即可解决该问题。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |次要|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Controls.DataGrid.UnloadingRow?displayProperty=nameWithType>
- <xref:System.Windows.Controls.DataGrid.UnloadingRowDetails?displayProperty=nameWithType>

<!--

#### Affected APIs

- `E:System.Windows.Controls.DataGrid.UnloadingRow`
- `E:System.Windows.Controls.DataGrid.UnloadingRowDetails`

-->
