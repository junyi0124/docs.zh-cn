---
ms.openlocfilehash: f202b39f1a45f740625827be25e72df0e403d605
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032422"
---
### <a name="signalr-javascript-client-package-name-changed"></a>SignalR：已更改 JavaScript 客户端包名称

在 ASP.NET Core 3.0 预览版 7 中，SignalR JavaScript 客户端包名称从 `@aspnet/signalr` 更改为 `@microsoft/signalr`。 由于 Azure SignalR 服务，名称更改反映了 SignalR 不只是在 ASP.NET Core 应用中有用这一事实。

若要对此更改做出反应，请更改 package.json  文件、`require` 语句和 ECMAScript `import` 语句中的引用。 在此重命名过程中，不会更改 API。

有关讨论，请参阅 [dotnet/aspnetcore#11637](https://github.com/dotnet/aspnetcore/issues/11637)。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

客户端包以前命名为 `@aspnet/signalr`。

#### <a name="new-behavior"></a>新行为

客户端包现在命名为 `@microsoft/signalr`。

#### <a name="reason-for-change"></a>更改原因

由于 Azure SignalR 服务，名称更改阐明了 SignalR 在 ASP.NET Core 应用之外也很有用。

#### <a name="recommended-action"></a>建议操作

切换到新包 `@microsoft/signalr`。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

None

<!-- 

#### Affected APIs

Not detectable via API analysis

-->
