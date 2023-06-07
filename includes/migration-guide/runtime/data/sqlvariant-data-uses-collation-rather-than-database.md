---
ms.openlocfilehash: fb339fb35cdcbba4f1c860fae9c17162c4cff596
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496801"
---
### <a name="sql_variant-data-uses-sql_variant-collation-rather-than-database-collation"></a>Sql_variant 数据使用 sql_variant 排序规则而不是数据库排序规则

#### <a name="details"></a>详细信息

<code>sql_variant</code> 数据使用 <code>sql_variant</code> 排序规则而不是数据库排序规则。

#### <a name="suggestion"></a>建议

如果数据库排序规则与 <code>sql_variant</code> 排序规则不同，则此更改将解决可能的数据损坏。 依赖损坏的数据的应用程序可能会失败。

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |透明|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
