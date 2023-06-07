---
ms.openlocfilehash: 05aec429e28ef74515ef6988d5b064e6d16b7c1b
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032428"
---
### <a name="signalr-handshakeprotocolsuccesshandshakedata-replaced"></a>SignalR：HandshakeProtocol.SuccessHandshakeData 已替换

已删除 [HandshakeProtocol.SuccessHandshakeData](https://github.com/dotnet/aspnetcore/blob/c5b2bc0df2a0027832bf7d01dfb19ca39cd08ae6/src/SignalR/common/SignalR.Common/src/Protocol/HandshakeProtocol.cs#L27) 字段，并将其替换为一个 Helper 方法，该方法根据特定 `IHubProtocol` 生成成功的握手响应。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

`HandshakeProtocol.SuccessHandshakeData` 是 `public static ReadOnlyMemory<byte>` 字段。

#### <a name="new-behavior"></a>新行为

`HandshakeProtocol.SuccessHandshakeData` 已被 `static` `GetSuccessfulHandshake(IHubProtocol protocol)` 方法替换，该方法根据指定的协议返回 `ReadOnlyMemory<byte>`。

#### <a name="reason-for-change"></a>更改原因

握手响应  中添加了其他字段，这些字段是非常量，并会根据所选协议的不同而变化。

#### <a name="recommended-action"></a>建议操作

无。 此类型不适用于从用户代码使用。 它是 `public` 类型，因此可以在 SignalR 服务器和客户端之间共享。 它还可以由以 .NET 编写的客户 SignalR 客户端使用。 SignalR 用户不应受此更改的影响  。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.AspNetCore.SignalR.Protocol.HandshakeProtocol.SuccessHandshakeData?displayProperty=nameWithType>

<!--

#### Affected APIs

`F:Microsoft.AspNetCore.SignalR.Protocol.HandshakeProtocol.SuccessHandshakeData`

-->
