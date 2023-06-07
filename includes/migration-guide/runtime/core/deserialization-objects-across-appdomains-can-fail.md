---
ms.openlocfilehash: 3f82a4daac3b5d8981532f0c82e9a76f13c68b6e
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496542"
---
### <a name="deserialization-of-objects-across-appdomains-can-fail"></a>跨应用域的对象的反序列化可能失败

#### <a name="details"></a>详细信息

在某些情况下，当应用使用两个或更多带有不同应用程序基的应用域时，尝试跨应用域在逻辑调用上下文中为对象反序列化将引发异常。

#### <a name="suggestion"></a>建议

请参阅[缓解措施：跨应用域的对象反序列化](~/docs/framework/migration-guide/mitigation-deserialization-of-objects-across-app-domains.md)

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |边缘|
|Version|4.5.1|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
