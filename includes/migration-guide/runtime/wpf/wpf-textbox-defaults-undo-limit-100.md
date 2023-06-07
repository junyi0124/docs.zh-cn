---
ms.openlocfilehash: 9d960774161fc44810f90ca30f56eb98f98de3ff
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497419"
---
### <a name="wpf-textbox-defaults-to-undo-limit-of-100"></a>WPF 文本框默认撤消限制是 100

#### <a name="details"></a>详细信息

在 .NET Framework 4.5 中，WPF 文本框的默认撤消限制是 100（.NET Framework 4.0 则没有限制）

#### <a name="suggestion"></a>建议

如果 100 的撤消限制过低，可使用 <xref:System.Windows.Controls.Primitives.TextBoxBase.UndoLimit> 显式设置该限制

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
