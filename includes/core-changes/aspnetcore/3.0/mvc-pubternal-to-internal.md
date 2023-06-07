---
ms.openlocfilehash: 5741e8cdd51e00d5459c4c1032a56682429aab17
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032296"
---
### <a name="mvc-pubternal-types-changed-to-internal"></a>MVC：“Pubternal”类型更改为内部

在 ASP.NET Core 3.0 中，MVC 中的所有“pubternal”类型都已更新为 `public`（在受支持的命名空间中）或 `internal`（根据需要）。

#### <a name="change-description"></a>更改描述

在 ASP.NET Core 中，“pubternal”类型声明为 `public`，但驻留在后缀为 `.Internal` 的命名空间中。 尽管这些类型为 `public`，但它们没有支持策略，并且可能会发生中断性变更。 遗憾的是，经常会意外使用这些类型，导致对这些项目做出中断性变更，并使维护框架的能力受到限制。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

MVC 中的某些类型为 `public`，但在 `.Internal` 命名空间中。 这些类型没有支持策略，并且可能会发生中断性变更。

#### <a name="new-behavior"></a>新行为

所有此类类型都将更新为 `public`（在受支持的命名空间中）或标记为 `internal`。

#### <a name="reason-for-change"></a>更改原因

经常会意外使用“pubternal”类型，导致对这些项目做出中断性变更，并使维护框架的能力受到限制。

#### <a name="recommended-action"></a>建议操作

如果使用的类型已真正成为 `public` 且已移动到新的受支持的命名空间中，请更新引用以匹配新命名空间。

如果使用的类型已标记为 `internal`，则需要查找替代方法。 永远不支持将以前的“pubternal”类型用于公共用途。 如果这些命名空间中的特定类型对应用至关重要，请在 [dotnet/aspnetcore](https://github.com/dotnet/aspnetcore/issues) 中提出问题。 可以考虑将请求的类型设为 `public`。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

此更改包括以下命名空间中的类型：

- `Microsoft.AspNetCore.Mvc.Cors.Internal`
- `Microsoft.AspNetCore.Mvc.DataAnnotations.Internal`
- `Microsoft.AspNetCore.Mvc.Formatters.Internal`
- `Microsoft.AspNetCore.Mvc.Formatters.Json.Internal`
- `Microsoft.AspNetCore.Mvc.Formatters.Xml.Internal`
- `Microsoft.AspNetCore.Mvc.Internal`
- `Microsoft.AspNetCore.Mvc.ModelBinding.Internal`
- `Microsoft.AspNetCore.Mvc.Razor.Internal`
- `Microsoft.AspNetCore.Mvc.RazorPages.Internal`
- `Microsoft.AspNetCore.Mvc.TagHelpers.Internal`
- `Microsoft.AspNetCore.Mvc.ViewFeatures.Internal`

<!--

#### Affected APIs

- `N:Microsoft.AspNetCore.Mvc.Cors.Internal`
- `N:Microsoft.AspNetCore.Mvc.DataAnnotations.Internal`
- `N:Microsoft.AspNetCore.Mvc.Formatters.Internal`
- `N:Microsoft.AspNetCore.Mvc.Formatters.Json.Internal`
- `N:Microsoft.AspNetCore.Mvc.Formatters.Xml.Internal`
- `N:Microsoft.AspNetCore.Mvc.Internal`
- `N:Microsoft.AspNetCore.Mvc.ModelBinding.Internal`
- `N:Microsoft.AspNetCore.Mvc.Razor.Internal`
- `N:Microsoft.AspNetCore.Mvc.RazorPages.Internal`
- `N:Microsoft.AspNetCore.Mvc.TagHelpers.Internal`
- `N:Microsoft.AspNetCore.Mvc.ViewFeatures.Internal`

-->
