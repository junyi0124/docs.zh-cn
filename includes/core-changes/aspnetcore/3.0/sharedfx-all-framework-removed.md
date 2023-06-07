---
ms.openlocfilehash: 959f3959c28c7d0159be7a213986345e2865b9a2
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032410"
---
### <a name="shared-framework-removed-microsoftaspnetcoreall"></a>共享框架：已删除 Microsoft.AspNetCore.All

从 ASP.NET Core 3.0 开始，不再生成 `Microsoft.AspNetCore.All` 元包和匹配的 `Microsoft.AspNetCore.All` 共享框架。 此包在 ASP.NET Core 2.2 中提供，并将继续接收 ASP.NET Core 2.1 中的服务更新。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

应用可以使用 `Microsoft.AspNetCore.All` 元包来针对 .NET Core 上的 `Microsoft.AspNetCore.All` 共享框架。

#### <a name="new-behavior"></a>新行为

.NET Core 3.0 不包含 `Microsoft.AspNetCore.All` 共享框架。

#### <a name="reason-for-change"></a>更改原因

`Microsoft.AspNetCore.All` 元包包含了大量外部依赖项。

#### <a name="recommended-action"></a>建议操作

迁移项目以使用 `Microsoft.AspNetCore.App` 框架。 以前在 `Microsoft.AspNetCore.All` 中可用的组件在 NuGet 上仍然可用。 这些组件现可与应用一起部署，而不是包含在共享框架中。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

None

<!-- 

#### Affected APIs

Not detectable via API analysis

-->
