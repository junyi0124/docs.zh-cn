---
ms.openlocfilehash: 58dbb73902c0226fa81acf1a70de2160f406f6c6
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032256"
---
### <a name="authorization-iauthorizationpolicyprovider-implementations-require-new-method"></a>授权：IAuthorizationPolicyProvider 实现需要新方法

在 ASP.NET Core 3.0 中，已将一个新 `GetFallbackPolicyAsync` 方法添加到 `IAuthorizationPolicyProvider`。 当未指定策略时，授权中间件会使用此回退策略。

有关详细信息，请参阅 [dotnet/aspnetcore#9759](https://github.com/dotnet/aspnetcore/pull/9759)。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

`IAuthorizationPolicyProvider` 的实现不需要 `GetFallbackPolicyAsync` 方法。

#### <a name="new-behavior"></a>新行为

`IAuthorizationPolicyProvider` 的实现需要 `GetFallbackPolicyAsync` 方法。

#### <a name="reason-for-change"></a>更改原因

如果未指定策略，则新 `AuthorizationMiddleware` 需要使用新方法。

#### <a name="recommended-action"></a>建议操作

将 `GetFallbackPolicyAsync` 方法添加到 `IAuthorizationPolicyProvider` 的实现。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.AspNetCore.Authorization.IAuthorizationPolicyProvider?displayProperty=fullName>

<!-- 

#### Affected APIs

`T:Microsoft.AspNetCore.Authorization.IAuthorizationPolicyProvider`

-->
