---
ms.openlocfilehash: 3244913e06a744dafc4440f824ebe6bed25b8dd1
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497071"
---
### <a name="objectdisposedexception-thrown-by-wpf-spellchecker"></a>WPF 拼写检查器引发的 ObjectDisposedException

#### <a name="details"></a>详细信息

应用程序关闭，拼写检查器引发 <xref:System.ObjectDisposedException?displayProperty=fullName>，在这期间，WPF 应用程序有时会出现故障。 .NET Framework 4.7 WPF 中已通过妥善处理异常解决了此问题，因此确保了应用程序不再受到不良影响。 应注意，在调试器控制下运行的应用程序中会继续观察到偶尔引发的最可能异常。

#### <a name="suggestion"></a>建议

升级到 .NET Framework 4.7

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
