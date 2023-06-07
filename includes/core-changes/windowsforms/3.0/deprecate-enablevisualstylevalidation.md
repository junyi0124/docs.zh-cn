---
ms.openlocfilehash: 196a26bd235e5e2556baa7fac979b3316ff81e1f
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556130"
---
### <a name="enablevisualstylevalidation-compatibility-switch-not-supported"></a>不支持 EnableVisualStyleValidation 兼容性开关

.NET Core 或 .NET 5.0 及更高版本上的 Windows 窗体不支持 `Switch.System.Windows.Forms.EnableVisualStyleValidation` 兼容性开关。

#### <a name="change-description"></a>更改描述

在 .NET Framework 中，`Switch.System.Windows.Forms.EnableVisualStyleValidation` 兼容性开关使得应用程序可选择不验证以数值格式提供的视觉样式。

.NET Core 和 .NET 5.0 及更高版本中尚不支持 `Switch.System.Windows.Forms.EnableVisualStyleValidation` 开关。

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
