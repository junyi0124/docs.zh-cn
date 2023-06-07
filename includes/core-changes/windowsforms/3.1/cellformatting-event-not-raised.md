---
ms.openlocfilehash: 4a34a64eba72ea24c1d830566565ce4fbee8e5b7
ms.sourcegitcommit: 0926684d8d34f4c6b5acce58d2193db093cb9cf2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/20/2020
ms.locfileid: "83720946"
---
### <a name="cellformatting-event-not-raised-if-tooltip-is-shown"></a>如果显示工具提示，则不引发 CellFormatting 事件

现在，当鼠标悬停和通过键盘选择时，<xref:System.Windows.Forms.DataGridView> 将显示单元格的文本和错误工具提示。 如果显示工具提示，则不会引发 <xref:System.Windows.Forms.DataGridView.CellFormatting?displayProperty=nameWithType> 事件。

#### <a name="change-description"></a>更改描述

在 .NET Core 3.1 之前，将 <xref:System.Windows.Forms.DataGridView.ShowCellToolTips%2A> 属性设置为 `true` 的 <xref:System.Windows.Forms.DataGridView> 会在鼠标悬停在单元格上方时显示单元格文本和错误的工具提示。 之前，通过键盘选择单元格时（例如通过使用 Tab 键、快捷键或箭头导航），不显示工具提示。 如果用户编辑了单元格，然后在 <xref:System.Windows.Forms.DataGridView> 仍处于编辑模式时将鼠标悬停在未设置 <xref:System.Windows.Forms.DataGridViewCell.ToolTipText> 属性的单元格上，则会引发 <xref:System.Windows.Forms.DataGridView.CellFormatting> 事件，对要在单元格中显示的单元格文本进行格式化。

为满足辅助功能标准，自 .NET Core 3.1 起，将 <xref:System.Windows.Forms.DataGridView.ShowCellToolTips%2A> 属性设置为 `true` 的 <xref:System.Windows.Forms.DataGridView> 不仅在鼠标悬停在单元格上时会显示单元格文本和错误的工具提示，而且在通过键盘选择单元格时也会显示。 由于这一变更，如果鼠标在 <xref:System.Windows.Forms.DataGridView> 处于编辑模式时悬停在未设置 <xref:System.Windows.Forms.DataGridViewCell.ToolTipText> 属性的单元格上，不会引发 <xref:System.Windows.Forms.DataGridView.CellFormatting> 事件  。 不引发该事件的原因是鼠标悬停的单元格的内容显示为工具提示，而不是显示在单元格中。

#### <a name="version-introduced"></a>引入的版本

3.1

#### <a name="recommended-action"></a>建议操作

当 <xref:System.Windows.Forms.DataGridView> 处于编辑模式时，对依赖 <xref:System.Windows.Forms.DataGridView.CellFormatting> 事件的所有代码进行重构。

#### <a name="category"></a>类别

Windows 窗体

#### <a name="affected-apis"></a>受影响的 API

None

<!-- 

#### Affected APIs

Not detectable via API analysis.

-->
