---
ms.openlocfilehash: df6169289fb96df5f7daf8bd58ccaeebc33ad87c
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556134"
---
### <a name="uselegacycontextmenustripsourcecontrolvalue-compatibility-switch-not-supported"></a>不支持 UseLegacyContextMenuStripSourceControlValue 兼容性开关

已在 .NET Framework 4.7.2 中引入 `Switch.System.Windows.Forms.UseLegacyContextMenuStripSourceControlValue` 兼容性开关，但它在 .NET Core 或 .NET 5.0 及更高版本上的 Windows 窗体中尚不受支持。

#### <a name="change-description"></a>更改描述

自 .NET Framework 4.7.2 起，开发人员可使用 `Switch.System.Windows.Forms.UseLegacyContextMenuStripSourceControlValue` 兼容性开关选择退出 <xref:System.Windows.Forms.ContextMenuStrip.SourceControl?displayProperty=nameWithType> 属性的新行为 - 新行为是返回对源控件的引用。 该属性之前的行为是返回 `null`。 有关详细信息，请参阅 [\<AppContextSwitchOverrides> 元素](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)。

.NET Core 和 .NET 5.0 及更高版本中尚不支持 `Switch.System.Windows.Forms.UseLegacyContextMenuStripSourceControlValue` 开关。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="recommended-action"></a>建议操作

删除此开关。 此开关不受支持，且未提供替代功能。

#### <a name="category"></a>类别

Windows 窗体

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Forms.ContextMenuStrip.SourceControl?displayProperty=nameWithType>

<!-- 

#### Affected APIs

- `P:System.Windows.Forms.ContextMenuStrip.SourceControl`

-->
