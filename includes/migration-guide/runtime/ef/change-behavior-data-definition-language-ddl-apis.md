---
ms.openlocfilehash: 4416a7c09f2d163961fe2fe3d6dfaa8bd5e66f93
ms.sourcegitcommit: cbacb5d2cebbf044547f6af6e74a9de866800985
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/05/2020
ms.locfileid: "89497700"
---
### <a name="change-in-behavior-in-data-definition-language-ddl-apis"></a>数据定义语言 (DDL) API 行为的更改

#### <a name="details"></a>详细信息

指定 AttachDBFilename 时，DDL API 的行为已更改，具体如下：<ul><li>连接字符串不需要指定 Initial Catalog 值。 以前，需要 AttachDBFilename 和 Initial Catalog。</li><li>如果指定了 AttachDBFilename 和 Initial Catalog，并且存在给定的 MDF 文件，<xref:System.Data.Objects.ObjectContext.DatabaseExists%2A> 方法将返回 <code>true</code>。 以前，它会返回 <code>false</code>。</li><li>如果指定了 AttachDBFilename 和 Initial Catalog，并且存在给定的 MDF 文件，则调用 <xref:System.Data.Objects.ObjectContext.DeleteDatabase%2A> 方法会删除这些文件。</li><li>如果在连接字符串指定一个 AttachDBFilename 值，且不存在 MDF 和 Initial Catalog 时调用 <xref:System.Data.Objects.ObjectContext.DeleteDatabase%2A>，则该方法将引发 <xref:System.InvalidOperationException> 异常。 以前，它会引发 <xref:System.Data.SqlClient.SqlException> 异常。</li></ul>

#### <a name="suggestion"></a>建议

利用这些更改，可以更轻松地生成使用 DDL API 的工具和应用程序。 这些更改会影响以下方案中的应用程序兼容性：<ul><li>如果 <code>DROP DATABASE</code> 返回 <xref:System.Data.Objects.ObjectContext.DeleteDatabase%2A>，则用户会编写直接执行 <xref:System.Data.Objects.ObjectContext.DatabaseExists%2A> 命令而不是调用 <code>true</code> 的代码。 如果未附加数据库但存在 MDF 文件，则会中断现有代码。</li><li>用户编写的代码预期 <xref:System.Data.Objects.ObjectContext.DeleteDatabase%2A> 方法在 Initial Catalog 和 MDF 文件不存在时引发 <xref:System.Data.SqlClient.SqlException> 而非 <xref:System.InvalidOperationException>。</li></ul>

| “属性”    | “值”       |
|:--------|:------------|
| 范围   |次要|
|Version|4.5|
|类型|运行时|

#### <a name="affected-apis"></a>受影响的 API

无法通过 API 分析检测到。

<!--

#### Affected APIs

Not detectable via API analysis.

-->
