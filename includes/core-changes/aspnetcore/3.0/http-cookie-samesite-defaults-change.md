---
ms.openlocfilehash: 15ba678431b97e7c961c119d83546569bdf9bad2
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032272"
---
### <a name="http-some-cookie-samesite-defaults-changed-to-none"></a>HTTP：某些 cookie SameSite 默认值更改为“None”

`SameSite` 是 cookie 的一个选项，可以帮助减轻某些跨站点请求伪造 (CSRF) 攻击。 最初引入此选项时，各种 ASP.NET Core API 中使用了不一致的默认值。 不一致会导致结果混乱。 从 ASP.NET Core 3.0 开始，这些默认值更一致了。 必须为每个组件选择启用此功能。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

类似的 ASP.NET Core API 使用不同的默认值 <xref:Microsoft.AspNetCore.Http.SameSiteMode>。 在 `HttpResponse.Cookies.Append(String, String)` 和 `HttpResponse.Cookies.Append(String, String, CookieOptions)` 中可以看到不一致的示例，它们分别默认为 `SameSiteMode.None` 和 `SameSiteMode.Lax`。

#### <a name="new-behavior"></a>新行为

所有受影响的 API 都默认为 `SameSiteMode.None`。

#### <a name="reason-for-change"></a>更改原因

更改了默认值，使 `SameSite` 成为可选功能。

#### <a name="recommended-action"></a>建议操作

发出 cookie 的每个组件都需要决定 `SameSite` 是否适用于其方案。 检查受影响 API 的使用情况，并根据需要重新配置 `SameSite`。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

- <xref:Microsoft.AspNetCore.Http.IResponseCookies.Append(System.String,System.String,Microsoft.AspNetCore.Http.CookieOptions)?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy%2A?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:Microsoft.AspNetCore.Http.IResponseCookies.Append(System.String,System.String,Microsoft.AspNetCore.Http.CookieOptions)`
- `Overload:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy`

-->
