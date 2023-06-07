---
ms.openlocfilehash: b0e1d6d720a1c9b827fb4585606e64b545d395d7
ms.sourcegitcommit: 0802ac583585110022beb6af8ea0b39188b77c43
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/25/2020
ms.locfileid: "96032285"
---
### <a name="kestrel-request-trailer-headers-moved-to-new-collection"></a>Kestrel：请求尾部标头已移动到新集合

在以前的版本中，当读取请求正文直到结尾时，Kestrel 会将 HTTP/1.1 分块尾部标头添加到请求标头集合中。 此行为将引发对标头和尾部之间存在不明确性的担忧。 因此，做出了将尾部移动到新集合的决定。

HTTP/2 请求尾部在 ASP.NET Core 2.2 中不可用，但现在可用于 ASP.NET Core 3.0 的此新集合。

添加了新的请求扩展方法以访问这些尾部。

读取整个请求正文后，HTTP/1.1 尾部即可用。

从客户端收到 HTTP/2 尾部后，这些尾部即可用。 客户端将不会发生尾部，直到整个请求正文至少已由服务器缓冲。 可能需要读取请求正文，以释放缓冲区空间。 如果读取请求正文直到结尾，则尾部始终可用。 尾部标记正文的结尾。

#### <a name="version-introduced"></a>引入的版本

3.0

#### <a name="old-behavior"></a>旧行为

请求尾部标头将添加到 `HttpRequest.Headers` 集合中。

#### <a name="new-behavior"></a>新行为

请求尾部标头在  `HttpRequest.Headers` 集合中不存在。 在 `HttpRequest` 上使用以下扩展方法来访问它们：

- `GetDeclaredTrailers()` - 获取列出了正文后应具有的尾部的请求“尾部”标头。
- `SupportsTrailers()` - 指示请求是否支持接收尾部标头。
- `CheckTrailersAvailable()` - 确定请求是否支持尾部以及是否可读取。
- `GetTrailer(string trailerName)` - 从响应获取请求的尾随标头。

#### <a name="reason-for-change"></a>更改原因

在 gRPC 等方案中，尾部是关键功能。 将尾部合并到请求标头会使用户感到困惑。

#### <a name="recommended-action"></a>建议操作

在 `HttpRequest` 上使用尾部相关扩展方法访问尾部。

#### <a name="category"></a>类别

ASP.NET Core

#### <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.AspNetCore.Http.HttpRequest.Headers?displayProperty=nameWithType>

<!--

#### Affected APIs

`P:Microsoft.AspNetCore.Http.HttpRequest.Headers`

-->
