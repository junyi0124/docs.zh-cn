---
ms.openlocfilehash: a4e20e0468d861138ad801c9dbfa15340b3f388c
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032258"
---
### <a name="authentication-oauthhandler-exchangecodeasync-signature-changed"></a>身份验证：已更改 OAuthHandler ExchangeCodeAsync 签名

在 ASP.NET Core 3.0 中，`OAuthHandler.ExchangeCodeAsync` 的签名已从以下位置更改：

```csharp
protected virtual System.Threading.Tasks.Task<Microsoft.AspNetCore.Authentication.OAuth.OAuthTokenResponse> ExchangeCodeAsync(string code, string redirectUri) { throw null; }
```

到:

```csharp
protected virtual System.Threading.Tasks.Task<Microsoft.AspNetCore.Authentication.OAuth.OAuthTokenResponse> ExchangeCodeAsync(Microsoft.AspNetCore.Authentication.OAuth.OAuthCodeExchangeContext context) { throw null; }
```

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

`code` 和 `redirectUri` 字符串作为单独的参数传递。

#### <a name="new-behavior"></a>新行为

`Code` 和 `RedirectUri` 是 `OAuthCodeExchangeContext` 上的属性，可通过 `OAuthCodeExchangeContext` 构造函数进行设置。 新的 `OAuthCodeExchangeContext` 类型是传递到 `OAuthHandler.ExchangeCodeAsync` 的唯一参数。

#### <a name="reason-for-change"></a>更改原因

此更改允许以非中断方式提供其他参数。 无需创建新的 `ExchangeCodeAsync` 重载。

#### <a name="recommended-action"></a>建议操作

使用适当的 `code` 和 `redirectUri` 值构造 `OAuthCodeExchangeContext`。 必须提供 <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties> 实例。 此单个 `OAuthCodeExchangeContext` 实例可传递到 `OAuthHandler.ExchangeCodeAsync` 而不是多个参数。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthHandler%601.ExchangeCodeAsync(System.String,System.String)?displayProperty=nameWithType>

<!--

#### Affected APIs

`M:Microsoft.AspNetCore.Authentication.OAuth.OAuthHandler`1.ExchangeCodeAsync(System.String,System.String)`

-->
