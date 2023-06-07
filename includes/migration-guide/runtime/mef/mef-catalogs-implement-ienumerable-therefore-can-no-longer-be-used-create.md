---
ms.openlocfilehash: 6f22d6211ec9238fd1c7786643ca95db02bfca64
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497144"
---
### <a name="mef-catalogs-implement-ienumerable-and-therefore-can-no-longer-be-used-to-create-a-serializer"></a>MEF 目录实现 IEnumerable，因此不能再用于创建序列化程序

#### <a name="details"></a>详细信息

从 .NET Framework 4.5 开始，MEF 目录实现 IEnumerable，因此不能再用于创建序列化程序（<xref:System.Xml.Serialization.XmlSerializer?displayProperty=fullName> 对象）。 尝试对 MEF 目录进行序列化会引发异常。

#### <a name="suggestion"></a>建议

不能再使用 MEF 创建序列化程序

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |主要|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
