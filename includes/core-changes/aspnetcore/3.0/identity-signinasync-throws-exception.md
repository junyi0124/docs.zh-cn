---
ms.openlocfilehash: 6679e38aefa7d61ce430dc5375ff3b35c641ea27
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032277"
---
### <a name="identity-signinasync-throws-exception-for-unauthenticated-identity"></a>标识：对于未经身份验证的标识，SignInAsync 会引发异常

默认情况下，`SignInAsync` 会为其中 `IsAuthenticated` 为 `false` 的主体/标识引发异常。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

`SignInAsync` 接受任何主体/标识，包括其中 `IsAuthenticated` 为 `false` 的标识。

#### <a name="new-behavior"></a>新行为

默认情况下，`SignInAsync` 会为其中 `IsAuthenticated` 为 `false` 的主体/标识引发异常。 有一个新的标志可禁止显示此行为，但默认行为已更改。

#### <a name="reason-for-change"></a>更改原因

旧行为是有问题的，因为在默认情况下，`[Authorize]` / `RequireAuthenticatedUser()` 拒绝了这些主体。

#### <a name="recommended-action"></a>建议操作

在 ASP.NET Core 3.0 预览版 6 中，`AuthenticationOptions` 上有 `RequireAuthenticatedSignIn` 标记，默认为 `true`。 将此标志设置为 `false` 以还原旧行为。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

None

<!-- 

#### Affected APIs

Not detectable via API analysis

-->
