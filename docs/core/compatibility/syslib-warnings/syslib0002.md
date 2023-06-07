---
title: SYSLIB0002 错误
description: 了解有关产生编译时错误 SYSLIB0002 的过时信息。
ms.topic: reference
ms.date: 10/20/2020
ms.openlocfilehash: 774675e96d11a8e1b5d82767695d0defd1e8cfad
ms.sourcegitcommit: e301979e3049ce412d19b094c60ed95b316a8f8c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/16/2020
ms.locfileid: "97596278"
---
# <a name="syslib0002-principalpermissionattribute-is-obsolete"></a>SYSLIB0002：PrincipalPermissionAttribute 已过时

从 .NET 5.0 开始，<xref:System.Security.Permissions.PrincipalPermissionAttribute> 构造函数已过时并产生编译时错误 `SYSLIB0002`。 不能实例化此属性或将其应用于方法。

与其他过时警告不同，无法禁止显示此错误。

## <a name="workarounds"></a>工作区

- 如果要将属性应用于 ASP.NET MVC 操作方法，请执行以下操作：

  考虑使用 ASP.NET 的内置授权基础结构。 以下代码演示如何使用 <xref:System.Web.Mvc.AuthorizeAttribute> 属性来为控制器添加批注。 ASP.NET 运行时将在执行操作之前向用户授权。

  ```csharp
  using Microsoft.AspNetCore.Authorization;

  namespace MySampleApp
  {
      [Authorize(Roles = "Administrator")]
      public class AdministrationController : Controller
      {
          public ActionResult MyAction()
          {
              // This code won't run unless the current user
              // is in the 'Administrator' role.
          }
      }
  }
  ```

  有关详细信息，请参阅 [ASP.NET Core 中基于角色的授权](/aspnet/core/security/authorization/roles)和 [ASP.NET Core 中的授权简介](/aspnet/core/security/authorization/introduction)。

- 如果要将属性应用到 Web 应用上下文之外的库代码，请执行以下操作：

  通过调用 <xref:System.Security.Principal.IPrincipal.IsInRole(System.String)?displayProperty=nameWithType> 方法，在方法开始时手动执行检查。

  ```csharp
  using System.Threading;

  void DoSomething()
  {
      if (Thread.CurrentPrincipal == null
          || !Thread.CurrentPrincipal.IsInRole("Administrators"))
      {
          throw new Exception("User is anonymous or isn't an admin.");
      }

      // Code that should run only when user is an administrator.
  }
  ```

[!INCLUDE [suppress-syslib-warning](../../../../includes/suppress-syslib-warning.md)]

## <a name="see-also"></a>另请参阅

- [PrincipalPermissionAttribute 已过时，报告为错误](../core-libraries/5.0/principalpermissionattribute-obsolete.md)
