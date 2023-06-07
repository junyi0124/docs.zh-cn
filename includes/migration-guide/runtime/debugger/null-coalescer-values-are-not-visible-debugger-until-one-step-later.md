---
ms.openlocfilehash: af8de731ee93d0bfb01042d894f5730570dcdd78
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496885"
---
### <a name="null-coalescer-values-are-not-visible-in-debugger-until-one-step-later"></a>在执行后一个步骤时才能在调试器中看到空联合器值

#### <a name="details"></a>详细信息

在 64 位版本的 Framework 上运行时，.NET Framework 4.5 中的 bug 导致在执行分配操作后无法立即在调试器中看到通过空联合操作设置的值。

#### <a name="suggestion"></a>建议

在调试器中多单步执行一次将正确更新本地/字段的值。 另外，此问题已在 .NET Framework 4.6 中解决；因此升级到该版本的 Framework 即可解决该问题。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
