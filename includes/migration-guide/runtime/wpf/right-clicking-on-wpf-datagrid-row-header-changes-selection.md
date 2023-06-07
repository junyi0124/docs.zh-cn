---
ms.openlocfilehash: e1faee846627b22b88eb888d6241d47d8ea6ea06
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497210"
---
### <a name="right-clicking-on-a-wpf-datagrid-row-header-changes-the-datagrid-selection"></a>右键单击 WPF DataGrid 行标题更改 DataGrid 选择

#### <a name="details"></a>详细信息

在选中多行时，右键单击所选的 <xref:System.Windows.Controls.DataGrid?displayProperty=fullName> 行标题会导致 <xref:System.Windows.Controls.DataGrid?displayProperty=fullName> 更改为仅选择该行。

#### <a name="suggestion"></a>建议

此问题已在 .NET Framework 4.6 中解决，因此升级到该版本的 .NET Framework 即可解决该问题。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Controls.DataGrid.%23ctor>

<!--

#### Affected APIs

- `M:System.Windows.Controls.DataGrid.#ctor`

-->
