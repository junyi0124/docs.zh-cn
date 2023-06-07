---
ms.openlocfilehash: ed5a90063b8963d79f412ec1c5c0b9030f980f83
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032279"
---
### <a name="identity-signinmanager-constructor-accepts-new-parameter"></a>标识：SignInManager 构造函数接受新参数

从 ASP.NET Core 3.0 开始，新的 `IUserConfirmation<TUser>` 参数添加到了 `SignInManager` 构造函数中。 有关详细信息，请参阅 [dotnet/aspnetcore#8356](https://github.com/dotnet/aspnetcore/issues/8356)。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="reason-for-change"></a>更改原因

更改的动机是为了添加对标识中的新电子邮件/确认流的支持。

#### <a name="recommended-action"></a>建议操作

如果手动构造 `SignInManager`，请提供 `IUserConfirmation` 的实现，或从依赖项注入获取一个实现来提供。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.AspNetCore.Identity.SignInManager%601.%23ctor%2A>

<!--

#### Affected APIs

`Overload:Microsoft.AspNetCore.Identity.SignInManager`1.#ctor`

-->
