---
ms.openlocfilehash: c3114277445daaae988b41782721c443c1e780d1
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496314"
---
### <a name="wpf-spawns-a-wisptisexe-process-which-can-freeze-the-mouse"></a>WPF 生成可冻结鼠标的 wisptis.exe 进程

#### <a name="details"></a>详细信息

4\.5.2 中引入了一个问题，导致生成可冻结鼠标输入的 <code>wisptis.exe</code>。

#### <a name="suggestion"></a>建议

.NET Framework 4.5.2 的服务版本（修补程序汇总 3026376）中提供了此问题的修补程序，也可通过升级到 .NET Framework 4.6 解决此问题

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |主要|
|Version|4.5.2|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
