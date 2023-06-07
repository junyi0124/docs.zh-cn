---
ms.openlocfilehash: 224cd3c7897c64ef05baba7d3d31dbe5ac0dd610
ms.sourcegitcommit: 27a15a55019f6b5f2733961738babe94aec0def3
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/15/2020
ms.locfileid: "90606539"
---
### <a name="pubternal-apis-removed"></a>已删除“Pubternal”API

为了更好地维护 ASP.NET Core 面向公众的 API，`*.Internal` 命名空间中的大多数类型（称为 :::no-loc text="\"pubternal\""::: API）已成为真正的内部类型。 这些命名空间中的成员永远不会作为面向公众的 API 得到支持。 API 可能在次要版本中中断（这种情况经常会发生）。 在更新到 ASP.NET Core 3.0 时，依赖于这些 API 的代码会中断。

有关详细信息，请参阅 [dotnet/aspnetcore#4932](https://github.com/dotnet/aspnetcore/issues/4932) 和 [dotnet/aspnetcore#11312](https://github.com/dotnet/aspnetcore/issues/11312)。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

受影响的 API 使用 `public` 访问修饰符进行标记，并存在于 `*.Internal` 命名空间中。

#### <a name="new-behavior"></a>新行为

受影响的 API 使用 [internal](../../../../docs/csharp/language-reference/keywords/internal.md) 访问修饰符进行标记，不能再供该程序集外部的代码使用。

#### <a name="reason-for-change"></a>更改原因

对于这些 :::no-loc text="\"pubternal\""::: API，指导原则是：

* 它们可能会更改，恕不另行通知。
* 它们不遵从 .NET 策略来防止中断性变更。

保留这些 API `public` （即使保留在 `*.Internal` 命名空间中）会对客户造成混淆。

#### <a name="recommended-action"></a>建议操作

停止使用这些 :::no-loc text="\"pubternal\""::: API。 如果对其他 API 有任何疑问，请在 [dotnet/aspnetcore](https://github.com/dotnet/aspnetcore/issues) 存储库中提问。

例如，请考虑 ASP.NET Core 2.2 项目中的以下 HTTP 请求缓冲代码。 `EnableRewind` 扩展方法存在于 `Microsoft.AspNetCore.Http.Internal` 命名空间中。

```csharp
HttpContext.Request.EnableRewind();
```

在 ASP.NET Core 3.0 项目中，将 `EnableRewind` 调用替换为对 `EnableBuffering` 扩展方法的调用。 请求缓冲功能的工作方式与过去相同。 `EnableBuffering` 立即调用 `internal` API。

```csharp
HttpContext.Request.EnableBuffering();
```

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

名称中具有 `Internal` 段的 `Microsoft.AspNetCore.*` 和 `Microsoft.Extensions.*` 命名空间中的所有 API。 例如：

- `Microsoft.AspNetCore.Authentication.Internal`
- `Microsoft.AspNetCore.Builder.Internal`
- `Microsoft.AspNetCore.DataProtection.Cng.Internal`
- `Microsoft.AspNetCore.DataProtection.Internal`
- `Microsoft.AspNetCore.Hosting.Internal`
- `Microsoft.AspNetCore.Http.Internal`
- `Microsoft.AspNetCore.Mvc.Core.Infrastructure`
- `Microsoft.AspNetCore.Mvc.Core.Internal`
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
- `Microsoft.AspNetCore.Rewrite.Internal`
- `Microsoft.AspNetCore.Routing.Internal`
- `Microsoft.AspNetCore.Server.Kestrel.Core.Adapter.Internal`
- `Microsoft.AspNetCore.Server.Kestrel.Core.Internal.Http`
- `Microsoft.AspNetCore.Server.Kestrel.Core.Internal.Infrastructure`
- `Microsoft.AspNetCore.Server.Kestrel.Https.Internal`

<!--

#### Affected APIs

- `N:Microsoft.AspNetCore.Authentication.Internal`
- `N:Microsoft.AspNetCore.Builder.Internal`
- `N:Microsoft.AspNetCore.DataProtection.Cng.Internal`
- `N:Microsoft.AspNetCore.DataProtection.Internal`
- `N:Microsoft.AspNetCore.Hosting.Internal`
- `N:Microsoft.AspNetCore.Http.Internal`
- `N:Microsoft.AspNetCore.Mvc.Core.Infrastructure`
- `N:Microsoft.AspNetCore.Mvc.Core.Internal`
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
- `N:Microsoft.AspNetCore.Rewrite.Internal`
- `N:Microsoft.AspNetCore.Routing.Internal`
- `N:Microsoft.AspNetCore.Server.Kestrel.Core.Adapter.Internal`
- `N:Microsoft.AspNetCore.Server.Kestrel.Core.Internal.Http`
- `N:Microsoft.AspNetCore.Server.Kestrel.Core.Internal.Infrastructure`
- `N:Microsoft.AspNetCore.Server.Kestrel.Https.Internal`

-->
