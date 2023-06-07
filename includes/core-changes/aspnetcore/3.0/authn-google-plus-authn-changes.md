---
ms.openlocfilehash: 14d80f25c34465caa6dec10e5704fe0a3cbccc71
ms.sourcegitcommit: 721c3e4bdbb1ea0bb420818ec944c538fe5c513a
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/01/2020
ms.locfileid: "96476746"
---
### <a name="authentication-google-deprecated-and-replaced"></a>身份验证：Google+ 已弃用并被替换

Google 早在 2019 年 1 月 28 日就开始[关闭](https://developers.google.com/+/api-shutdown)对应用使用 Google + 登录。

#### <a name="change-description"></a>更改描述

ASP.NET 4.x 和 ASP.NET Core 已使用 Google + 登录 API 在 Web 应用中对 Google 帐户用户进行身份验证。 受影响的 NuGet 包为 [Microsoft.AspNetCore.Authentication.Google](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Google/)（对于 ASP.NET Core）和 [Microsoft.Owin.Security.Google](https://www.nuget.org/packages/Microsoft.Owin.Security.Google/)（对于具有 ASP.NET Web Forms 和 MVC 的 `Microsoft.Owin`）。

Google 的替代 API 使用不同的数据源和格式。 下面提供的缓解措施和解决方案说明存在结构性更改。 应用应验证数据本身是否仍然满足其需求。 例如，名称、电子邮件地址、配置文件链接和个人资料照片的值可能与以前稍有不同。

#### <a name="version-introduced"></a>引入的版本

所有版本。 此更改是 ASP.NET Core 的外部更改。

#### <a name="recommended-action"></a>建议操作

##### <a name="owin-with-aspnet-web-forms-and-mvc"></a>包含 ASP.NET Web Forms 和 MVC 的 Owin

针对 `Microsoft.Owin` 3.1.0 和更高版本，[本文](https://github.com/aspnet/AspNetKatana/issues/251#issuecomment-449587635)概述了临时的缓解措施。 应用应通过缓解措施来完成测试，以检查数据格式的更改。 计划发布 `Microsoft.Owin` 4.0.1，并提供一个修补程序。 使用任何较低版本的应用都应更新到版本 4.0.1。

##### <a name="aspnet-core-1x"></a>ASP.NET Core 1.x

[包含 ASP.NET Web Form 和MVC 的 Owin](#owin-with-aspnet-web-forms-and-mvc)的缓解措施可应用于 ASP.NET Core 1.x。 未计划 NuGet 包修补程序，因为 1.x 已处于[生命周期结束](https://dotnet.microsoft.com/platform/support-policy)状态。

##### <a name="aspnet-core-2x"></a>ASP.NET Core 2.x

对于 `Microsoft.AspNetCore.Authentication.Google` 版本 2.x，请将对 `Startup.ConfigureServices` 中 `AddGoogle` 的现有调用替换为以下代码：

```csharp
.AddGoogle(o =>
{
    o.ClientId = Configuration["Authentication:Google:ClientId"];
    o.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
    o.UserInformationEndpoint = "https://www.googleapis.com/oauth2/v2/userinfo";
    o.ClaimActions.Clear();
    o.ClaimActions.MapJsonKey(ClaimTypes.NameIdentifier, "id");
    o.ClaimActions.MapJsonKey(ClaimTypes.Name, "name");
    o.ClaimActions.MapJsonKey(ClaimTypes.GivenName, "given_name");
    o.ClaimActions.MapJsonKey(ClaimTypes.Surname, "family_name");
    o.ClaimActions.MapJsonKey("urn:google:profile", "link");
    o.ClaimActions.MapJsonKey(ClaimTypes.Email, "email");
});
```

2 月 2.1 和 2.2 修补程序将前面的重新配置合并为新的默认配置。 不会为 ASP.NET Core 2.0 计划任何修补程序，因为它的[生命周期已经结束](https://dotnet.microsoft.com/platform/support-policy)。

##### <a name="aspnet-core-30"></a>ASP.NET Core 3.0

为 ASP.NET Core 2.x 提供的缓解措施也可用于 ASP.NET Core 3.0。 未来的 3.0 预览版中，可能会删除 `Microsoft.AspNetCore.Authentication.Google` 包。 改为将用户定向到 `Microsoft.AspNetCore.Authentication.OpenIdConnect`。 以下代码演示了如何在 `Startup.ConfigureServices` 中将 `AddGoogle` 替换为 `AddOpenIdConnect`。 此替换可以用于 ASP.NET Core 2.0 及更高版本，并且可以根据需要应用于 ASP.NET Core 1.x。

```csharp
.AddOpenIdConnect("Google", o =>
{
    o.ClientId = Configuration["Authentication:Google:ClientId"];
    o.ClientSecret = Configuration["Authentication:Google:ClientSecret"];
    o.Authority = "https://accounts.google.com";
    o.ResponseType = OpenIdConnectResponseType.Code;
    o.CallbackPath = "/signin-google"; // Or register the default "/signin-oidc"
    o.Scope.Add("email");
});
JwtSecurityTokenHandler.DefaultInboundClaimTypeMap.Clear();
```

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.AspNetCore.Authentication.Google?displayProperty=fullName>

<!-- 

#### Affected APIs

`N:Microsoft.AspNetCore.Authentication.Google`

-->
