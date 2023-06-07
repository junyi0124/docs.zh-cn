---
ms.openlocfilehash: d4e60f2a59980263916718ebcc71cc359952c031
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496951"
---
### <a name="wpf-spell-checking-fails-in-unexpected-ways"></a>WPF 拼写检查以一种意想不到的方式失败

#### <a name="details"></a>详细信息

这包括 WPF 拼写检查器的诸多问题：<ul><li>WPF 拼写检查器有时会引发 <xref:System.Runtime.InteropServices.COMException?displayProperty=fullName></li><li>当使用“以不同用户身份运行”启动应用程序时，WPF 拼写检查器会无法使用 <xref:System.UnauthorizedAccessException></li><li>WPF 拼写检查器在组合词（如德语中的“Hausnummer”）中错误地标出了拼写错误。</li></ul>

#### <a name="suggestion"></a>建议

问题 #1 - 此问题已在 .NET Framework 4.6.2 中解决 问题 #2 - 当使用“以不同用户身份运行”启动应用程序时，不再支持 WPF 拼写检查器。 从 .NET Framework 4.6.2 开始，以此方式启动的应用程序将不再意外出现故障 - 而是静默禁用 WPF 拼写检查器。 问题 #3 - 此问题已在 .NET Framework 4.6.2 中解决。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.6.1|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
