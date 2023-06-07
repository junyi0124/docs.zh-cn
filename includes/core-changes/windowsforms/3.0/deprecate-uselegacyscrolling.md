---
ms.openlocfilehash: ee4a27dde9f1e7756401e3d8b709514082f5d3b1
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556135"
---
### <a name="domainupdownuselegacyscrolling-compatibility-switch-not-supported"></a>不支持 DomainUpDown.UseLegacyScrolling 兼容性开关

已在 .NET Framework 4.7.1 中引入 `Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling` 兼容性开关，但它在 .NET Core 或 .NET 5.0 及更高版本上的 Windows 窗体中尚不受支持。

#### <a name="change-description"></a>更改描述

自 .NET Framework 4.7.1 起，开发人员可使用 `Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling` 兼容性开关选择不执行独立 <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType> 和 <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> 操作。 该开关会还原旧行为，也就是说如果存在上下文文本，则忽略 <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType>而且开发人员需要先在控件上执行 <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType> 操作，然后才能执行 <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> 操作。 有关详细信息，请参阅 [\<AppContextSwitchOverrides> 元素](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)。

.NET Core 和 .NET 5.0 及更高版本中尚不支持 `Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling` 开关。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="recommended-action"></a>建议操作

删除此开关。 此开关不受支持，且未提供替代功能。

#### <a name="category"></a>类别

Windows 窗体

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType>

<!-- 

#### Affected APIs

- `M:System.Windows.Forms.DomainUpDown.DownButton`
- `M:System.Windows.Forms.DomainUpDown.UpButton`

-->
