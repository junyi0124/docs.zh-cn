---
ms.openlocfilehash: d6405573e476ce18513938b500041adefbeeef1b
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497087"
---
### <a name="ribbongroup-background-is-set-to-transparent-in-localized-builds"></a>本地化版本中的 RibbonGroup 背景设置为透明

#### <a name="details"></a>详细信息

始终用透明画笔绘制本地化版本上的 <xref:System.Windows.Controls.Ribbon.RibbonGroup?displayProperty=fullName> 背景，导致 UI 体验不佳。 .NET Framework 4.7 WPF 修复中通过更新 <xref:System.Windows.Controls.Ribbon.RibbonGroup?displayProperty=fullName> 本地化资源修复了此问题，因而又确保会选中正确的画笔。

#### <a name="suggestion"></a>建议

升级到 .NET Framework 4.7

| “属性”    | 值       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.6.2|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
