---
ms.openlocfilehash: de06825f1031d05bc83183a83bae165e2f9512ff
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032421"
---
### <a name="signalr-hubconnection-resetsendping-and-resettimeout-methods-removed"></a>SignalR：已删除 HubConnection ResetSendPing 和 ResetTimeout 方法

已从 SignalR `HubConnection` API 中删除了 `ResetSendPing` 和 `ResetTimeout` 方法。 这些方法最初仅供内部使用，但已在 ASP.NET Core 2.2 中公开。 从 ASP.NET Core 3.0 预览版 4 开始，这些方法将不可用。 有关讨论，请参阅 [dotnet/aspnetcore#8543](https://github.com/dotnet/aspnetcore/issues/8543)。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

API 可用。

#### <a name="new-behavior"></a>新行为

API 已删除。

#### <a name="reason-for-change"></a>更改原因

这些方法最初仅供内部使用，但已在 ASP.NET Core 2.2 中公开。

#### <a name="recommended-action"></a>建议操作

不要使用这些方法。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

- <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.ResetSendPing?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.ResetTimeout?displayProperty=nameWithType>

<!--

#### Affected APIs

- `M:Microsoft.AspNetCore.SignalR.Client.HubConnection.ResetSendPing`
- `M:Microsoft.AspNetCore.SignalR.Client.HubConnection.ResetTimeout`

-->
