---
ms.openlocfilehash: b23182ad1da8208a69b78fc7bc872780d386a59a
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496212"
---
### <a name="minfreememorypercentagetoactiveservice-is-now-respected"></a>MinFreeMemoryPercentageToActiveService 现在受到遵从

#### <a name="details"></a>详细信息

此设置可确立激活 WCF 服务之前必须在服务器上可用的最小内存。 它旨在阻止 <xref:System.OutOfMemoryException?displayProperty=fullName> 异常。 在 .NET Framework 4.5 中，此设置没有效果。 在 .NET Framework 4.5.1 中，观察到该设置。

#### <a name="suggestion"></a>建议

如果 Web 服务器上的可用内存少于由配置设置定义的百分比，将引发异常。 某些成功启动并且在受约束的内存环境中运行的 WCF 服务现在可能失败。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |次要|
|Version|4.5.1|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
