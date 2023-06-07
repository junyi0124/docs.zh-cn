---
ms.openlocfilehash: 4a22d78f2308aab544d7a7d2e4d05ddc1ad5457d
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617129"
---
### <a name="xml-schema-validation-is-stricter"></a>XML 架构验证更为严格

#### <a name="details"></a>详细信息

在 .NET Framework 4.5 中，XML 架构验证更为严格。 如果使用 xsd:anyURI 来验证 URL（如 mailto 协议），则当 URL 中有空格时，验证将失败。 在 .NET Framework 的早期版本中，验证将成功。 此更改仅影响面向 .NET Framework 4.5 的应用程序。

#### <a name="suggestion"></a>建议

如果需要更宽松的 .NET Framework 4.0 验证，该验证应用程序可以面向版本 4.0 的 .NET Framework。 但是，重定向到 .NET Framework 4.5 时，应执行代码评审，确保无效的 URI（包含空格）不会作为 anyURI 数据类型的属性值。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   | 次要       |
| Version | 4.5         |
| 类型    | 重定目标 |
