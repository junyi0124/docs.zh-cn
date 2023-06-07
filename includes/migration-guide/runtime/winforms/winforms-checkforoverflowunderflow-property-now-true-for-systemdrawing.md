---
ms.openlocfilehash: 68b0c4bb032b9744ef585eaef3d68e31afebee24
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496737"
---
### <a name="winforms-checkforoverflowunderflow-property-is-now-true-for-systemdrawing"></a>对于 System.Drawing，WinForm 的 CheckForOverflowUnderflow 属性现在为 true

#### <a name="details"></a>详细信息

System.Drawing.dll 程序集的 CheckForOverflowUnderflow 设置为 true。

#### <a name="suggestion"></a>建议

之前在发生溢出时，结果会在不提示的情况下被截断。 现在引发了 <xref:System.OverflowException?displayProperty=fullName> 异常。

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
