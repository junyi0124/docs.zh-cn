---
ms.openlocfilehash: fccf349517133245ec85ae3c25cedbfb27a7dd8b
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497680"
---
### <a name="the-replace-method-in-odata-urls-is-disabled-by-default"></a>默认情况下，禁用 OData URL 中的 Replace 方法

#### <a name="details"></a>详细信息

从 .NET Framework 4.5 开始，默认会禁用 OData URL 中的 Replace 方法。 在禁用 OData Replace 时（现在是默认设置），任何包含 replace 函数的用户请求（这并不常见）都将失败。

#### <a name="suggestion"></a>建议

如果需要 replace 方法（这并不常见），可通过配置设置 (<xref:System.Data.Services.Configuration.DataServicesFeaturesSection.ReplaceFunction?displayProperty=fullName>) 重新启用。 但是，启用的 replace 方法可能会带来安全漏洞，应仅在仔细检查后使用。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.Data.Services.DataService%601?displayProperty=nameWithType>

<!--

#### Affected APIs

- ``T:System.Data.Services.DataService`1``

-->
