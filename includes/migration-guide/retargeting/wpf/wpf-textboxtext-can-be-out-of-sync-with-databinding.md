---
ms.openlocfilehash: d0de1a262d57c67dd4dfb258d5ac013af5d8783d
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617128"
---
### <a name="wpf-textboxtext-can-be-out-of-sync-with-databinding"></a>WPF TextBox.Text 可能与数据绑定不同步

#### <a name="details"></a>详细信息

在某些情况下，如果在数据绑定的写入操作期间修改属性，则 <xref:System.Windows.Controls.TextBox.Text> 属性会反映数据绑定属性值的以前的值。

#### <a name="suggestion"></a>建议

这不应有负面影响。 不过，你可以通过将 <xref:System.Windows.FrameworkCompatibilityPreferences.KeepTextBoxDisplaySynchronizedWithTextProperty> 属性设置为 `false` 来还原以前的行为。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 边缘        |
| Version | 4.5         |
|类型|重定目标

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Windows.Controls.TextBox.Text?displayProperty=nameWithType>
