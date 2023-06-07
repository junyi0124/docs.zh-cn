---
ms.openlocfilehash: 0b2d3c1383246d4259c6d906ecf9dab927f4bdb1
ms.sourcegitcommit: 0926684d8d34f4c6b5acce58d2193db093cb9cf2
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 05/20/2020
ms.locfileid: "83720966"
---
### <a name="default-control-font-changed-to-segoe-ui-9-pt"></a>默认控件字体更改为 Segoe UI 9 pt

#### <a name="change-description"></a>更改描述

在 .NET Framework 中，<xref:System.Windows.Forms.Control.DefaultFont?displayProperty=nameWithType> 属性设置为 `Microsoft Sans Serif 8 pt`。 下图显示了使用默认字体的窗口。

![.NET Framework 中的默认控件字体](~/docs/images/core-changes/windowsforms/control-defaultfont-changed/defaultfont-framework.png)

从 .NET Core 3.0 开始，默认字体设置为 `Segoe UI 9 pt`（与 <xref:System.Drawing.SystemFonts.MessageBoxFont?displayProperty=nameWithType> 相同的字体）。 作为此更改的结果，窗体和控件的大小会增加约 27%，以适应新默认字体的更大大小。 例如:

![.NET Core 中的默认控件字体](~/docs/images/core-changes/windowsforms/control-defaultfont-changed/defaultfont-core.png)

此更改是为了与 [Windows 用户体验 (UX) 准则](/windows/win32/uxguide/vis-fonts#fonts-and-colors)保持一致。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="recommended-action"></a>建议操作

由于窗体和控件的大小改变，因此请确保应用程序能够正确呈现。

要保留原始字体，请将窗体的默认字体设置为 `Microsoft Sans Serif 8 pt`。 例如:

```csharp
public MyForm()
{
    InitializeComponent();
    Font = new Font(new FontFamily("Microsoft Sans Serif"), 8f);
}
```

#### <a name="category"></a>类别

- Windows 窗体

#### <a name="affected-apis"></a>受影响的 API

无。

<!--

#### Affected APIs

- Not detectable via API analysis

-->
