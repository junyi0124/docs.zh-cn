---
ms.openlocfilehash: 3fb8807a9b6f0bb0d2bc746f5e89eaa8a81d6aa8
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556137"
---
### <a name="donotsupportselectallshortcutinmultilinetextbox-compatibility-switch-not-supported"></a>不支持 DoNotSupportSelectAllShortcutInMultilineTextBox 兼容性开关

已在 .NET Framework 4.6.1 中引入 `Switch.System.Windows.Forms.DoNotSupportSelectAllShortcutInMultilineTextBox` 兼容性开关，但它在 .NET Core 和 .NET 5.0 及更高版本上的 Windows 窗体中尚不受支持。

#### <a name="change-description"></a>更改描述

自 .NET Framework 4.6.1 起，在 <xref:System.Windows.Forms.TextBox> 控件中选择 <kbd>Ctrl</kbd> + <kbd>A</kbd> 快捷键会选中所有文本。 在 .NET Framework 4.6 及更低版本中，如果 [Textbox.ShortcutsEnabled](xref:System.Windows.Forms.TextBoxBase.ShortcutsEnabled) 和 <xref:System.Windows.Forms.TextBox.Multiline?displayProperty=nameWithType> 属性都设置为 `true`，则选择 <kbd>Ctrl</kbd> + <kbd>A</kbd> 快捷键没法选中全部文本。 为保留原始行为，已在 .NET Framework 4.6.1 中引入 `Switch.System.Windows.Forms.DoNotSupportSelectAllShortcutInMultilineTextBox` 兼容性开关。 有关更多信息，请参见<xref:System.Windows.Forms.TextBox.ProcessCmdKey%2A?displayProperty=nameWithType>。

.NET Core 和 .NET 5.0 及更高版本中尚不支持 `Switch.System.Windows.Forms.DoNotSupportSelectAllShortcutInMultilineTextBox` 开关。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="recommended-action"></a>建议操作

删除此开关。 此开关不受支持，且未提供替代功能。

#### <a name="category"></a>类别

Windows 窗体

#### <a name="affected-apis"></a>受影响的 API

- None

<!-- 

#### Affected APIs

- Not detectable via API analysis

-->
