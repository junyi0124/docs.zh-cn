---
ms.openlocfilehash: 0c88d40e34d2d6458bb463a09d716dea42b711fe
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032260"
---
### <a name="authorization-iallowanonymous-removed-from-authorizationfiltercontextfilters"></a>授权：从 AuthorizationFilterContext.Filters 中删除 IAllowAnonymous

从 ASP.NET Core 3.0 起，MVC 不会为在控制器和操作方法上发现的 [[AllowAnonymous]](xref:Microsoft.AspNetCore.Authorization.AllowAnonymousAttribute) 属性添加 [AllowAnonymousFilters](xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter)。 此更改针对 <xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute> 派生在本地进行处理，但对于 <xref:Microsoft.AspNetCore.Mvc.Filters.IAsyncAuthorizationFilter> 和 <xref:Microsoft.AspNetCore.Mvc.Filters.IAuthorizationFilter> 实现来说是一个重大变更。 包装在 [[TypeFilter]](xref:Microsoft.AspNetCore.Mvc.TypeFilterAttribute) 属性中的此类实现是在需要配置和依赖项注入时实现强类型的基于属性的授权的[常见](https://stackoverflow.com/a/41348219/608220)的支持方式。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

<xref:Microsoft.AspNetCore.Authorization.IAllowAnonymous> 出现在 [AuthorizationFilterContext.Filters](xref:Microsoft.AspNetCore.Mvc.Filters.FilterContext.Filters%2A) 集合中。 对接口的状态进行测试是对单个控制器方法上的筛选器进行重写或禁用的一种有效方法。

#### <a name="new-behavior"></a>新行为

`IAllowAnonymous` 不再出现在 `AuthorizationFilterContext.Filters` 集合中。 依赖于旧行为的 `IAsyncAuthorizationFilter` 实现通常导致间歇性 HTTP 401 未授权或 HTTP 403 禁止响应。

#### <a name="reason-for-change"></a>更改原因

ASP.NET Core 3.0 中引入了新的终结点路由策略。

#### <a name="recommended-action"></a>建议操作

在终结点元数据中搜索 `IAllowAnonymous`。 例如：

```csharp
var endpoint = context.HttpContext.GetEndpoint();
if (endpoint?.Metadata?.GetMetadata<IAllowAnonymous>() != null)
{
}
```

[此 HasAllowAnonymous 方法](https://github.com/dotnet/aspnetcore/blob/bd65275148abc9b07a3b59797a88d485341152bf/src/Mvc/Mvc.Core/src/Authorization/AuthorizeFilter.cs#L236)中演示了此方法。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

None

<!--

#### Affected APIs

Not detectable via API analysis

-->
