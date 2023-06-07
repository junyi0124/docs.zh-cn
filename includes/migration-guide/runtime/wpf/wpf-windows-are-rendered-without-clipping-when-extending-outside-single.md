---
ms.openlocfilehash: a133c2ea48df11915f8708029c70549e8a1ccefd
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497524"
---
### <a name="wpf-windows-are-rendered-without-clipping-when-extending-outside-a-single-monitor"></a>当 WPF 窗口超出单个显示屏时，会不经剪辑就呈现该窗口

#### <a name="details"></a>详细信息

在 Windows 8 及更高版本上运行的 .NET Framework 4.6 中，当一个窗口超出多监视器方案中的单个显示屏时，会不经剪辑就呈现整个窗口。 这与先前版本的 .NET Framework 不同，以前的版本在 WPF 窗口超出单个显示屏时会剪辑该窗口。

#### <a name="suggestion"></a>建议

可以使用应用程序的配置文件中的 <code>&lt;appSettings&gt;</code> 的 <code>&lt;EnableMultiMonitorDisplayClipping&gt;</code> 元素，或在应用启动时设置 <code>EnableMultiMonitorDisplayClipping</code> 属性，显式设置此行为（无论剪辑与否）。

| 名称    | 值       |
|:--------|:------------|
| 范围   |次要|
|Version|4.6|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
