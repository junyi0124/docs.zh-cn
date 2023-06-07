---
ms.openlocfilehash: 346fb6ecd43f7f93529e45f169c79b7acacc9c1f
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89496450"
---
### <a name="opt-in-break-to-revert-from-different-45-sql-generation-to-simpler-40-sql-generation"></a>选择中断以从不同的 4.5 SQL 生成还原到更简单的 4.0 SQL 生成

#### <a name="details"></a>详细信息

生成 JOIN 语句并包含对限制操作的调用（无需先使用 OrderBy）的查询现在可以生成更简单的 SQL。 升级到 .NET Framework 4.5 后，这些查询生成了比以前版本更复杂的 SQL。

#### <a name="suggestion"></a>建议

在默认情况下，禁用此功能。 如果实体框架生成可导致性能降低的额外 JOIN 语句，则可以通过将以下项添加到应用程序配置 (app.config) 文件的 <code>&lt;appSettings&gt;</code> 部分来启用此功能：<pre><code class="lang-xml">&lt;add key=&quot;EntityFramework_SimplifyLimitOperations&quot; value=&quot;true&quot; /&gt;&#13;&#10;</code></pre>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |透明|
|Version|4.5.2|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
