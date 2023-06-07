---
ms.openlocfilehash: 3db4b0b42a154c5bc9296889ae9dc4b7ecf1e58e
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556133"
---
### <a name="allowupdatechildcontrolindexfortabcontrols-compatibility-switch-not-supported"></a>不支持 AllowUpdateChildControlIndexForTabControls 兼容性开关

`Switch.System.Windows.Forms.AllowUpdateChildControlIndexForTabControls` 兼容性开关在 .NET Framework 4.6 及更高版本上的 Windows 窗体中受支持，但在自 .NET Core 或 .NET 5.0 及更高版本中不受支持。

#### <a name="change-description"></a>更改描述

在 .NET Framework 4.6 及更高版本中，选中选项卡会对其控件集合重新排序。 借助 `Switch.System.Windows.Forms.AllowUpdateChildControlIndexForTabControls` 兼容性开关，应用程序可在不需要此类重新排序时跳过此行为。

.NET Core 和 .NET 5.0 及更高版本中尚不支持 `Switch.System.Windows.Forms.AllowUpdateChildControlIndexForTabControls` 开关。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="recommended-action"></a>建议操作

删除此开关。 此开关不受支持，且未提供替代功能。

#### <a name="category"></a>类别

Windows 窗体

#### <a name="affected-apis"></a>受影响的 API

- 无

<!-- 

#### Affected APIs

- Not detectable via API analysis

-->
