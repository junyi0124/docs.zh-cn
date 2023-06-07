---
title: 关于 .NET 微服务和 Web 应用中的授权
description: .NET 微服务和 Web 应用程序中的安全性 - 了解 ASP .NET Core 应用程序中的主要授权选项（基于角色和基于策略）。
author: mjrousos
ms.date: 01/30/2020
ms.openlocfilehash: 27936a33ea2bb46cedb9d10ee47a2117e1843e14
ms.sourcegitcommit: e3cbf26d67f7e9286c7108a2752804050762d02d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2020
ms.locfileid: "80988201"
---
# <a name="about-authorization-in-net-microservices-and-web-applications"></a>关于 .NET 微服务和 Web 应用中的授权

身份验证之后，ASP.NET Core Web API 需要授予访问权限。 该过程允许服务向部分通过身份验证的用户提供 API，但不是向所有用户提供。 [身份验证](/aspnet/core/security/authorization/introduction)可基于用户角色或自定义策略完成，这可能包括检查声明或试探法。

限制 ASP.NET Core MVC 路由的访问权限十分简单，即将 Authorize 特性应用到操作方法（或者，如果所有控制器的操作都需要身份验证，则应用到控制器的类），如以下示例所示：

```csharp
public class AccountController : Controller
{
    public ActionResult Login()
    {
    }

    [Authorize]
    public ActionResult Logout()
    {
    }
}
```

默认情况下，添加不具有参数的 Authorize 特性将限制已通过验证的用户对该控制器或操作的访问权限。 为了将 API 进一步限制于仅向特定用户提供，该特性可扩展为指定用户必须满足的必需角色或策略。

## <a name="implement-role-based-authorization"></a>实现基于角色的授权

ASP.NET Core 标识具有角色的内置概念。 除了用户，ASP.NET Core 标识存储应用程序使用的不同角色的相关信息，并跟踪哪些用户分配到了哪些角色。 可以使用更新持久存储中的角色的 `RoleManager` 类型和可以授予或撤销用户角色的 `UserManager` 类型以编程方式更改这些分配。

如果使用 JWT 持有者令牌进行身份验证，ASP.NET Core JWT 持有者身份验证中间件将基于令牌中找到的角色声明填充用户角色。 为了将对 MVC 操作或控制器的访问权限限制于特定角色中的用户，可在 Authorize 批注（特性）中包含 Roles 参数，如以下代码段所示：

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [Authorize(Roles = "Administrator")]
    public ActionResult ShutDown()
    {
    }
}
```

在此示例中，仅管理员或 PowerUser 角色中的用户可访问 ControlPanel 控制器中的 API（如执行 SetTime 操作）。 ShutDown API 已进一步限制为仅允许访问 Administrator 角色的用户。

若要用户处于多种角色，则使用多个 Authorize 特性，如以下示例所示：

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
[Authorize(Roles = "RemoteEmployee ")]
[Authorize(Policy = "CustomPolicy")]
public ActionResult API1 ()
{
}
```

在此示例中，要调用 API1，用户必须：

- 具有 Administrator 或 PowerUser 角色；以及   

- 处于 RemoteEmployee 角色； 

- 满足 CustomPolicy 授权的自定义处理程序。

## <a name="implement-policy-based-authorization"></a>实现基于策略的授权

还可使用[授权策略](https://docs.asp.net/en/latest/security/authorization/policies.html)编写自定义授权规则。 以下部分进行了概述。 有关详细信息，请参阅 [ASP.NET 授权研讨会](https://github.com/blowdart/AspNetAuthorizationWorkshop)。

Startup.ConfigureServices 方法中使用 service.AddAuthorization 方法注册了自定义授权策略。 此方法采用了配置 AuthorizationOptions 参数的委托。

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("AdministratorsOnly", policy =>
        policy.RequireRole("Administrator"));

    options.AddPolicy("EmployeesOnly", policy =>
        policy.RequireClaim("EmployeeNumber"));

    options.AddPolicy("Over21", policy =>
        policy.Requirements.Add(new MinimumAgeRequirement(21)));
});
```

如示例所示，策略可与不同类型的要求相关联。 注册策略后，可通过将策略名称作为 Authorize 特性的 Policy 参数传递，将这些策略应用于操作或控制器（例如，`[Authorize(Policy="EmployeesOnly")]`），策略可有多个要求，而非仅有一个（如这些示例所示）。

在之前的示例中，第一个 AddPolicy 调用只是按角色授权的替代方式。 如果 `[Authorize(Policy="AdministratorsOnly")]` 应用于某个 API，则仅 Administrator 角色的用户能访问它。

第二个 <xref:Microsoft.AspNetCore.Authorization.AuthorizationOptions.AddPolicy%2A> 调用演示要求应该为用户提供特定声明所使用的一种简单方法。 <xref:Microsoft.AspNetCore.Authorization.AuthorizationPolicyBuilder.RequireClaim%2A> 方法还可选择为声明采用期望值。 如果指定了值，则仅当用户同时拥有正确类型的声明和某个指定值时才满足要求。 如果使用 JWT 持有者身份验证中间件，则所有 JWT 属性都可用作用户声明。

本文所示的最有趣的策略是在第三个 `AddPolicy` 方法中，因为它使用了自定义授权要求。 通过使用自定义授权要求，可极大地控制授权的执行方式。 为此，必须实现这些类型：

- 从 <xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> 派生并包含指定要求的详细信息的字段的要求类型。 在此示例中，这是用于示例 `MinimumAgeRequirement` 类型的年龄字段。

- 实现 <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandler%601> 的处理程序，其中 T 是处理程序可以满足的 <xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> 类型。 处理程序必须实现 <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandler%601.HandleRequirementAsync%2A> 方法，该方法检查包含用户相关信息的指定上下文是否满足要求。

如果用户满足要求，对 `context.Succeed` 的调用将指示该用户已授权。 如果用户可能有多种方式满足授权要求，则可创建多个处理程序。

除使用 `AddPolicy` 调用注册自定义策略要求外，还需要通过依赖项注入注册自定义要求处理程序 (`services.AddTransient<IAuthorizationHandler, MinimumAgeHandler>()`)。

ASP.NET Core [授权文档](https://docs.asp.net/en/latest/security/authorization/policies.html) 提供了用于（基于 `DateOfBirth` 声明）检查用户年龄的自定义授权要求和处理程序的示例。

## <a name="additional-resources"></a>其他资源

- **ASP.NET Core 身份验证** \
  [https://docs.microsoft.com/aspnet/core/security/authentication/identity](/aspnet/core/security/authentication/identity)

- **ASP.NET Core 授权** \
  [https://docs.microsoft.com/aspnet/core/security/authorization/introduction](/aspnet/core/security/authorization/introduction)

- **基于角色的授权** \
  [https://docs.microsoft.com/aspnet/core/security/authorization/roles](/aspnet/core/security/authorization/roles)

- **基于自定义策略的授权** \
  [https://docs.microsoft.com/aspnet/core/security/authorization/policies](/aspnet/core/security/authorization/policies)

>[!div class="step-by-step"]
>[上一页](index.md)
>[下一页](developer-app-secrets-storage.md)
